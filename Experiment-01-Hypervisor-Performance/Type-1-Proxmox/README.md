# Part 1: Performance Analysis Using Type-1 Hypervisor (Proxmox VE)

## Experiment Information

- **Experiment:** Performance Analysis of Type-1 Hypervisor – Proxmox VE | Part 1
- **Focus:** CPU Performance Analysis of an Ubuntu Virtual Machine using Sysbench
- **Student Name:** Mehak Sayed Yusuf
- **USN:** 01FE24BCI012
- **Division:** B
- **Roll No.:** 202

---

## 1. Aim

To create and verify a virtual machine on the **Proxmox VE Type-1 hypervisor**, inspect its CPU, memory, and disk configuration, monitor resource utilization, and measure CPU performance using **Sysbench**.

---

## 2. Experimental Configuration

| Parameter | Configured / Observed Value |
| :--- | :--- |
| **Hypervisor** | Proxmox VE (Type-1 Bare-Metal Hypervisor) |
| **Guest OS** | Ubuntu 22.04.5 LTS (x86_64) |
| **CPU Allocation** | 1 socket, 2 cores = 2 vCPU |
| **CPU Model** | QEMU Virtual CPU version 2.5+ |
| **Memory Allocation** | 2048 MiB (2 GB) |
| **Virtual Disk** | 20 GB |
| **Network Bridge** | vmbr0 |
| **Virtualization Vendor** | KVM (Full Virtualization) |

---

## 3. Step-by-Step Procedure

1. **Access Proxmox VE:** Open the Proxmox VE web management interface and sign in with the assigned credentials.
2. **Create the VM:** Use the *Create VM* wizard. Configure Ubuntu as the guest OS and allocate 2 vCPUs, 2 GB RAM, and a 20 GB virtual disk.
3. **Configure Networking:** Attach the VM to the `vmbr0` bridge using the default VirtIO network model.
4. **Install Ubuntu:** Start the VM from the Proxmox console, install Ubuntu 22.04.5 LTS, restart, and log in.
5. **Verify the VM:** Run `hostnamectl`, `lscpu`, `free -h`, and `df -h` inside the guest terminal to verify the operating system, CPU, memory, and disk.
6. **Monitor Resources:** Run `top` to observe CPU utilization, memory utilization, active processes, and load averages.
7. **Install Sysbench:** Update package repositories using `sudo apt update` and install Sysbench with `sudo apt install sysbench -y`.
8. **Run CPU Benchmark:** Execute:
   ```bash
   sysbench cpu --cpu-max-prime=20000 run
   ```
   Record execution time, total events, events per second, and latency figures.
9. **Monitor from Proxmox:** Use the Proxmox VE *VM Summary* and monitoring charts to observe host-level CPU, memory, network traffic, and disk I/O.
10. **Shut Down:** After completing all measurements, shut down the Ubuntu VM using `sudo poweroff` or the Proxmox shutdown option.

---

## 4. Commands Used

```bash
# VM and OS inspection
hostnamectl
lscpu
free -h
df -h
top

# Sysbench installation
sudo apt update
sudo apt install sysbench -y
sysbench --version

# CPU benchmark execution
sysbench cpu --cpu-max-prime=20000 run

# Clean shutdown
sudo poweroff
```

---

## 5. Observed Experimental Results

| Metric | Observed Result |
| :--- | :--- |
| **Operating System** | Ubuntu 22.04.5 LTS |
| **Architecture** | x86_64 |
| **Virtual CPUs** | 2 CPUs / 2 cores |
| **CPU Model** | QEMU Virtual CPU version 2.5+ |
| **Hypervisor / Virtualization** | KVM; virtualization type shown as full |
| **Memory** | ~1.9 GiB total reported by `free -h`; Proxmox VM configured with 2.00 GiB |
| **Disk** | 20 GB virtual disk; `df -h` shows a 20G root filesystem |
| **Sysbench Execution Time** | 10.0005 s |
| **Sysbench Total Events** | 15,877 |
| **Sysbench Events Per Second** | 1,587.47 |
| **Sysbench Minimum Latency** | 0.59 ms |
| **Sysbench Average Latency** | 0.63 ms |
| **Sysbench Maximum Latency** | 1.34 ms |
| **Sysbench 95th Percentile** | 0.65 ms |

---

## 6. Resource Utilization Observations

- The Proxmox VE screenshots show the virtual machine actively running with 2 vCPUs and 2.00 GiB of allocated memory.
- The CPU graph shows distinct short periods of increased utilization during the Sysbench CPU workload.
- The memory graph indicates memory usage rising to approximately 1.8 GiB during active system operation.
- Network traffic shows a pronounced transient spike corresponding to the package repository update and installation phase.
- The Disk I/O graph shows clear read/write activity spikes corresponding to OS operations and package installation.

---

## 7. Result and Conclusion

Part 1 was successfully completed on a **Proxmox VE Type-1 bare-metal hypervisor**. The Ubuntu 22.04.5 LTS virtual machine was provisioned with 2 vCPUs, 2 GB RAM, and a 20 GB virtual disk. The VM configuration was verified using standard Linux system commands, and resource utilization was monitored both inside the guest and through the Proxmox VE management interface.

During the Sysbench CPU benchmark test, the virtual machine completed the benchmark in **10.0005 seconds**, processed **15,877 total events** at an average rate of **1,587.47 events per second**, with an average latency of **0.63 ms**. These values provide the Type-1 performance baseline for comparative analysis.

---

## 8. Artifacts and Evidence

- **Full PDF Report:** [type 1.pdf](./type%201.pdf)
- **Extracted Experimental Screenshots:** Available in the [screenshots/](./screenshots/) folder.
  - Figure 1: `hostnamectl` output
  - Figure 2: `lscpu` output
  - Figure 3: Extended `lscpu` output (caches, vulnerabilities)
  - Figure 4: `free -h` memory output
  - Figure 5: Combined `free -h`, `df -h`, and `top` output
  - Figure 6: `sysbench cpu --cpu-max-prime=20000 run` benchmark output
  - Figures 7–11: Proxmox VE summary, CPU, RAM, network, and disk I/O monitoring graphs
