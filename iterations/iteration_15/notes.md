# Iteration 15: Step 2 of Retail Onboarding — Automated CCTV Network Discovery & Camera-to-Map Registration

## 1. Problem Formulation: Non-Invasive Camera Calibration
In physical supermarkets:
- Security cameras are already installed on high ceilings (3.5m to 5.0m elevation).
- Drilling, re-aiming, or physical camera modifications are strictly forbidden by supermarket facility management and security policies.
- Cameras are diverse (Hikvision, Dahua, CP Plus, Axis, Honeywell) connected via local PoE network switches.

The proposal must demonstrate that **StoreFlow AI discovers and maps every existing camera feed onto the store blueprint automatically without touching the physical cameras**.

---

## 2. Research & Technical References

### Reference 1: ONVIF WS-Discovery & Zero-Conf Network Protocols
- **Sources:** ONVIF Standard Specification (Profile S/T)
- **Key Insight:** Standard IP surveillance cameras broadcast multicast probe messages (WS-Discovery on `239.255.255.250:3702`). The edge appliance listens on the local CCTV VLAN subnet, identifying camera IP addresses, MAC addresses, resolutions, and RTSP stream endpoints in $< 45\text{ seconds}$ with zero manual IP typing.

### Reference 2: Semi-Automated Ground-Plane Homography Registration
- **Sources:** arXiv:2609.18582 (On-the-Fly Homographies Calibration for Multi-Camera Tracking)
- **Key Insight:** After discovering camera feeds, the onboarding UI presents a split-screen tool:
  - On the left: Camera live preview.
  - On the right: Digitized store blueprint.
  - The technician or manager clicks 4 corresponding landmark points (e.g., Aisle 3 front-left corner, front-right corner, rear-left corner, rear-right corner).
  - The system computes initial planar homography $\mathbf{H}_m$ via Direct Linear Transformation (DLT).
  - An online background Kalman filter refines $\mathbf{H}_m$ continuously using walking trajectory speeds.

---

## 3. The Step 2 Onboarding Workflow

```
[Local Store PoE Network Switch]
       │
       │ (ONVIF WS-Discovery Scan on Subnet)
       ▼
[StoreFlow Discovery Agent]
       ├── Discovers 12 IP Cameras across Store Subnet
       ├── Probes H.264/H.265 RTSP Stream URLs (Sub-stream 720p / Main-stream 1080p)
       └── Tests Video Stream Health & Jitter
       │
       ▼
[Interactive Web Calibration Tool (Split-Screen)]
       │
       │ 3-Minute Assisted Landmark Alignment:
       │ User clicks 4 fixture corners on Camera View <───► Matches on Blueprint Map
       ▼
[Initial Homography Matrix H_m Generated for All Cameras]
       │
       └── Auto-Calculates Camera Coverage Frustums (Field-of-View Polygons)
       └── Highlights Overlapping Zones & Blind Corridors on Store Blueprint
```

---

## 4. Deliverable to the Supermarket
- **Time Required:** $< 15\text{ minutes}$ for a 12-camera store.
- **Physical Impact:** Zero ladders, zero ceiling wiring, zero physical camera contact.
- **Outcome:** Every camera is mathematically anchored to real-world floor coordinates in meters.
