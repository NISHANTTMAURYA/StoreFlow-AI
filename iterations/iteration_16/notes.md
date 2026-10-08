# Iteration 16: Step 3 of Retail Onboarding — Plug-and-Play Edge Appliance Provisioning & Non-Invasive Topology

## 1. Problem Formulation: Overcoming Enterprise IT & Security Roadblocks
Supermarket Enterprise IT and Security teams reject third-party IoT deployments due to three core fears:
1. **Bandwidth Hogging:** Will video analytics slow down POS billing terminals and credit card payment gateways?
2. **Cybersecurity Vulnerabilities:** Will installing a device on the store LAN create backdoors for hackers to access financial data?
3. **Operational Downtime:** Will installing hardware disrupt security NVR recording or cause loss of CCTV footage?

Our proposal must provide **ironclad technical proof of zero IT disruption and bank-grade network security**.

---

## 2. Research & Technical References

### Reference 1: Non-Invasive Passive Video Tap & SPAN Port Architecture
- **Sources:** Enterprise Network Security & CCTV Best Practices (Cisco / Verkada Deployment Whitepapers)
- **Key Insight:** The edge appliance connects passively via a single RJ-45 Ethernet cable to an unused port on the CCTV network switch. Using standard RTSP unicast sub-streams or switch port mirroring (SPAN), the edge device reads video frames *in parallel* with the existing NVR. If the edge device is unplugged or powered down, the existing NVR continues recording uninterrupted.

### Reference 2: Zero-Trust Outbound-Only Telemetry Architecture
- **Sources:** ISO 27001 / SOC 2 Enterprise IoT Security Standards
- **Key Insight:** The edge appliance requires **Zero Inbound Firewall Ports (0 open listening ports)**. It connects outbound only to the StoreFlow cloud telemetry broker via MQTT over TLS 1.3 (Port 8883). Total store network usage is capped at $< 3.5\text{ KB/second}$, making it completely invisible on the store's broadband or cellular backup connection.

---

## 3. The Step 3 Onboarding Workflow

```
[Supermarket Server Rack / CCTV NVR Room]
       │
       │ Store clerk unpacks the StoreFlow Edge Box (Size of a small paperback book)
       │
       ├── 1. Connects 1 standard Ethernet cable to the CCTV PoE Switch
       └── 2. Plugs power adapter into standard 220V wall socket (12W power draw)
       │
       ▼
[Zero-Touch Boot Sequence (< 60 Seconds)]
       │
       ├── Auto-acquires local IP via DHCP on CCTV VLAN
       ├── Establishes outbound TLS 1.3 encrypted handshake with StoreFlow Cloud
       ├── Spins up containerized GStreamer zero-copy ingestion pipelines
       └── Starts INT8 YOLOv10-Nano + OC-SORT inference on local RAM
```

---

## 4. Deliverable to the Supermarket
- **Physical Installation Time:** $< 10\text{ minutes}$ (can be performed by an on-duty store clerk without IT technicians).
- **Network Impact:** Zero inbound attack surface; $3.25\text{ KB/s}$ bandwidth consumption ($< 0.01\%$ of standard store bandwidth).
- **Power Consumption:** 12 Watts (costs $\approx ₹100/\text{month}$ in electricity).
- **Operational Risk:** Zero. NVR video recording and POS billing are completely unaffected.
