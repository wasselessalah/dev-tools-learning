# — Virtual Machines

## 1. Learning Objectives

After studying this documentation, you should be able to:

- Understand what a Virtual Machine (VM) is.
- Explain the difference between a host and a guest.
- Understand virtual CPUs, RAM, disks, and network interfaces.
- Install and manage VMs on Ubuntu Linux.
- Use KVM, QEMU, and libvirt.
- Manage VM states using command-line tools.
- Understand snapshots, backups, and cloning.
- Troubleshoot common VM networking problems.

## 2. What Is a Virtual Machine?

A **Virtual Machine (VM)** is a software-based computer that runs an operating system and applications inside another computer.

A VM behaves like a physical computer, with virtual hardware such as a CPU, RAM, hard disk, and network interface.

For example, you can run Ubuntu Server inside your Ubuntu desktop without installing Ubuntu Server directly on your physical disk.

### Important terminology

- **Host:** The physical computer that runs the virtualization software.
- **Guest:** The operating system running inside the VM.
- **Hypervisor:** Software that creates and manages virtual machines.
- **Virtual disk:** Storage used by the guest operating system.
- **ISO image:** A file containing installation media for an operating system.
- **Virtual NIC:** A virtual network interface card.
- **Snapshot:** A point-in-time state of a VM or its disks, depending on the platform.

## 3. Why Use Virtual Machines?

Virtual machines are useful for:

- Testing different operating systems.
- Learning Linux system administration.
- Practicing networking and cybersecurity in isolated labs.
- Running servers without purchasing additional physical computers.
- Developing and testing applications.
- Building private cloud environments.
- Testing software before deploying it to production.

**Example:** A single physical computer can run Ubuntu Server, Windows, and another Linux distribution as separate VMs, provided it has sufficient resources.

## 4. VM Architecture

A simplified architecture looks like this:

```text
Physical Computer
        |
        v
Host Operating System
        |
        v
Hypervisor / Virtualization Layer
        |
        +--------------------+
        |                    |
        v                    v
     VM 1                  VM 2
   Ubuntu Server          Windows
        |                    |
        v                    v
   Virtual CPU           Virtual CPU
   Virtual RAM           Virtual RAM
   Virtual Disk          Virtual Disk
   Virtual NIC           Virtual NIC
```

Each VM has its own virtual hardware configuration and normally runs its own guest operating system kernel.

## 5. Main Components of a VM

### 5.1 Virtual CPU (vCPU)

A vCPU is a virtual processor presented to the guest operating system.

Example:
- Host: 8 CPU cores.
- VM 1: 2 vCPUs.
- VM 2: 2 vCPUs.

The number of vCPUs assigned to VMs does not necessarily correspond to dedicated physical cores.

### 5.2 RAM

RAM is the memory allocated to a VM.

Example:

| VM | RAM |
|---|---:|
| Ubuntu Server | 2 GB |
| Windows 11 | 8 GB |
| Linux testing VM | 2 GB |

Always leave sufficient memory for the host operating system.

### 5.3 Virtual Disk

A virtual disk stores the guest operating system, applications, and data.

Common formats include:

- **QCOW2:** Supports features such as snapshots and thin provisioning, depending on the storage configuration.
- **RAW:** A simple disk-image format.
- **VMDK:** Commonly used by VMware.
- **VDI:** Commonly used by VirtualBox.

Example files:

```text
ubuntu-server.qcow2
windows-server.qcow2
```

### 5.4 Virtual Network Interface

A virtual network interface connects the VM to a virtual network.

Common networking modes:

- **NAT:** The guest accesses external networks through a translated connection.
- **Bridged:** The VM appears as a separate device on the physical network.
- **Host-only:** The VM communicates with the host and potentially other VMs on that network.
- **Isolated:** Communication is restricted to the configured virtual network.

### 5.5 ISO Image

An ISO image is commonly used to install a guest operating system.

Example:

```text
ubuntu-24.04-live-server-amd64.iso
```

The VM boots from the ISO, runs the installer, and installs the operating system on its virtual disk.

## 6. Virtual Machine States

| State | Description |
|---|---|
| Running | The VM is executing. |
| Shut off | The VM is not running. |
| Paused | Execution is temporarily suspended. |
| Suspended | Runtime state may be saved for later continuation. |
| Crashed | The platform reports an abnormal failure. |

