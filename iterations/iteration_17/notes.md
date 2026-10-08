# Iteration 17: Step 4 of Retail Onboarding — 48-Hour Baseline Learning, POS Fusion & Go-Live

## 1. Problem Formulation: Accelerating Time-to-Value (TTV)
A common downfall of enterprise enterprise AI software is protracted implementation cycles (taking 3 to 6 months before showing any business results).
Supermarket executives demand rapid proof of value:
- How quickly does the system produce actionable insights after installation?
- How does the system adapt to the unique customer walking habits of this specific store format?
- How does spatial dwell time translate into actual revenue metrics?

StoreFlow AI guarantees a **guaranteed 48-hour Go-Live milestone with zero store downtime**.

---

## 2. Research & Technical References

### Reference 1: Unsupervised Self-Calibrating Spatial Baselines
- **Sources:** IEEE Transactions on Cybernetics / Retail Systems Evaluation
- **Key Insight:** Retail foot-traffic distributions follow predictable statistical diurnal cycles (morning lull, afternoon peak, evening rush). Running unsupervised background tracking for 24 to 48 hours establishes empirical baseline velocity distributions $P(v)$, normal queue lengths $\mathcal{N}(\mu_{queue}, \sigma_{queue})$, and average section dwell times without manual threshold tuning.

### Reference 2: Basket-to-Dwell Correlation Fusion
- **Sources:** arXiv:2601.00928 (Analyzing the Shopping Journey: Computing Shelf Browsing Visits)
- **Key Insight:** Anonymously correlating checkout register timestamps with exit trajectory tokens enables the system to compute the **Engagement-to-Purchase Conversion Factor**:
  $$\text{Conversion Yield}(Z_k) = \frac{\text{Transactions with SKU } \in Z_k}{\text{Unique Shoppers Dwelling } \ge 10\text{s in } Z_k}$$
  proving the exact rupee revenue generated per minute of customer dwell time.

---

## 3. The Step 4 Onboarding Workflow

```
[Day 1 (Hour 0 - 24): Silent Background Learning]
       │
       ├── Edge box tracks real customer flow silently
       ├── Refines camera homographies on-the-fly using trajectory velocities
       ├── Learns store-specific normal velocity baselines: v_free = 1.18 m/s
       └── Identifies natural peak hours and quiet periods
       │
       ▼
[Day 2 (Hour 24 - 48): Automated Threshold Calibration]
       │
       ├── Calibrates Bottleneck Severity Index (BSI) trigger limits for checkout lanes
       ├── Calculates baseline Discovery Rates D(k) across all 50+ store zones
       └── Connects optional POS billing API (matches timestamps with basket conversions)
       │
       ▼
[Hour 48: Manager Go-Live & Executive Handover]
       │
       ├── Store Manager logs into Light-Mode Spatial Decision Cockpit on Tablet/PC
       ├── Instant Access: Real-time Gaussian heatmaps, live customer trajectory traces
       └── First Automated Action Reports Delivered:
           - "Register 3 queue bottleneck alert dispatched"
           - "Aisle 7 Spices flagged as dead zone: Relocate Amul display for +₹1.2L/mo"
```

---

## 4. Deliverable to the Supermarket
- **Time-to-Value:** Complete operational go-live in **$< 48\text{ hours}$**.
- **Management Effort:** Zero manual data entry; automated calibration.
- **ROI Impact:** Immediate queue reduction and quantified layout optimization from Day 2 onwards.
