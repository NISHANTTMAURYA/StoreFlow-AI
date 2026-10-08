# Problem Statement: Dwell-Time & Bottleneck Mapper

## 1. Official Problem Definition
> **"Design and build an intelligent computer vision solution that processes standard in-store camera feeds to map customer journeys, generate real-time foot-traffic heat maps, calculate precise section-by-section dwell times, and automatically flag layout bottlenecks or dead zones."**

### Background & Context
Physical retail stores struggle to measure which store sections attract long browsing times versus areas that block traffic or are ignored completely. Optimizing a store's layout, promotional display placement, and traffic flow directly impacts revenue and customer satisfaction. By utilizing existing, standard in-store security cameras (CCTV/RTSP), computer vision can unlock deep operational insights, transforming raw video feeds into actionable heat maps and flow metrics.

---

## 2. Core Functional Requirements
1. **Standard In-Store Video Feed Ingestion:**
   - Ingest multiple asynchronous RTSP/ONVIF CCTV streams (varying resolutions: 720p/1080p, variable frame rates 10–30 FPS, ceiling mounted oblique angles).
2. **Customer Journey Mapping:**
   - Track shoppers seamlessly across disjoint and overlapping camera views (Multi-Target Multi-Camera Tracking / MTMC).
   - Reconstruct continuous 2D planar trajectories onto the store blueprint/floor plan.
3. **Real-Time Foot-Traffic Heatmaps:**
   - Accumulate spatial-temporal density representations (Kernel Density Estimation / Gaussian splatting onto the metric floor plan).
   - Temporal slicing (hourly, daily, promotional event windows).
4. **Precise Section-by-Section Dwell Time Calculation:**
   - Define arbitrary polygonal zones (e.g., Aisle 3, Endcap B, Checkout, Promo Island).
   - Distinguish transit motion (walking through) from true browsing/engagement (stationary or micro-movement within interaction radius).
   - Attribute dwell times accurately to individual customer journeys.
5. **Automated Bottleneck & Dead-Zone Detection:**
   - **Bottleneck Detection:** Detect localized congestion, flow blockages, queue spillovers, or aisle choking points based on velocity divergence, crowd density, and duration thresholds.
   - **Dead-Zone Flagging:** Automatically identify shelf sections, gondolas, or perimeter aisles with statistical under-utilization, low discovery rate, and zero engagement.

---

## 3. Engineering & Operational Constraints
- **Hardware & Cost Efficiency:** Must leverage *existing legacy security cameras* (no expensive LiDAR, 3D stereo-rigs, or ceiling-mounted depth sensors). Edge inference must run on commodity low-cost hardware (e.g., NVIDIA Jetson Orin Nano, low-power Mini-PCs with Intel OpenVINO, or single consumer GPU per 8–16 streams).
- **Privacy & Regulatory Compliance (Zero-PII):** Full adherence to privacy regulations (EU GDPR, Indian Digital Personal Data Protection Act 2023). No facial recognition, biometric identity storage, or persistent cross-visit tracking. Embeddings must be ephemeral and purged upon store exit.
- **Occlusion & Perspective Challenges:** Severe occlusion in crowded narrow retail aisles, tall shelving units, fish-eye distortion, variable overhead lighting, and oblique camera vantage angles.
- **Latency & Scalability:** Real-time edge filtering (< 200ms latency for anomaly/bottleneck alerting) with lightweight telemetry synchronization to cloud/local dashboards.