Always prefer shutting down the guest operating system cleanly. A forced stop can cause data loss.

## 7. VM vs Physical Machine vs Container

| Feature | Physical machine | Virtual machine | Container |
|---|---|---|---|
| Hardware | Physical | Virtual hardware | Uses host hardware |
| Operating system | Installed on hardware | Usually has its own guest OS kernel | Usually shares the host kernel |
| Isolation | Depends on system configuration | Managed by the hypervisor | Process-level isolation |
| Startup | Hardware boot | Guest OS boot | Usually fast process startup |
| Typical use | Workstations and servers | OS labs and virtual servers | Application deployment |

VMs and containers solve different problems. Containers do not replace VMs in every situation.

## 8. Installing Virtualization Tools on Ubuntu

This section uses KVM and libvirt.

### Step 1: Check hardware virtualization

Run:

```bash
lscpu | grep -i virtualization
```

Check processor virtualization flags:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

Check whether the KVM device exists:

```bash
ls -l /dev/kvm
```

A missing device may indicate disabled firmware settings, unavailable virtualization support, unloaded modules, or missing permissions.

### Step 2: Install the tools

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients virt-manager
```

- `qemu-kvm`: QEMU with KVM virtualization support.
- `libvirt`: Tools and services for managing VMs.
- `virsh`: Command-line VM management client.
- `virt-manager`: Graphical VM management application.

### Step 3: Check libvirt

```bash
virsh -c qemu:///system list --all
```

Check the relevant service:

```bash
systemctl status libvirtd
```

Some Ubuntu releases use modular libvirt services. If `libvirtd` is not present, inspect the installed libvirt services rather than assuming the installation failed.

## 9. Managing VMs with virsh

`virsh` is a command-line tool for managing virtual machines through libvirt.

### List all VMs

```bash
virsh list --all
```

Example:

```text
 Id   Name              State
---------------------------------
 -    ubuntu-server     shut off
 2    test-vm           running
