# Experiment 01: Performance Analysis of Type-1 and Type-2 Hypervisors

## Student Information

- **Name:** Mehak Sayed Yusuf
- **USN:** 01FE24BCI012
- **Division:** B
- **Roll No.:** 202

---

## 1. Aim

To create and verify a virtual machine on both a **Type-1 hypervisor (Proxmox VE)** and a **Type-2 hypervisor (VMware Workstation)**, inspect its CPU, memory, and disk configuration, monitor resource utilization, and measure CPU performance using **Sysbench**.

---

## 2. Hypervisors Under Evaluation

### Type-1 Hypervisor: Proxmox VE (Bare-Metal)
- **Architecture:** Proxmox Virtual Environment (PVE) is a Type-1 bare-metal hypervisor based on Debian Linux that integrates Kernel-based Virtual Machine (KVM) and QEMU.
- **Hardware Interaction:** It runs directly on the underlying server hardware without an intervening host operating system layer. Virtual machines execute instructions natively on the physical CPU through hardware-assisted virtualization extensions (Intel VT-x / AMD-V).
- **Management & Monitoring:** Includes a centralized web-based management interface providing real-time and historical RRD metrics for CPU load, RAM allocation, network throughput, and disk I/O.
- **Network Architecture:** Employs Linux bridge networking (`vmbr0`) mapped directly to physical interfaces.

### Type-2 Hypervisor: VMware Workstation (Hosted)
- **Architecture:** VMware Workstation is a Type-2 hosted hypervisor that operates as an application layer on top of a primary host operating system (e.g., Windows 11).
- **Hardware Interaction:** Guest VM operations and hardware requests are mediated through the host operating system's kernel, scheduler, and device drivers before reaching physical hardware.
- **Management & Monitoring:** Management is provided via a desktop client application. Resource inspection within the guest is performed via standard Linux introspection utilities (`top`, `lscpu`, `free`, `df`).
- **Network Architecture:** Commonly utilizes Network Address Translation (NAT) or host-only adapters managed by the host OS virtual network editor.

---

## 3. Common Virtual Machine Configuration

To ensure a fair and consistent baseline for comparative benchmarking, identical virtual resource limits were configured across both hypervisors:

| Parameter | Configured Value (Proxmox VE) | Configured Value (VMware Workstation) |
| :--- | :--- | :--- |
| **Guest OS** | Ubuntu 22.04.5 LTS (x86_64) | Ubuntu 22.04.5 LTS (x86_64) |
| **Virtual CPU Allocation** | 1 socket, 2 cores = **2 vCPU** | 1 processor, 2 cores = **2 vCPU** |
| **Memory Allocation** | 2048 MiB (**2 GB RAM**) | 2048 MB (**2 GB RAM**) |
| **Virtual Disk Capacity** | **20 GB** virtual disk | **20 GB** virtual disk |
| **Network Configuration** | Bridged (`vmbr0`) / VirtIO | NAT (`vmnet8`) |

---

## 4. Sysbench Benchmark Command

The CPU benchmarking across both virtual machines was executed using the standard `sysbench` suite:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

### Benchmark Mechanics:
- **Test Mode:** `cpu`
- **Workload:** Calculation of prime numbers up to `20000` via repeated division.
- **Worker Threads:** Default 1 worker thread.
- **Execution Target:** Sustained prime calculation over a standard 10-second sampling window.
- **Key Metrics Captured:**
  - **Total execution time (s):** Wall-clock duration of the test run.
  - **Total events:** Number of prime calculation loops completed.
  - **Events per second (eps):** Primary throughput indicator.
  - **Latency statistics (min, avg, max, 95th percentile in ms):** Responsiveness per event.

---

## 5. Experiment Repository Structure

```text
Experiment-01-Hypervisor-Performance/
│
├── Type-1-Proxmox/
│   ├── screenshots/              # Authentic screenshots captured during Part 1
│   │   ├── fig01_hostnamectl.jpeg
│   │   ├── fig02_lscpu.jpeg
│   │   ├── fig03_lscpu_extended.jpeg
│   │   ├── fig04_free_memory.jpeg
│   │   ├── fig05_free_df_top.jpeg
│   │   ├── fig06_sysbench_cpu_benchmark.jpeg
│   │   ├── fig07_proxmox_cpu_memory_graphs.jpeg
│   │   ├── fig08_proxmox_vm_summary.jpeg
│   │   ├── fig09_proxmox_memory_graph.jpeg
│   │   ├── fig10_proxmox_network_graph.jpeg
│   │   ├── fig11_proxmox_disk_io_graph.jpeg
│   │   └── README.md
│   ├── type 1.pdf                 # Completed lab report document for Part 1
│   └── README.md                  # Comprehensive documentation, procedure & observations for Type-1
│
├── Type-2-VMware/
│   ├── screenshots/              # Authentic screenshots captured during Part 2
│   │   ├── fig01_hostnamectl.jpeg
│   │   ├── fig02_lscpu_part1.jpeg
│   │   ├── fig02_lscpu_part2.jpeg
│   │   ├── fig03_free_memory.jpeg
│   │   ├── fig04_df_disk.jpeg
│   │   ├── fig05_top_processes.jpeg
│   │   ├── fig06_sysbench_install_part1.jpeg
│   │   ├── fig06_sysbench_install_part2.jpeg
│   │   ├── fig07_sysbench_cpu_benchmark.jpeg
│   │   └── README.md
│   ├── type 2.pdf                 # Completed lab report document for Part 2
│   └── README.md                  # Comprehensive documentation, procedure & observations for Type-2
│
├── Comparison/
│   └── README.md                  # Architectural comparison, latency/throughput analysis & trade-offs
│
├── results/
│   └── performance-analysis.md    # Consolidated comparison tables & analytical metrics
│
└── README.md                      # Experiment overview, configuration & command guide (this file)
```

---

## 6. Summary of Experimental Results

| Metric | Type-1: Proxmox VE | Type-2: VMware Workstation | Performance Winner |
| :--- | :---: | :---: | :---: |
| **Total Events (10s)** | **15,877** | 14,410 | **Proxmox VE (+10.18%)** |
| **Events per Second** | **1,587.47** | 1,440.80 | **Proxmox VE (+10.18%)** |
| **Average Latency** | **0.63 ms** | 0.69 ms | **Proxmox VE (8.7% lower latency)** |
| **Minimum Latency** | **0.59 ms** | 0.65 ms | **Proxmox VE** |
| **Maximum Latency** | **1.34 ms** | 1.89 ms | **Proxmox VE (29.1% lower peak latency)** |

For detailed breakdowns, refer to:
- [Type-1 Proxmox VE Detailed Documentation](./Type-1-Proxmox/README.md)
- [Type-2 VMware Workstation Detailed Documentation](./Type-2-VMware/README.md)
- [Performance Analysis and Metric Tables](./results/performance-analysis.md)
- [Architectural Comparison & Deep Dive](./Comparison/README.md)
