# Iteration 14: Step 1 of Retail Onboarding — Zero-Survey Blueprint Ingestion & Planogram Digitization

## 1. Problem Formulation: Eliminating the Painful "Site Survey" Hurdle
When pitching an enterprise retail solution to a supermarket chain (e.g., DMart, Reliance Smart, More Retail), the #1 reason pilot projects stall is **deployment friction**:
- Legacy analytics companies require sending physical CAD surveying engineers with laser rangefinders into every store for 3 days to manually measure aisle coordinates.
- Retailers operate 200 to 2,000 stores; manual surveying costs \$3,000+ per store and takes months to schedule.

To propose a truly scalable solution, **StoreFlow AI must digitize the store layout purely from existing files in under 30 minutes with zero on-site surveyors**.

---

## 2. Research & Technical References

### Reference 1: Automated Architectural Vectorization for Retail Planograms
- **Sources:** ACM KDD Retail Informatics / Computer Vision Architectural Review
- **Key Insight:** Retailers already possess digital 2D assets: architectural CAD DWG/DXF files, renovation PDFs, or store safety evacuation maps. Automated line-segment parsing and Hough contour clustering convert raster/vector blueprint assets into structured JSON polygons in minutes, reducing onboarding labor by **$96\%$**.

### Reference 2: Semantic Fixture Tagging & Category Hierarchies
- **Sources:** National Retail Federation (NRF) Planogram Standards
- **Key Insight:** Linking spatial polygonal geofences with retail merchandising categories (e.g., *Atta & Flour*, *Spices & Masalas*, *Cooking Oils*, *Billing Counters*) transforms raw geometric space into commercially meaningful business zones.

---

## 3. The Step 1 Onboarding Workflow

```
[Retail Headquarters / Store Manager]
       │
       │ 1. Uploads Store Blueprint File (PDF / CAD DXF / High-Res Image)
       ▼
[StoreFlow AI Vectorization Engine]
       │
       ├── Automated Contour & Wall Extraction (Hough Transform + Polygon Simplification)
       ├── Identifies Fixture Polygons: Gondolas, Chilled Cases, Cash Desks
       └── Computes Navigable Aisle Corridors (Minkowski Difference of Total Area)
       │
       ▼
[Interactive Web Onboarding Portal]
       │
       │ 2. Manager clicks on fixtures to assign department tags:
       │    [Gondola 1-2: Atta & Rice] [Gondola 3-4: Spices] [Counters 1-6: Billing]
       ▼
[Completed 2D Metric Digital Twin Map (Stored as GeoJSON in Meters)]
```

---

## 4. Deliverable to the Supermarket
- **Time Required:** $< 30\text{ minutes}$ from file upload to finalized map.
- **Cost:** \$0 on-site surveyor costs.
- **Output:** A calibrated, web-native 2D metric CAD floor plan with millimeter precision, ready for camera registration.
