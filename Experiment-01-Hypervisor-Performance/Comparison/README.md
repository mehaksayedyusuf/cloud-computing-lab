# Architectural Comparison: Type-1 (Proxmox VE) vs Type-2 (VMware Workstation)

## Overview

Virtualization technologies are categorized fundamentally by the layer at which the hypervisor (Virtual Machine Monitor / VMM) interacts with the underlying hardware:
- **Type-1 (Bare-Metal Hypervisors):** Run directly on the bare-metal hardware.
- **Type-2 (Hosted Hypervisors):** Run as software applications on top of a conventional host operating system.

This document examines the structural and operational distinctions between **Proxmox VE (Type-1)** and **VMware Workstation (Type-2)** in light of the experimental benchmarks obtained.

---

## 1. Architectural Stack Comparison

```text
┌──────────────────────────────────────┐     ┌──────────────────────────────────────┐
│        Type-1: Proxmox VE            │     │     Type-2: VMware Workstation       │
├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│  [Guest OS: Ubuntu 22.04.5 LTS]      │     │  [Guest OS: Ubuntu 22.04.5 LTS]      │
│  [Sysbench CPU Workload]             │     │  [Sysbench CPU Workload]             │
├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│  Virtual Hardware Layer (KVM/QEMU)   │     │  VMware Virtual Hardware Layer       │
├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│  Proxmox VE Hypervisor Core (Debian) │     │  VMware Workstation Application      │
├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│                 ──                   │     │  Host Operating System (Windows 11)  │
├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│       Physical Bare-Metal Hardware   │     │       Physical Bare-Metal Hardware   │
└──────────────────────────────────────┘     └──────────────────────────────────────┘
```

---

## 2. Key Architectural Differences

| Attribute | Type-1: Proxmox VE | Type-2: VMware Workstation |
| :--- | :--- | :--- |
| **Execution Layer** | Direct bare-metal installation | Application running on top of Windows/Linux host |
| **CPU Scheduling** | KVM schedules vCPU threads directly onto physical CPU cores | Host OS scheduler multiplexes hypervisor threads alongside desktop applications |
| **Memory Management** | Direct page allocation; zero host OS memory contention | Guest RAM is allocated from host OS virtual memory; subject to host swapping |
| **Storage Subsystem** | Direct LVM-thin / ZFS block storage devices | Virtual disk files (`.vmdk`) residing on host NTFS/ext4 filesystem |
| **Networking** | Direct Linux kernel bridging (`vmbr0`) | Host software network adapter (NAT / Virtual Network Switch) |
| **Context Switching Overhead** | Minimal (Guest $\leftrightarrow$ Hypervisor/Hardware) | Doubled (Guest $\leftrightarrow$ VMware VMM $\leftrightarrow$ Host Kernel $\leftrightarrow$ Hardware) |

---

## 3. Experimental Metric Correlation

The benchmark numbers recorded in this experiment reflect these architectural differences:

| Measured Metric | Proxmox VE (Type-1) | VMware Workstation (Type-2) | Architectural Explanation |
| :--- | :---: | :---: | :--- |
| **CPU Throughput** | **1,587.47 eps** | 1,440.80 eps | Proxmox delivers **+10.18%** higher throughput because CPU instructions execute without host OS context-switching penalties. |
| **Average Latency** | **0.63 ms** | 0.69 ms | Shorter instruction path and direct hardware virtualization traps reduce round-trip execution latency by **~8.7%**. |
| **Tail Latency (95th %)** | **0.65 ms** | 0.90 ms | VMware experiences greater variance (+38.5% tail latency) due to host OS background threads interrupting guest vCPU execution. |
| **Maximum Latency** | **1.34 ms** | 1.89 ms | Host OS system interrupts, background desktop services, and scheduler preemption cause higher peak latency spikes in Type-2. |

---

## 4. Trade-Off and Use Case Matrix

| Evaluation Criteria | Type-1 (Proxmox VE) | Type-2 (VMware Workstation) |
| :--- | :--- | :--- |
| **Raw Compute Performance** | **Optimal** (lowest overhead, near-native speeds) | Moderate (host abstraction penalty) |
| **Deterministic Latency** | **High** (consistent response times) | Moderate (vulnerable to host workload spikes) |
| **Ease of Setup** | Requires dedicated server hardware or partition | Simple one-click desktop application install |
| **Hardware Flexibility** | Limited to server configurations | Runs on any standard laptop or workstation |
| **Multi-Tenancy & Clustering** | Native support for clustering, Ceph, and live migration | Limited to local workstation execution |
| **Target Deployment** | Production clouds, enterprise data centers, homelabs | Software engineering, sandboxing, malware analysis, student labs |
