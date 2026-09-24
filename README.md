# Performance Analysis of Type-1 and Type-2 Hypervisors

A comparative performance benchmarking study evaluating **Type-1 (Bare-Metal)** and **Type-2 (Hosted)** hypervisors using identical Ubuntu 22.04 LTS virtual machines under standardized `sysbench` CPU computational workloads.

---

## Student Details

| Field | Information |
| :--- | :--- |
| **Name** | Mehak Sayed Yusuf |
| **USN** | 01FE24BCI012 |
| **Division** | B |
| **Roll No.** | 202 |
| **Course** | Cloud Computing Laboratory |

---

## 1. Project Overview & Motivation

Virtualization is the foundational technology underpinning modern cloud computing infrastructure. Hypervisors are classified into two architectural categories:

1. **Type-1 (Bare-Metal) Hypervisors**: Run directly on physical server hardware without an intermediate host operating system. Examples include Proxmox VE (KVM), VMware ESXi, and Microsoft Hyper-V Server.
2. **Type-2 (Hosted) Hypervisors**: Run as an application on top of a conventional host operating system (e.g., Windows, macOS, Linux). Examples include VMware Workstation, Oracle VirtualBox, and Parallels Desktop.

### Primary Objective
To empirically evaluate the CPU performance, computational throughput, latency profiles, and architectural overhead between:
- **Type-1 Hypervisor:** Proxmox VE (Kernel-based Virtual Machine / KVM)
- **Type-2 Hypervisor:** VMware Workstation Pro

---

## 2. Architectural Comparison

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

## 3. Standardized VM Configurations

To ensure a strictly fair and controlled benchmark, both virtual machines were configured with identical resource limits:

| Parameter | Proxmox VE (Type-1) | VMware Workstation (Type-2) |
| :--- | :--- | :--- |
| **Guest OS** | Ubuntu 22.04.5 LTS (x86_64) | Ubuntu 22.04.5 LTS (x86_64) |
| **vCPU Allocation** | 1 socket, 2 cores (**2 vCPU**) | 1 processor, 2 cores (**2 vCPU**) |
| **CPU Model Presented** | QEMU Virtual CPU version 2.5+ | 13th Gen Intel Core i5-13450HX |
| **Memory Allocation** | 2048 MiB (**2 GB RAM**) | 2048 MB (**2 GB RAM**) |
| **Virtual Disk** | **20 GB** Virtual Disk | **20 GB** Virtual Disk |
| **Network Configuration** | Linux Bridge (`vmbr0`) / VirtIO | NAT (`VMnet8`) |
| **Virtualization Engine** | KVM (Full Virtualization) | VMware (Full Virtualization) |

---

## 4. Benchmark Command & Methodology

The CPU benchmarking was conducted using `sysbench` computing prime numbers up to 20,000 using a single worker thread over a 10-second test window:

```bash
# Package update and installation
sudo apt update && sudo apt install sysbench -y

# Verify Sysbench version
sysbench --version

# Execute CPU benchmark
sysbench cpu --cpu-max-prime=20000 run
```

---

## 5. Empirical Results & Console Evidence

### Type-1 Hypervisor (Proxmox VE)

Below is the verified screenshot captured from the Proxmox VE Ubuntu virtual machine:

![Proxmox VE Type-1 Sysbench Result](images/1.png)

*Figure 1: Proxmox VE (Type-1 Hypervisor) Sysbench Benchmark Console Output.*

---

### Type-2 Hypervisor (VMware Workstation)

Below is the verified screenshot captured from the VMware Workstation Ubuntu virtual machine:

![VMware Workstation Type-2 Sysbench Result](images/2.png)

*Figure 2: VMware Workstation (Type-2 Hypervisor) Sysbench Benchmark Console Output.*

---

## 6. Performance Comparison Table

The following table summarizes the exact values recorded from the experimental benchmark runs:

