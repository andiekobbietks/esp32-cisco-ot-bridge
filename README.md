# ESP32 + Cisco OT/IT Bridge: Industry 4.0 Edge Computing

## Origin

This project grew out of a conversation about the Weight-Space Programming (WSP) findings. The key insight from WSP was that frontier LLMs compile logic into sparse integer matrices (95%+ zeros) that can run on bare metal with sub-millisecond latency. The natural question: where does that matter?

The answer was Industry 4.0. The original 2011 vision promised smart factories with intelligent sensors on every machine. Instead, the industry got "Pilot Purgatory": cloud-dependent sensors, no runtime on microcontrollers, and OT/IT security walls. Factories run on 30-year-old PLCs with 1970s ladder logic. You cannot deploy Python or PyTorch on a $5 microcontroller.

This project bridges that gap: use LLMs offline to compile industrial safety rules and anomaly detection into tiny sparse matrices, flash them onto $8 ESP32-S3 chips, and connect them to real Cisco industrial networking infrastructure (CML-Free, IOL switches, ASAv firewalls) through legacy protocols (Modbus, PROFINET, OPC UA). The result is air-gapped, sub-millisecond edge intelligence that talks to 30-year-old machinery without replacing it.

The Cisco Networking Academy Industrial OT track (Industrial Networking Essentials, IIoT Control Systems, Industrial Cybersecurity) provides the protocol knowledge. The ESP32 provides the hardware. The WSP research provides the compilation technique.

## What is this?

A proof-of-concept project bridging a physical $8 ESP32-S3 microcontroller to real Cisco industrial networking infrastructure. The goal: demonstrate that bare-metal edge AI (compiled logic in sparse integer matrices) can run on microcontrollers and communicate securely through legacy industrial protocols (Modbus, PROFINET, OPC UA) into modern SCADA/IoT dashboards.

This project connects three threads:
1. **Weight-Space Programming**: frontier LLMs compiling logic into sparse integer matrices (95%+ zeros)
2. **Edge LLM benchmark**: proving on-device inference works on consumer hardware (40 tok/s on a 2021 laptop)
3. **Cisco OT/IT convergence**: bridging legacy factory protocols to modern IP networks securely

## Architecture

```
[Physical $8 ESP32-S3 Sensor]
        │ (Wi-Fi / Serial)
        ▼
[Home Network / VMware Bridged Adapter]
        │
        ▼
[CML-Free: External Connector] ──► [Cisco IOL-L2 Switch (VLAN Segmentation)]
        │
        ▼
[Cisco ASAv Firewall (IEC 62443 ACLs)]
        │
        ▼
[MQTT / SCADA Server / Cloud Dashboard]
```

## Components

### `/firmware`
Lean C code running on the ESP32-S3 (based on Karpathy's `llama2.c` blueprint). Bare-metal, no OS, no Python runtime. Compiles sparse integer matrices for sub-millisecond sensor evaluation.

### `/cml-topology`
Cisco Modeling Labs Free topology exported as YAML, plus running-configs for the IOL-L2 switch and ASAv firewall. Demonstrates VLAN segmentation, ACLs, and protocol bridging.

### `/proof`
Screenshots and logs showing:
- ESP32 serial monitor sending telemetry
- Cisco switch CLI (`show access-lists`, `show vlan brief`) proving traffic passes cleanly
- Unauthorized ports blocked

## Why this matters

The original Industry 4.0 vision (2011, Hannover Messe) promised smart factories where every sensor is an autonomous intelligent agent. Instead, the industry got "Pilot Purgatory": cloud-dependent sensors, no runtime on microcontrollers, and OT/IT security walls.

This project demonstrates the path forward:
- **Zero-runtime execution**: sparse integer matrices run on bare metal in microseconds
- **Air-gapped autonomy**: sensors evaluate locally, only sending alerts when thresholds breach
- **Legacy protocol bridging**: Modbus/PROFINET/OPC UA into modern SCADA without replacing 30-year-old machinery

## Hardware

- ESP32-S3 with 16 MB flash + 8 MB PSRAM (N16R8 variant, ~$8)
- Cisco CML-Free (free, runs in VMware, up to 5 IOS nodes)
- OR Cisco DevNet Sandbox (free cloud-hosted Cisco gear)

## Learning path

1. Cisco Networking Academy: Industrial Networking Essentials (free, 35 hours)
2. Cisco Networking Academy: Industrial IoT and Control Systems (free, 15 hours)
3. Cisco Networking Academy: Industrial Cybersecurity Essentials (free, 25 hours)
4. Hands-on: ESP32-S3 + CML-Free + real sensor data

## Related repositories

- [weight-space-programming](https://github.com/andiekobbietks/weight-space-programming): the research behind compiling logic into sparse matrices
- [edge-llm-benchmark-vivobook](https://github.com/andiekobbietks/edge-llm-benchmark-vivobook): edge LLM inference benchmark (40 tok/s on consumer hardware)
- [ai-hype-auditing](https://github.com/andiekobbietks/ai-hype-auditing): forensic methodology for auditing AI claims
- [sovereign-edge-ai-research](https://github.com/andiekobbietks/sovereign-edge-ai-research): research portfolio connecting all threads

## License

Project code: MIT. Cisco configurations and topologies: for educational use. Hardware: see manufacturer licenses.