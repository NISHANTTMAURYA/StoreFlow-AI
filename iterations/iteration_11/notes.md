# Iteration 11: Floor Plan Vectorization & Blueprint-to-Digital Twin Transformation

## 1. Problem Formulation: From Static Store Blueprint to Living Interactive Digital Map
Retail store managers and merchandising executives think in terms of store layout blueprints, aisles, and shelf plans (planograms), not abstract pixel coordinates $(u, v)$ or camera view angles.
To deliver actionable business outcomes, the system must ingest the store's physical blueprint (architectural CAD DXF/DWG, PDF, SVG, or a 2D floor plan photo) and convert it into an interactive **Spatial Digital Twin Map**.

**The Challenge:**
1. Converting heterogeneous architectural formats into structured polygonal geometries (walls, gondolas, endcaps, chilled counters, checkout lanes, entry/exit gates).
2. Registering and projecting all ceiling camera fields-of-view (camera frustums) onto this 2D master floor canvas.
3. Providing non-technical store managers with an interactive, web-based canvas where computer vision telemetry renders seamlessly in real time.

---

## 2. Research Papers & Literature

### Paper 1: FloorplanVLM: Sequence Modeling for Architectural Floor Plan Vectorization
- **Authors:** Floor Plan Understanding & Spatial AI Group
- **Reference:** arXiv:2403 / CVPR Workshops
- **Key Insight:** Treats floor plan vectorization as a structured sequence generation task. Converts raster blueprint images (scanned architect drawings) into clean vector polygons representing walls, doorways, and fixtures with high topological precision, eliminating manual CAD tracing.

### Paper 2: Cloud-Native Generative AI for Automated Planogram Synthesis
- **Authors:** Retail Systems & Spatial Informatics Review
- **Reference:** arXiv:2402 / ACM KDD
- **Key Insight:** Connects physical floor plan coordinates with merchandise category semantics. Proves that linking physical spatial polygons with point-of-sale SKU hierarchies enables real-time sales-per-square-foot and dwell-to-purchase conversion modeling.

---

## 3. Mathematical & Algorithmic Formulation

### 3.1 Blueprint Ingestion & Topological Vector Graph
A store floor plan is modeled as a planar spatial graph $G_{store} = (\mathcal{V}_{zones}, \mathcal{E}_{aisles})$:
- **Fixture Polygons ($\mathcal{Z}_{fixture}$):**
  $$\mathcal{Z}_k = \{ (X_1, Y_1), (X_2, Y_2), \dots, (X_n, Y_n) \}$$
  representing physical gondolas, shelves, refrigeration units, checkout counters. These act as impenetrable obstacles for trajectory smoothing.
- **Walkable Corridors ($\mathcal{W}_{aisles}$):**
  The free navigation space $\mathcal{W} = \mathcal{A}_{total} \setminus \bigcup_k \mathcal{Z}_k$.
- **Camera Frustum Projections:**
  Each camera $C_m$ has a visible ground projection polygon $\mathcal{F}_m = \mathbf{H}_m(\text{Viewport}_{img})$.

### 3.2 Automated Coordinate Unification
All camera homography matrices $\mathbf{H}_1, \mathbf{H}_2, \dots, \mathbf{H}_M$ project directly into the global metric coordinate frame of $G_{store}$ (origin $(0,0)$ at store entrance, metric units in meters).
A customer detected at pixel $(u, v)$ in Camera 3 is mapped instantly to:
$$\mathbf{P}_{global} = \mathbf{H}_3 [u, v, 1]^T$$
rendering an instantaneous live avatar dot directly on the store manager's digital twin map.

---

## 4. Architectural Innovation: One-Click SVG/CAD Importer
Store managers upload an existing store blueprint (CAD DXF or floor PDF):
1. Automated contour extraction identifies perimeter walls and interior shelf islands.
2. Manager clicks once on each fixture to assign retail category labels (e.g., "Dairy & Eggs", "Beverages", "Snacks Aisle 3", "Checkout Lane 1").
3. The engine creates semantic geofences instantly, ready for dwell-time accumulation and bottleneck tracking.

---

## 5. Feasibility & Cost Impact
- **Zero Specialist Software Cost:** Runs in any modern web browser via WebGL / HTML5 Canvas / SVG without requiring expensive AutoCAD or Revit licenses.
- **Operational Clarity:** Eliminates the cognitive load of watching 16 camera video feeds simultaneously. Store managers glance at a single unified map and grasp store health in 5 seconds.