| Performance Metric | Proxmox VE (Type-1) | VMware Workstation (Type-2) | Performance Delta | Winner / Advantage |
| :--- | :---: | :---: | :---: | :---: |
| **Hypervisor Type** | Bare-Metal | Hosted | Architectural | Type-1 Direct Access |
| **Guest OS** | Ubuntu 22.04.5 LTS | Ubuntu 22.04.5 LTS | Matched | Identical Baseline |
| **vCPU Allocation** | 2 vCPU | 2 vCPU | Matched | Identical Compute |
| **RAM Allocation** | 2 GB | 2 GB | Matched | Identical Memory |
| **Disk Capacity** | 20 GB | 20 GB | Matched | Identical Storage |
| **Benchmark Limit** | 20,000 Primes | 20,000 Primes | Matched | Identical Stress Test |
| **Total Execution Time** | **10.0005 s** | **10.0004 s** | 0.0001 s variance | Fixed 10s Window |
| **Total Events Processed** | **15,877** | **14,410** | **+1,467 events (+10.18%)** | **Proxmox VE (Type-1)** |
| **Events per Second (EPS)** | **1,587.47** | **1,440.80** | **+146.67 eps (+10.18%)** | **Proxmox VE (Type-1)** |
| **Minimum Latency** | **0.59 ms** | **0.65 ms** | **-0.06 ms (-9.23%)** | **Proxmox VE (Faster)** |
| **Average Latency** | **0.63 ms** | **0.69 ms** | **-0.06 ms (-8.70%)** | **Proxmox VE (Lower)** |
| **95th Percentile Latency**| **0.65 ms** | **0.90 ms** | **-0.25 ms (-27.78%)** | **Proxmox VE (More Consistent)**|
| **Maximum Latency** | **1.34 ms** | **1.89 ms** | **-0.55 ms (-29.10%)** | **Proxmox VE (Fewer Spikes)** |

---

## 7. Metric Explanations & Visualizations

### Performance Metric Definitions

1. **Total Execution Time (seconds)**: The wall-clock duration of the benchmark run, standardized to ~10 seconds.
2. **Events per Second (Throughput / EPS)**: The number of prime-number computational cycles completed per second (**Higher is better**).
3. **Total Events**: Total number of prime calculation iterations executed during the test duration (**Higher is better**).
4. **Latency (milliseconds)**: Elapsed time per individual computation event (**Lower is better**):
   - **Minimum Latency**: The fastest recorded event execution time.
   - **Average Latency**: The arithmetic mean of all event processing times.
   - **95th Percentile Latency**: The threshold below which 95% of events finished. Critical indicator for tail consistency.
   - **Maximum Latency**: The worst-case event delay, demonstrating scheduler interruption and jitter.

---

### Chart 1: CPU Throughput Comparison (Events / Sec)

![CPU Throughput Comparison](images/events_per_second_comparison.png)

*Figure 3: CPU Throughput comparison showing Proxmox VE (+10.18% faster).*

---

### Chart 2: CPU Latency Metrics Comparison

![Latency Comparison](images/latency_comparison.png)

*Figure 4: Latency metrics (Min, Avg, 95th Percentile, Max) across both hypervisors.*

---

### Chart 3: Total Events Processed

![Total Events Comparison](images/total_events_comparison.png)

*Figure 5: Total Events completed in 10 seconds (15,877 vs 14,410).*

---

### Chart 4: Comprehensive Performance Dashboard

![Overall Performance Dashboard](images/overall_performance_dashboard.png)

*Figure 6: Multi-panel performance evaluation dashboard.*

---

## 8. Technical Analysis & Discussion

The empirical benchmark data demonstrates consistent performance advantages for **Proxmox VE (Type-1)** over **VMware Workstation (Type-2)** in compute-bound workloads.

