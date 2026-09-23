# Type-2 (VMware Workstation) Experimental Evidence & Screenshots

This directory contains the authentic screenshots extracted directly from the completed Type-2 experiment document (`type 2.pdf`).

## Index of Screenshots

| File Name | Figure in Document | Description | Key Details Visible |
| :--- | :--- | :--- | :--- |
| `fig01_hostnamectl.jpeg` | **Figure 1** | Guest system identification via `hostnamectl` | Static hostname: `nupur-virtual-machine`, OS: Ubuntu 22.04.5 LTS, Kernel: Linux 6.8.0-138-generic, Virtualization: vmware |
| `fig02_lscpu_part1.jpeg` | **Figure 2 (Upper)** | CPU architecture details via `lscpu` | Architecture: x86_64, 2 CPUs / 2 cores, Model name: 13th Gen Intel(R) Core(TM) i5-13450HX |
| `fig02_lscpu_part2.jpeg` | **Figure 2 (Lower)** | CPU features and virtualization flags via `lscpu` | Virtualization: full, Hypervisor vendor: VMware, caches and vulnerability status |
| `fig03_free_memory.jpeg` | **Figure 3** | Memory inspection via `free -h` | Mem Total: 1.9Gi, Used: 869Mi, Free: 256Mi, Available: 865Mi; Swap: 2.1Gi |
| `fig04_df_disk.jpeg` | **Figure 4** | Disk filesystem inspection via `df -h` | `/dev/sda3` 20G total, 12G used, 6.5G avail (64% use) |
| `fig05_top_processes.jpeg` | **Figure 5** | Live processes and utilization via `top` | 292 tasks, load average: 0.88, 0.63, 0.65; CPU user: 1.2%, system: 0.0%, idle: 97.7% |
| `fig06_sysbench_install_part1.jpeg` | **Figure 6 (Upper)** | Package manager `apt install sysbench` start | Repository index fetching and dependency calculation |
| `fig06_sysbench_install_part2.jpeg` | **Figure 6 (Lower)** | Sysbench package setup and triggers | Unpacking and configuring `sysbench 1.0.20+ds-2` and `libluajit` |
| `fig07_sysbench_cpu_benchmark.jpeg` | **Figure 7** | Sysbench CPU benchmark execution & results | `sysbench 1.0.20`, 1 thread, prime limit 20000; events/sec: 1440.80, total events: 14410, total time: 10.0004s, avg latency: 0.69 ms, max latency: 1.89 ms |
