# Performance Analysis: Type-1 (Proxmox VE) vs Type-2 (VMware Workstation) Hypervisors

## Experiment Information

- **Experiment Title:** Performance Analysis of Type-1 and Type-2 Hypervisors
- **Student Name:** Mehak Sayed Yusuf
- **USN:** 01FE24BCI012
- **Division:** B
- **Roll No.:** 202

---

## 1. Primary Comparison Table

The following comparison table presents the exact configuration and Sysbench benchmark results recorded across both hypervisors:

| Parameter | Type-1 Hypervisor (Proxmox VE) | Type-2 Hypervisor (VMware Workstation) | Difference / Impact |
| :--- | :--- | :--- | :--- |
| **Hypervisor** | **Proxmox VE** | **VMware Workstation** | — |
| **Hypervisor Type** | **Type-1 (Bare-Metal)** | **Type-2 (Hosted)** | Architectural distinction |
| **Guest OS** | **Ubuntu 22.04.5 LTS** | **Ubuntu 22.04.5 LTS** | Identical |
| **CPU configuration** | **2 vCPU** (1 socket, 2 cores) | **2 vCPU** (1 processor, 2 cores) | Identical allocation |
| **Memory** | **2 GB** (2048 MiB) | **2 GB** (2048 MB) | Identical allocation |
| **Disk** | **20 GB** | **20 GB** | Identical allocation |
| **Total execution time** | **10.0005 s** | **10.0004 s** | 0.0001 s variance (standard ~10s window) |
| **Total events** | **15,877** | **14,410** | **+1,467 events** (+10.18% in favor of Type-1) |
| **Events per second** | **1,587.47** | **1,440.80** | **+146.67 eps** (+10.18% in favor of Type-1) |
| **Average latency** | **0.63 ms** | **0.69 ms** | **0.06 ms lower** (~8.70% faster responsiveness) |

---

## 2. Extended Benchmark & Latency Profile

In addition to the primary parameters, the Sysbench CPU benchmark (`--cpu-max-prime=20000`) generated detailed latency distribution statistics:

| Latency / Hardware Metric | Proxmox VE (Type-1) | VMware Workstation (Type-2) | Advantage |
| :--- | :--- | :--- | :--- |
| **Minimum Latency** | **0.59 ms** | 0.65 ms | Proxmox VE (-0.06 ms / 9.2% faster) |
| **Average Latency** | **0.63 ms** | 0.69 ms | Proxmox VE (-0.06 ms / 8.7% faster) |
| **Maximum Latency** | **1.34 ms** | 1.89 ms | Proxmox VE (-0.55 ms / 29.1% lower peak) |
| **95th Percentile Latency** | **0.65 ms** | 0.90 ms | Proxmox VE (-0.25 ms / 27.8% lower tail latency) |
| **CPU Model Presented to Guest** | `QEMU Virtual CPU version 2.5+` | `13th Gen Intel Core i5-13450HX` | VMware exposes host model |
| **Virtualization Engine** | KVM / Full virtualization | VMware / Full virtualization | Both support hardware virtualization |
| **Total Memory (free -h)** | ~1.9 GiB | ~1.9 GiB (865 MiB available) | Consistent Linux memory overhead |
| **Disk Storage (df -h)** | 20 GB root filesystem | 20 GB (12 GB used, 6.5 GB available) | Standard root mount |

---

## 3. Detailed Performance Analysis & Calculations

### 3.1 Throughput Gain (Events per Second)
- **Type-1 (Proxmox VE):** $1,587.47\text{ events/sec}$
- **Type-2 (VMware Workstation):** $1,440.80\text{ events/sec}$
- **Delta:** 
  $$\Delta \text{Throughput} = 1587.47 - 1440.80 = +146.67\text{ eps}$$
- **Percentage Improvement:**
  $$\frac{1587.47 - 1440.80}{1440.80} \times 100 = +10.18\%$$
  Proxmox VE completed **1,467 more computation events** within the 10-second test window, delivering a **10.18% throughput superiority**.

### 3.2 Latency and Tail-Latency Analysis
- **Average Latency:** Proxmox VE achieved an average latency of $0.63\text{ ms}$ versus $0.69\text{ ms}$ on VMware Workstation, representing an **8.70% latency reduction**.
- **95th Percentile Latency:** Proxmox VE maintained $0.65\text{ ms}$ compared to $0.90\text{ ms}$ on VMware Workstation. This demonstrates substantially superior tail consistency.
- **Maximum Latency Spike:** VMware Workstation experienced a peak latency spike of $1.89\text{ ms}$ compared to Proxmox VE's $1.34\text{ ms}$ (a **29.1% higher spike on Type-2**). This difference is directly attributable to host OS background scheduling interruptions and context switching overhead.

---

## 4. Architectural Findings & Key Observations

1. **Direct Hardware Execution vs Host Mediation:**
   - Proxmox VE installs directly onto bare-metal hardware. KVM schedules virtual vCPU threads directly into the physical CPU scheduler without an intervening general-purpose OS.
   - VMware Workstation runs as a user/kernel application inside a desktop OS. Virtual CPU execution must contend with host OS background services, Windows kernel interrupts, and the host scheduler.

2. **Jitter and Scheduling Contention:**
   - The tail latency (95th percentile and max latency) disparity clearly reflects the overhead of the Type-2 host operating system. In Type-2 virtualization, background tasks on the host machine intermittently preempt the hypervisor threads.
   - Proxmox VE shows minimal jitter ($0.59\text{ ms}$ min to $1.34\text{ ms}$ max), demonstrating the real-time predictability critical for production cloud and enterprise workloads.

3. **Resource Introspection:**
   - Both hypervisors exposed full hardware-assisted virtualization to the guest Ubuntu operating system.
   - Proxmox VE provided granular infrastructure telemetry directly through its integrated web dashboard (RRD charts for CPU, RAM, network traffic spikes, and disk I/O), whereas VMware Workstation relied primarily on guest-internal utilities (`top`, `lscpu`, `free`, `df`).
