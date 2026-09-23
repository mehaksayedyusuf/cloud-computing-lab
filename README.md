# Cloud Computing Laboratory Repository

This repository contains practical laboratory experiments, benchmarks, reports, and performance analyses conducted as part of the **Cloud Computing Laboratory** course.

---

## Student Details

| Field | Details |
| :--- | :--- |
| **Name** | Mehak Sayed Yusuf |
| **USN** | 01FE24BCI012 |
| **Division** | B |
| **Roll No.** | 202 |

---

## Repository Overview

The objective of this laboratory repository is to systematically document virtualized infrastructure deployments, cloud architectures, hypervisor evaluations, performance benchmarking, and resource utilization monitoring.

### Laboratory Index

| Experiment # | Experiment Title | Hypervisors / Platforms | Status | Link |
| :---: | :--- | :--- | :---: | :--- |
| **01** | **Performance Analysis of Type-1 and Type-2 Hypervisors** | Proxmox VE (Type-1) & VMware Workstation (Type-2) | Completed | [Experiment 01](./Experiment-01-Hypervisor-Performance/) |
| *02* | *Upcoming Experiment* | *TBD* | Planned | — |

---

## Repository Structure

```text
cloud_computing_lab/
│
├── Experiment-01-Hypervisor-Performance/
│   ├── Type-1-Proxmox/
│   │   ├── screenshots/
│   │   │   ├── fig01_hostnamectl.jpeg
│   │   │   ├── fig02_lscpu.jpeg
│   │   │   ├── ...
│   │   │   └── README.md
│   │   ├── type 1.pdf
│   │   └── README.md
│   │
│   ├── Type-2-VMware/
│   │   ├── screenshots/
│   │   │   ├── fig01_hostnamectl.jpeg
│   │   │   ├── fig02_lscpu_part1.jpeg
│   │   │   ├── ...
│   │   │   └── README.md
│   │   ├── type 2.pdf
│   │   └── README.md
│   │
│   ├── Comparison/
│   │   └── README.md
│   │
│   ├── results/
│   │   └── performance-analysis.md
│   │
│   └── README.md
│
├── type 1.pdf
├── type 2.pdf
├── .gitignore
└── README.md
```

---

## Lab Technologies & Toolstack

- **Type-1 Bare-Metal Hypervisor:** Proxmox Virtual Environment (PVE) with KVM/QEMU kernel virtualization
- **Type-2 Hosted Hypervisor:** VMware Workstation running on top of a host operating system
- **Guest Operating System:** Ubuntu 22.04.5 LTS (x86_64)
- **Benchmarking Suite:** Sysbench (CPU prime-number computation benchmark)
- **Monitoring & Introspection Tools:** `hostnamectl`, `lscpu`, `free -h`, `df -h`, `top`, and Proxmox VE Web Management Dashboard (RRD graphs for CPU, RAM, Network, Disk I/O)

---

## Guidelines for Navigation

- Navigate to [Experiment-01-Hypervisor-Performance](./Experiment-01-Hypervisor-Performance/) for the detailed methodology, benchmark commands, and findings.
- Check [results/performance-analysis.md](./Experiment-01-Hypervisor-Performance/results/performance-analysis.md) for direct side-by-side metric comparison tables.
- Visit [Comparison/README.md](./Experiment-01-Hypervisor-Performance/Comparison/README.md) for architectural overhead evaluations and trade-offs.
