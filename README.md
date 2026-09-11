# High-Density 12-GPU Heterogeneous Compute & Mining Cluster

[![Hardware-Status](https://img.shields.io/badge/Hardware-12x%20GPU%20Array-blue.svg)](#system-architecture--hardware-bom)
[![OS-HiveOS](https://img.shields.io/badge/OS-HiveOS%20Linux%205.10-orange.svg)](https://hiveon.com/os/)
[![Consensus-Ethash](https://img.shields.io/badge/Algorithm-Ethash%20PoW-green.svg)](#software-optimization--firmware-tuning)
[![Throughput](https://img.shields.io/badge/Hashrate-~394%20MH%2Fs-brightgreen.svg)](#telemetry--operational-results)
[![Electrical](https://img.shields.io/badge/Electrical-20A%20%2F%2012AWG%20Dedicated-red.svg)](#electrical-infrastructure--power-engineering)

An end-to-end engineering project detailing the automated procurement, low-level firmware flashing, Lite Hash Rate (LHR) kernel tuning, custom 20A electrical infrastructure, and operational deployment of a 12-GPU heterogeneous parallel compute rig during the 2021–2022 Ethereum Proof-of-Work (PoW) cycle.

---

## Table of Contents
- [Project Overview](#project-overview)
- [System Architecture & Hardware BOM](#system-architecture--hardware-bom)
- [Automated GPU Procurement](#automated-gpu-procurement)
- [Software Optimization & Firmware Tuning](#software-optimization--firmware-tuning)
  - [Custom VBIOS Flashing (AMD)](#custom-vbios-flashing-amd)
  - [Algorithmic LHR Unlock (NVIDIA Ampere)](#algorithmic-lhr-unlock-nvidia-ampere)
  - [Driver & Kernel Clock Profiles](#driver--kernel-clock-profiles)
- [Electrical Infrastructure & Power Engineering](#electrical-infrastructure--power-engineering)
- [Telemetry & Operational Results](#telemetry--operational-results)
- [Repository Layout](#repository-layout)
- [Setup & BIOS Configuration Guide](#setup--bios-configuration-guide)
- [Authors & Acknowledgments](#authors--acknowledgments)

---

## Project Overview

Prior to Ethereum's transition to Proof-of-Stake ("The Merge"), verifying blocks under the memory-hard Ethash hashing algorithm offered a high-yield application for distributed compute systems. However, decentralized solo validation suffered from extreme statistical variance, making pooled mining essential for stable payouts.

This project tackled the engineering bottlenecks of operating a multi-tier, 12-GPU heterogeneous cluster:
1. **Supply Chain Scarcity:** Building automated real-time restock monitors to acquire GPUs at MSRP during global semiconductor shortages.
2. **Firmware Constraints:** Modifying AMD VBIOS memory straps and configuring dynamic LHR bypass kernels on NVIDIA Ampere cards.
3. **Power Limitations:** Resolving standard residential 15A branch circuit trips by engineering a dedicated 20A line backed by 12 AWG copper wiring and an enterprise-grade rack-mount UPS.

---

## System Architecture & Hardware BOM

The rig utilizes an open-air dual-layer steel frame designed for optimal convective and forced cross-flow heat dissipation. Compute hardware spans three microarchitectures across two manufacturers:

| Index | Graphics Card Model | Architecture | VRAM Configuration | Nominal Hashrate | Optimization Profile |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **GPU 0** | NVIDIA GeForce RTX 3070 Ti | Ampere (8nm) | 8 GB GDDR6X (Micron) | ~40.50 MH/s | LHR Unlock / PL 310W |
| **GPU 1** | NVIDIA GeForce RTX 3070 | Ampere (8nm) | 8 GB GDDR6 (Samsung) | ~31.36 MH/s | Undervolted / PL 240W |
| **GPU 2** | NVIDIA GeForce RTX 3070 Ti | Ampere (8nm) | 8 GB GDDR6X (Micron) | ~39.69 MH/s | LHR Unlock / PL 310W |
| **GPU 3** | NVIDIA GeForce RTX 3070 Ti | Ampere (8nm) | 8 GB GDDR6X (Micron) | ~38.99 MH/s | LHR Unlock / PL 310W |
| **GPU 4** | NVIDIA GeForce RTX 3080 | Ampere (8nm) | 10 GB GDDR6X (Micron) | ~45.94–100 MH/s | Thermal Pad Mod / PL 340W |
| **GPU 5** | NVIDIA GeForce RTX 3070 | Ampere (8nm) | 8 GB GDDR6 (Samsung) | ~30.71 MH/s | Undervolted / PL 240W |
| **GPU 6** | NVIDIA GeForce RTX 3070 | Ampere (8nm) | 8 GB GDDR6 (Samsung) | ~31.00 MH/s | Undervolted / PL 240W |
| **GPU 7** | AMD Radeon RX 5700 XT | RDNA 1 (7nm) | 8 GB GDDR6 (Samsung/Micron)| ~52.50 MH/s | Custom VBIOS Strap Mod |
| **GPU 8** | NVIDIA GeForce RTX 2060 | Turing (12nm) | 6 GB GDDR6 | ~28.50 MH/s | Locked Core Clock / Mem OC |
| **GPU 9** | NVIDIA GeForce RTX 2060 | Turing (12nm) | 6 GB GDDR6 | ~28.50 MH/s | Locked Core Clock / Mem OC |
| **GPU 10**| NVIDIA GeForce RTX 2070 SUPER| Turing (12nm) | 8 GB GDDR6 | ~41.50 MH/s | Power Limited / Mem OC |
| **GPU 11**| NVIDIA GeForce RTX 2080 | Turing (12nm) | 8 GB GDDR6 | ~40.00 MH/s | Power Limited / Mem OC |

### Core Platform Infrastructure
- **Motherboard:** Multi-PCIe Mining Mainboard with onboard Intel HD Graphics 510 host display
- **Risers:** 12x Powered USB 3.0 PCIe x1-to-x16 riser cards (6-pin PCIe powered)
- **Power Delivery:**
  - Primary: EVGA 1600W SuperNOVA G2 (80+ Gold)
  - Secondary: Auxiliary 850W High-Efficiency ATX PSU linked via dual-PSU synchronous relay
- **Power Protection:** Line-Interactive Server-Rack Uninterruptible Power Supply (UPS)
- **Cooling:** 6x 120mm high-static pressure fans mounted across the GPU bracket

---