```

### Start a VM

```bash
virsh start ubuntu-server
```

### Shut down a VM

```bash
virsh shutdown ubuntu-server
```

This requests a graceful shutdown. The guest must support and respond to the shutdown request.

### Force-stop a VM

```bash
virsh destroy ubuntu-server
```

**Warning:** This forcibly stops the VM. It does not delete the VM definition, but it can cause data loss.

### Reboot a VM

```bash
virsh reboot ubuntu-server
```

### Display VM information

```bash
virsh dominfo ubuntu-server
```

### Check the VM state

```bash
virsh domstate ubuntu-server
```

### Enable automatic startup

```bash
virsh autostart ubuntu-server
```

Disable automatic startup:

```bash
virsh autostart --disable ubuntu-server
```

Replace `ubuntu-server` with the actual VM name.

## 10. VM Networking

Virtual networking allows VMs to communicate with each other, the host, and external networks.

### NAT networking

NAT is commonly used for development and testing.

Advantages:
- Convenient internet access for guests.
- Usually requires less physical network configuration.

Limitations:
- Other devices on the LAN generally cannot initiate connections directly to the guest without additional configuration.

### Bridged networking

A bridged VM can appear as a separate device on the LAN.

Advantages:
- Other network devices may connect directly to the VM.
- Useful for server and network labs.

Limitations:
- Wi-Fi networks and managed networks may restrict bridged access.
- Network policies and firewall rules can affect connectivity.

### Inspect virtual networks

```bash
virsh net-list --all
```

Inspect the default network if it exists:

```bash
virsh net-dumpxml default
```

### Troubleshoot inside a Linux guest

Check the IP address:

```bash
ip addr
```

Check the routing table:

```bash
ip route
```

Test the gateway:

```bash
ping -c 3 <gateway-ip>
```

Test external IP connectivity:

```bash
ping -c 3 1.1.1.1
```

Test DNS resolution:

```bash
getent hosts example.com
```

Replace `<gateway-ip>` with the actual gateway address.

Interpretation:
- No IP address: investigate DHCP and the virtual network.
- No gateway connectivity: investigate the guest interface, virtual switch, and firewall.
- External IP works but domain names fail: investigate DNS.
- Ping fails: remember that ICMP traffic may be blocked even when other network services work.

## 11. Snapshots, Backups, and Cloning

### Snapshot

A snapshot records a point-in-time state of a VM or its disks, depending on the hypervisor and storage configuration.

Useful for:
- Testing software.
- Returning to a known state.
- Experimenting with system configuration.

### Backup

A backup is a separate recoverable copy of VM data and configuration.

Useful for:
- Recovering from disk failure.
- Restoring accidentally deleted data.
- Disaster recovery.

### Clone

A clone is a copy of an existing VM.

Useful for:
- Creating multiple test machines.
- Preparing development environments.
- Deploying similar systems.

### Template

A template is a reusable VM definition or base image used to create new VMs.

**Important:** A snapshot is not a substitute for a reliable backup. Always test your recovery process.

## 12. VM Storage Management

Inspect available filesystem space on the host:

```bash
df -h
```

List block devices:

```bash
lsblk
```

List libvirt storage pools:

```bash
virsh pool-list --all
```

List volumes in a pool:

```bash
virsh vol-list POOL_NAME
```

Replace `POOL_NAME` with an actual pool name.

Check storage capacity regularly. A full host filesystem or datastore can prevent VMs from starting or writing data.

## 13. Common VM Problems

| Problem | Possible cause | First action |
|---|---|---|
| `/dev/kvm` is missing | Virtualization support or modules unavailable | Check BIOS/UEFI, CPU flags, and modules |
| VM is slow | Resource contention or slow storage | Check host CPU, RAM, and disk usage |
| VM has no IP address | DHCP or virtual network problem | Inspect `ip addr` and virtual networks |
| VM cannot boot | Incorrect boot order or invalid ISO | Check ISO and boot configuration |
| VM cannot shut down | Guest OS is unresponsive | Check the console before force-stopping |
| Not enough disk space | Host or datastore is full | Run `df -h` and inspect storage pools |
| VM cannot reach the internet | Routing, NAT, DNS, or firewall issue | Test connectivity in layers |

## 14. Security Best Practices

- Keep the host and guest operating systems updated.
- Use strong passwords and least-privilege accounts.
- Avoid exposing VM management services to untrusted networks.
- Isolate risky experiments from important systems.
- Protect VM images, templates, and snapshots.
- Back up important data.
- Monitor CPU, RAM, storage, and network usage.
- Document VM names, operating systems, IP addresses, and allocated resources.

## 15. Practical Exercises

### Exercise 1: Understand your host

Run:

```bash
lscpu
free -h
lsblk
ls -l /dev/kvm
```

Write down the CPU, available memory, disks, and KVM device status.

### Exercise 2: Inspect your VMs

```bash
virsh list --all
```

Choose a test VM and inspect it:

```bash
virsh dominfo VM_NAME
virsh domstate VM_NAME
```

### Exercise 3: Manage the VM lifecycle

Practice starting and shutting down a test VM:

```bash
virsh start VM_NAME
virsh shutdown VM_NAME
```

Verify its state after each operation.

### Exercise 4: Investigate networking

Inside the guest, run:

```bash
ip addr
ip route
getent hosts example.com
```

Identify the guest IP address, default gateway, and whether DNS resolution works.

### Exercise 5: Explain the concepts

Answer these questions in your own words:

1. What is a virtual machine?
2. What is the difference between a host and a guest?
3. What is the role of KVM?
4. What is the role of QEMU?
5. What is the role of libvirt?
6. What is the difference between NAT and bridged networking?
7. Why is a snapshot different from a backup?
8. What happens when a VM receives insufficient RAM?

## 16. Official Documentation

- Ubuntu Server documentation: https://documentation.ubuntu.com/server/
- KVM: https://www.linux-kvm.org/
- QEMU documentation: https://www.qemu.org/docs/master/
- libvirt documentation: https://libvirt.org/docs.html
- VirtualBox documentation: https://www.virtualbox.org/wiki/Documentation
- OpenNebula documentation: https://docs.opennebula.io/

---

**Recommended learning order:** Linux fundamentals → VM concepts → KVM/QEMU/libvirt → VM networking → VM storage → snapshots and backups → OpenNebula.
