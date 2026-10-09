# StoreFlow AI

> **"Waze for the Inside of a Store"**  
> *Dwell-Time & Bottleneck Mapping Engine for Physical Retail*  
> **"We don't track customers. We track the store."**  
> Target Submission: IIT Bombay Technical Hackathon

---

## 📌 Executive Overview
StoreFlow AI turns existing standard retail CCTV/RTSP security cameras into an intelligent, real-time spatial analytics engine running on an ultra-low-cost on-prem edge appliance ($130 Mini-PC / $499 Jetson). 

### The Core Operating Loop: Observe → Diagnose → Prescribe → Verify
1. **Observe:** Ingest standard commodity CCTV feeds, extract anonymous ground-contact trajectories, and build real-time metric heatmaps.
2. **Diagnose:** Detect localized bottlenecks, queue spillovers, flow drop-offs, and under-utilized dead zones.
3. **Prescribe:** Translate spatial choke points into concrete, rule-based operational action cards and layout recommendations.
4. **Verify (Physical A/B Testing):** Compare post-intervention metrics against historical baselines to prove whether layout alterations delivered measurable ROI.

---

## 🚀 Key Technical Pillars & Differentiators

- **The Store Graph Mental Model:** Represents the retail store as an interconnected spatial network where aisles are roads, sections are destinations, and customer movement is fluid flow.
- **Store Friction Score (0–100):** A single executive RAG metric per zone (**0–20 🟢 Healthy** | **20–40 🟡 Watch** | **40–70 🟠 Friction** | **70–100 🔴 Critical**) combining density deviations, velocity deceleration, dwell anomalies, and queue persistence.
- **Physics-Based Bottleneck Detection:** Continuum fluid mechanics divergence ($\nabla \cdot \vec{\mathbf{v}} < 0$) with group-deflection filtering and Minimum Spanning Tree (MST) queue overflow vectors.
- **Shelf Micro-Dwell Engine:** Savitzky-Golay polynomial smoothing ($W=7, p=2$) with 1.0m Voronoi shelf frontages to eliminate passage contamination and calculate the **Engagement-to-Passby Ratio (EPR)**.
- **Dead-Zone Discovery & Flow Drop-Off:** Ergodic Markov chain transition matrix ($\mathbf{T}_{ij}$) with steady-state distribution $\boldsymbol{\pi}$ and Spatial Opportunity Scores ($\text{SOS} > 75$).
- **Physical Layout A/B Testing:** Empirically verifies whether moving fixtures or changing displays cured congestion and lifted basket conversion.
- **Edge-Native Low-Cost Hardware:** 100% on-prem video inference on a **\$130 Intel N100** or **\$499 Jetson Orin Nano**. Streams lightweight JSON telemetry ($3.25\text{ KB/s}$, a $14,000\times$ bandwidth reduction) over MQTT/TLS. Total Year 1 TCO is **\$601/yr** (>97% savings vs legacy).
- **100% Zero-PII Privacy-by-Design:** Volatile DMA RAM processing (zero disk writes), top 25% facial masking, ephemeral tokens purged upon store exit. Full statutory compliance with **India DPDP Act 2023** and **EU GDPR Art 9**.

---

## 📂 Key Project Artifacts

- **[Master Technical Specification (solution.md)](solution.md)** — Comprehensive architecture: mathematical formulations, edge-native design, tiered database schemas (PostgreSQL, Redis, ClickHouse), and the 7-Phase MVP build roadmap.
- **[Pitch Deck Specification (slides.md)](slides.md)** — 8-Slide executive and technical pitch deck specification for slide generator agents and hackathon presentations.
- **[Foundational Product Blueprint (vinish.md)](vinish.md)** — The product framing, operating loop, database schemas, and product mental model.
- **[Architectural Iterations Log (iterations.md)](iterations.md)** — Complete 19-cycle engineering evolution tracking academic literature, benchmarking, and optimization.
- **[Interactive Deck (Light Mode)](presentation_light.html)** — Clean corporate light-mode HTML executive presentation.
- **[Interactive Deck (Dark Mode)](presentation.html)** — Premium interactive 18-slide HTML presentation.
- **[Deck Automation Scripts](create_presentation_deck.py)** — Python automation scripts to generate PowerPoint presentations (`.pptx`).

---

## 👥 Contributors & Collaborators
- [@NISHANTTMAURYA](https://github.com/NISHANTTMAURYA)
- [@yashpunmiya](https://github.com/yashpunmiya)
- [@Vinish-Rexson](https://github.com/Vinish-Rexson)
- [@Abhi-engg](https://github.com/Abhi-engg)

