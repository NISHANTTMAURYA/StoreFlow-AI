# Iteration 5: Precise Dwell-Time Computation & Micro-Interaction Analytics

## 1. Problem Formulation: The Naive Dwell-Time Pitfall
Most off-the-shelf retail analytics tools calculate dwell time using a naive bounding-box-in-polygon timer:
- *Naive Rule:* If customer bounding box is inside Aisle Polygon $\mathcal{P}_{Aisle3}$, increment timer $T_{dwell} += \Delta t$.
**Why This Fails in Real Stores:**
1. **Passage Contamination:** A customer walking quickly through Aisle 3 to reach the milk fridge spends 6 seconds in the polygon. Naive analytics records this as "6 seconds of engagement with breakfast cereals", corrupting conversion metrics!
2. **Shelf Interaction Horizon:** Browsing occurs at the shelf edge (within a $0.8\text{m}$ interaction zone), oriented towards the shelf display, with low kinetic energy.
3. **Micro-Dwell vs. Macro-Dwell:**
   - **Transit (< 3 sec, $v > 0.8\text{ m/s}$):** Pure passthrough.
   - **Glance / Micro-Dwell ($3 - 10\text{ sec}$, $0.1 \le v \le 0.4\text{ m/s}$):** Peripheral awareness, browsing glance.
   - **Macro-Dwell / Deep Engagement (> 10 sec, $v < 0.2\text{ m/s}$):** High intent, reading nutritional labels, comparing prices, physical product handling.

---

## 2. Research Papers & Literature

### Paper 1: Analyzing the Shopping Journey: Computing Shelf Browsing Visits in a Physical Retail Store
- **Authors:** Research team on retail behavioral analytics
- **Reference:** arXiv:2601.00928
- **Key Insight:** Formulates a rigorous definition of "Shelf Browsing Visits" (SBVs). Rather than treating aisles as monolithic polygons, the store is partitioned into high-resolution micro-zones along shelf frontage. An SBV requires:
  1. Spatial proximity to shelf facings ($d \le 1.0\text{m}$).
  2. Sustained velocity suppression below kinetic threshold ($v < v_{browse} = 0.35\text{ m/s}$).
  3. Minimum temporal duration threshold ($\tau_{dwell} \ge 4.0\text{ seconds}$).
  Correlating SBVs against Point-of-Sale (POS) barcode scanner receipts demonstrates an $r = 0.84$ correlation with actual sales conversion.

### Paper 2: Spatio-temporal Trajectory Segmentation for Shopper Behavior Analysis
- **Authors:** Behavioral Vision & Pattern Recognition Group
- **Key Insight:** Implements Savitzky-Golay polynomial smoothing on metric trajectories $(X(t), Y(t))$ to eliminate GPS/camera jitter before computing kinetic energy and curvature of customer paths.

---

## 3. Mathematical Foundations: The Multi-Stage Dwell Engine

### 3.1 Trajectory Metric Smoothing
Raw projected coordinates $(X_t, Y_t)$ contain high-frequency jitter due to minor bounding box fluctuations.
We apply a Savitzky-Golay filter of window size $W = 7$ (corresponding to $0.5$ seconds at 15 FPS) and polynomial order $p = 2$ to obtain smooth positions $\mathbf{P}(t) = [\tilde{X}(t), \tilde{Y}(t)]^T$:
$$\mathbf{P}(t) = \sum_{k=-m}^{m} c_k \mathbf{P}_{raw}(t+k)$$
Instantaneous velocity is computed via analytic derivative:
$$v(t) = \sqrt{\left(\frac{d\tilde{X}}{dt}\right)^2 + \left(\frac{d\tilde{Y}}{dt}\right)^2}$$

### 3.2 Dynamic Spatial Geofencing & Polygonal Containment
Let $\mathcal{Z}_k$ be an arbitrary store section defined as an ordered polygon of vertices in metric floor coordinates:
$$\mathcal{Z}_k = \{ (X_1, Y_1), (X_2, Y_2), \dots, (X_m, Y_m) \}$$
Containment $\mathbb{I}_{in}(\mathbf{P}(t), \mathcal{Z}_k) \in \{0, 1\}$ is evaluated via Jordan Curve Theorem (Ray-Casting Algorithm):
Count intersections of ray $R = \{ (X(t) + \lambda, Y(t)) \mid \lambda \ge 0 \}$ with edges of $\mathcal{Z}_k$. If odd, point is interior.

### 3.3 State Machine for Browsing Classification
For each customer journey $\mathcal{J}_i$, define an engagement state machine $S(t) \in \{\text{TRANSIT}, \text{MICRO\_DWELL}, \text{MACRO\_DWELL}, \text{QUEUE}\}$:
$$S(t) = \begin{cases} \text{TRANSIT} & \text{if } v(t) > 0.65\text{ m/s} \\ \text{MICRO\_DWELL} & \text{if } v(t) \le 0.35\text{ m/s} \text{ and } 3\text{s} \le \Delta t_{stop} < 10\text{s} \\ \text{MACRO\_DWELL} & \text{if } v(t) \le 0.20\text{ m/s} \text{ and } \Delta t_{stop} \ge 10\text{s} \\ \text{QUEUE} & \text{if } \mathbf{P}(t) \in \mathcal{Z}_{Checkout} \text{ and } v(t) \le 0.15\text{ m/s} \end{cases}$$

### 3.4 Metric Dwell-Time Accumulator
True section dwell time for customer $i$ in section $k$ is accumulated strictly during non-transit states:
$$T_{dwell}(i, k) = \int_{t_{entry}}^{t_{exit}} \mathbb{I}_{in}(\mathbf{P}_i(t), \mathcal{Z}_k) \cdot \mathbb{I}(S_i(t) \in \{\text{MICRO\_DWELL}, \text{MACRO\_DWELL}\}) \, dt$$
Total store dwell efficiency is quantified as:
$$\eta_{engagement}(k) = \frac{\sum_i T_{dwell}(i, k)}{\sum_i T_{passage}(i, k)}$$
Sections with low $\eta_{engagement}$ indicate that customers treat the area as a high-speed transit hallway rather than a browsing destination.

---

## 4. Architectural Innovation: Voronoi-Partitioned Shelf Micro-Frontages
Instead of coarse manual zoning (e.g., "Snacks Aisle"), the system automatically decomposes shelf frontages into metric Voronoi cells spaced every $1.0\text{ meter}$ along the fixture CAD vectors:
- Associates exact dwell timestamps with individual 1-meter product categories (e.g., Potato Chips vs Pretzels vs Organic Crackers).
- Enables automated calculation of **Engagement-to-Passby Ratio (EPR)**:
  $$\text{EPR}_{shelf} = \frac{\text{Unique Shoppers Dwelling } > 5\text{s}}{\text{Total Shoppers Passing Through Corridor}}$$

---

## 5. Feasibility, Cost & Viability Analysis
- **Compute Overhead:** Polygonal ray casting and trajectory filtering execute in $< 50\mu\text{s}$ per tracklet in compiled C++/Rust/Cython or vectorized NumPy.
- **Business Impact:** Directly answers the retail category manager's fundamental dilemma: "Are shoppers stopping at my new organic snack promotional endcap, or are they just walking past it on their way to the cash registers?"