### 1. Architectural Overhead & Trap-and-Emulate Delays
- **Proxmox VE (Type-1)** leverages Linux KVM, interfacing directly with physical CPU hardware virtualization extensions (Intel VT-x / AMD-V). Virtual machine instructions execute in hardware VMX non-root mode with direct register access and zero user-to-kernel host OS mediation.
- **VMware Workstation (Type-2)** runs as a user-space application mediated by device drivers on the Windows host. Sensitive instructions and I/O requests must pass through VMware's VMM engine, host Windows kernel dispatchers, and hardware abstraction layers.

### 2. CPU Scheduling & Context Switching Latency
- In Proxmox VE, guest vCPUs map directly to host Linux kernel POSIX threads scheduled by the **Completely Fair Scheduler (CFS)** operating at Ring 0.
- In VMware Workstation, guest threads compete against the host operating system's background services (e.g., Windows Defender, background telemetry, and desktop applications), introducing scheduling jitter. This explains VMware's higher peak latency (**1.89 ms vs 1.34 ms**) and higher 95th percentile latency (**0.90 ms vs 0.65 ms**).

### 3. Memory & Virtual Cache Access
- Proxmox VE utilizes hardware-assisted nested paging (EPT / NPT) to translate Guest Physical Addresses directly into Host Physical Addresses.
- Type-2 hypervisors incur double-layer memory mapping overhead ($GPA \rightarrow HVA \rightarrow HPA$) and are subject to host virtual memory paging contention.

---

## 9. Conclusion & Engineering Takeaways

1. **Bare-Metal Superiority**: Proxmox VE (Type-1) delivers **+10.18% higher CPU throughput** and **8.70% lower average latency** compared to VMware Workstation (Type-2).
2. **Predictable Low-Latency Execution**: Proxmox VE exhibits substantially tighter latency distributions (0.65 ms vs 0.90 ms at the 95th percentile, and 29.10% lower peak latency spikes), crucial for latency-sensitive cloud workloads.
3. **Use-Case Recommendations**:
   - **Type-1 (Proxmox VE / KVM / ESXi)**: Best suited for Cloud Data Centers, Enterprise Virtualization, Database Servers, and High-Performance Computing (HPC).
   - **Type-2 (VMware Workstation / VirtualBox)**: Best suited for Local Software Development, Sandboxed Testing, Desktop Virtualization, and Educational Labs.

---

## 10. Repository Structure & Reproduction

### Folder Layout

```text
cloud-computing-lab/
│
├── README.md                                  # Main Project & Benchmark Report
├── LAB_REPORT.md                              # Formal Academic Lab Report Submission
├── type 1.pdf                                 # Original Type-1 Lab Document
├── type 2.pdf                                 # Original Type-2 Lab Document
├── .gitignore                                 # Git Ignore Rules
│
├── images/                                    # Screenshots & Generated Charts
│   ├── 1.png                                  # Proxmox VE Sysbench Result Screenshot
│   ├── 2.png                                  # VMware Workstation Sysbench Result Screenshot
│   ├── events_per_second_comparison.png       # Throughput Comparison Graph
│   ├── latency_comparison.png                 # Latency Metrics Graph
│   ├── total_events_comparison.png            # Total Events Graph
│   ├── overall_performance_dashboard.png      # Multi-panel Dashboard
│   └── ...                                    # Additional System Inspection Evidence
│
└── scripts/                                   # Automation & Plotting Scripts
    ├── benchmark.sh                           # Sysbench Automation Script
    ├── generate_plots.py                      # Matplotlib Visualization Generator
    └── parse_sysbench.py                      # Results Parser & Ratio Calculator
```

### How to Reproduce

1. **Run Benchmark Script on VM**:
   ```bash
   chmod +x scripts/benchmark.sh
   ./scripts/benchmark.sh
   ```

2. **Generate Plots**:
   ```bash
   python scripts/generate_plots.py
   ```

3. **Parse & Compare Results**:
   ```bash
   python scripts/parse_sysbench.py
   ```

---
*Laboratory Experiment conducted for Cloud Computing Laboratory Course.*
