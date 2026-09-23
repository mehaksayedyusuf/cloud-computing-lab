# Type-1 (Proxmox VE) Experimental Evidence & Screenshots

This directory contains the authentic screenshots extracted directly from the completed Type-1 experiment document (`type 1.pdf`).

## Index of Screenshots

| File Name | Figure in Document | Description | Key Details Visible |
| :--- | :--- | :--- | :--- |
| `fig01_hostnamectl.jpeg` | **Figure 1** | Guest system identification via `hostnamectl` | Static hostname: `vm01-Standard-PC-i440FX-PIIX-1996`, OS: Ubuntu 22.04.5 LTS, Kernel: Linux 6.8.0-40-generic, Virtualization: kvm |
| `fig02_lscpu.jpeg` | **Figure 2** | CPU architecture inspection via `lscpu` | Architecture: x86_64, 2 CPUs / 2 cores, Model: QEMU Virtual CPU version 2.5+, Hypervisor: KVM (full) |
| `fig03_lscpu_extended.jpeg` | **Figure 3** | Extended `lscpu` output | Virtualization caches, NUMA node (1 node, CPUs 0,1), vulnerability mitigations |
| `fig04_free_memory.jpeg` | **Figure 4** | Memory and swap inspection via `free -h` | Mem Total: 1.9Gi, Used: 765Mi, Free: 449Mi, Available: 1.0Gi; Swap: 2.1Gi |
| `fig05_free_df_top.jpeg` | **Figure 5** | Combined resource view (`free -h`, `df -h`, `top`) | Root filesystem: 20G total, 9.5G used, 8.6G avail (53% use); `top` showing active processes, 0.3% us, 0.2% sy |
| `fig06_sysbench_cpu_benchmark.jpeg` | **Figure 6** | Sysbench CPU benchmark execution & results | `sysbench 1.0.20`, 1 thread, prime limit 20000; events/sec: 1587.47, total events: 15877, total time: 10.0005s, avg latency: 0.63 ms |
| `fig07_proxmox_cpu_memory_graphs.jpeg` | **Figure 7** | Proxmox VE summary graphs | CPU usage peaks (~50%) and Memory usage timeline (~1.8 GiB) |
| `fig08_proxmox_vm_summary.jpeg` | **Figure 8** | Proxmox VE VM Summary card | VM ID: 109 (`vm01-type01`), Status: running, 2 CPUs, 2.00 GiB memory, 20.00 GiB bootdisk |
| `fig09_proxmox_memory_graph.jpeg` | **Figure 9** | Detailed Proxmox VE memory usage chart | Memory usage graph spanning 09:35:00 to 10:45:00 |
| `fig10_proxmox_network_graph.jpeg` | **Figure 10** | Detailed Proxmox VE network traffic chart | Inbound and outbound network bandwidth spikes during package update/installation |
| `fig11_proxmox_disk_io_graph.jpeg` | **Figure 11** | Detailed Proxmox VE disk I/O chart | Disk read/write throughput during guest operations |
