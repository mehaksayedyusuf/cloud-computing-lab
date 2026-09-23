# Part 2: Performance Analysis Using Type-2 Hypervisor (VMware Workstation)

## Experiment Information

- **Experiment:** Performance Analysis of Type-2 Hypervisor – VMware Workstation | Part 2
- **Focus:** CPU Performance Analysis of an Ubuntu Virtual Machine using Sysbench
- **Student Name:** Mehak Sayed Yusuf
- **USN:** 01FE24BCI012
- **Division:** B
- **Roll No.:** 202

---

## 1. Aim

To create and verify a virtual machine on the **VMware Workstation Type-2 hypervisor**, inspect its CPU, memory, and disk configuration, monitor resource utilization, and measure CPU performance using **Sysbench**.

---

## 2. Experimental Configuration

| Parameter | Configured / Observed Value |
| :--- | :--- |
| **Hypervisor** | VMware Workstation (Type-2 Hosted Hypervisor) |
| **Guest OS** | Ubuntu 22.04.5 LTS (x86_64) |
| **CPU Allocation** | 1 processor, 2 cores = 2 vCPU |
| **CPU Model** | 13th Gen Intel Core i5-13450HX presented to guest |
| **Memory Allocation** | 2048 MB (approximately 2 GB) |
| **Virtual Disk** | 20 GB virtual disk |
| **Network Configuration** | NAT (`vmnet8`) |
| **Virtualization Information** | VMware / full virtualization |

---

## 3. Step-by-Step Procedure

1. **Launch VMware Workstation:** Open the VMware Workstation application on the host machine and select *Create a New Virtual Machine*.
2. **Select Wizard Mode:** Choose the *Typical* configuration option and point the installer to the Ubuntu 22.04.5 LTS ISO image.
3. **Configure Storage:** Create the virtual machine and specify a 20 GB virtual disk size.
4. **Customize Virtual Hardware:** Allocate 2 GB RAM and 2 vCPUs (1 processor, 2 cores); configure the network adapter using NAT.
5. **Install Ubuntu:** Power on the virtual machine, complete the standard Ubuntu installation process, reboot the VM, and log in.
6. **Verify the VM:** Run `hostnamectl`, `lscpu`, `free -h`, and `df -h` inside the terminal to confirm the operating system, processor architecture, memory, and disk parameters.
7. **Monitor Resources:** Run `top` to observe CPU utilization, memory utilization, active processes, and system load averages.
8. **Install Sysbench:** Update the package lists via `sudo apt update` and install the benchmark tool via `sudo apt install sysbench -y`.
9. **Execute CPU Benchmark:** Run:
   ```bash
   sysbench cpu --cpu-max-prime=20000 run
   ```
   Record execution duration, total processed events, throughput (events/sec), and latency statistics.
10. **Shut Down:** Power down the Ubuntu VM cleanly using `sudo poweroff` after all experimental data has been captured.

---

## 4. Commands Used

```bash
# VM and OS verification
hostnamectl
lscpu
free -h
df -h
top

# Package installation
sudo apt update
sudo apt install sysbench -y
sysbench --version

# Sysbench execution
sysbench cpu --cpu-max-prime=20000 run

# Shutdown
sudo poweroff
```

---

## 5. Observed Experimental Results

| Metric | Observed Result |
| :--- | :--- |
| **Operating System** | Ubuntu 22.04.5 LTS |
| **Architecture** | x86_64 |
| **Virtual CPUs** | 2 CPUs / 2 cores |
| **CPU Model Reported by Guest** | 13th Gen Intel Core i5-13450HX |
| **Virtualization** | VMware; full virtualization |
| **Memory Reported by `free -h`** | Approximately 1.9 GiB total (available ~865 MiB) |
| **Disk** | 20 GB virtual disk (`df -h`: ~12 GB used, 6.5 GB available) |
| **Sysbench Execution Time** | 10.0004 s |
| **Sysbench Total Events** | 14,410 |
| **Sysbench Events Per Second** | 1,440.80 |
| **Sysbench Minimum Latency** | 0.65 ms |
| **Sysbench Average Latency** | 0.69 ms |
| **Sysbench Maximum Latency** | 1.89 ms |
| **Sysbench 95th Percentile** | 0.90 ms |

---

## 6. Resource Utilization Observations

- The Ubuntu guest successfully detected 2 online virtual CPUs and approximately 2 GB of allocated memory.
- The `free -h` output confirmed ~1.9 GiB of total addressable RAM, with ~865 MiB available at the sampled instant.
- The `df -h` command showed a 20 GB virtual root filesystem with 12 GB utilized and 6.5 GB free.
- The `top` output displayed active guest processes with an idle CPU baseline prior to benchmark initiation.
- Because VMware Workstation is a Type-2 hypervisor, the virtual resources exposed to Ubuntu are mediated through the Windows host operating system kernel and hardware scheduler.

---

## 7. Type-2 Hypervisor Interpretation

- **Architecture:** VMware Workstation operates above the host operating system, while Ubuntu runs as a guest inside an application-managed sandbox.
- **Host–Guest Interaction:** Virtual CPU, RAM, storage, and network requests are translated by the hypervisor and handled through the host OS device drivers.
- **Isolation:** The guest executes within isolated virtual disks and virtual CPUs, preventing it from directly accessing or controlling the host's physical peripheral devices.
- **Typical Use Cases:** Desktop virtualization, software development and testing, educational laboratory environments, and running non-native secondary operating systems on personal workstations.
- **Performance Considerations:** Sysbench benchmark throughput and latency are subject to host OS context switching, background host processes, and virtualization translation overhead.

---

## 8. Result and Conclusion

Part 2 was completed using **VMware Workstation as a Type-2 hypervisor**. The Ubuntu virtual machine was configured with 2 vCPUs, 2 GB RAM, and a 20 GB virtual disk. The VM configuration was verified using standard Linux system commands, and guest resource utilization was observed using `top`.

For the Sysbench CPU run (`--cpu-max-prime=20000`), the virtual machine completed the benchmark in **10.0004 seconds** and processed **14,410 events** at **1,440.80 events per second**, with an average latency of **0.69 ms**. These values provide the Type-2 result set for comparative evaluation with the Type-1 Proxmox VE experiment.

---

## 9. Artifacts and Evidence

- **Full PDF Report:** [type 2.pdf](./type%202.pdf)
- **Extracted Experimental Screenshots:** Available in the [screenshots/](./screenshots/) folder.
  - Figure 1: `hostnamectl` output (`nupur@nupur-virtual-machine`)
  - Figure 2: `lscpu` output (Parts 1 & 2 showing Intel Core i5-13450HX CPU model)
  - Figure 3: `free -h` memory output
  - Figure 4: `df -h` disk filesystem output
  - Figure 5: `top` live process monitoring output
  - Figure 6: Sysbench installation log (Parts 1 & 2)
  - Figure 7: `sysbench cpu --cpu-max-prime=20000 run` benchmark output
