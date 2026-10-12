#  — Virtualization

## 1. Learning Objectives

After studying this documentation, you should be able to:

- Define virtualization and explain its advantages.
- Understand different types of virtualization.
- Distinguish Type 1 and Type 2 hypervisors.
- Explain KVM, QEMU, and libvirt.
- Understand virtual networking and storage.
- Manage virtual machines on Ubuntu Linux.
- Understand OpenNebula architecture and its role.
- Troubleshoot basic virtualization problems.

---

## 2. What Is Virtualization?

**Virtualization** is a technology that creates software-based versions of computing resources, such as computers, operating systems, networks, and storage.

It allows multiple isolated environments to share the resources of a physical computer.

For example, one physical server can run multiple virtual machines, each with its own operating system and applications.

### Example

A physical server with 16 CPU cores, 64 GB of RAM, and 1 TB of storage could host several VMs, depending on their resource requirements and the virtualization configuration.

```text
Physical Server
       |
       v
Virtualization Layer
       |
       +------------------+
       |                  |
       v                  v
     VM 1                VM 2
 Ubuntu Server          Windows
       |                  |
       v                  v
  Applications        Applications
```

Virtualization does not create additional physical resources. It allocates and shares the resources available on the host.

---

## 3. Why Is Virtualization Important?

Virtualization is widely used in system administration, cloud computing, development, and data centers.

### Main advantages

**1. Resource optimization**

Multiple workloads can share one physical server.

**2. Cost reduction**

Organizations can reduce the number of physical servers they need.

**3. Isolation**

Virtual machines provide separate operating-system environments, subject to the security of the hypervisor and its configuration.

**4. Flexibility**

Administrators can create, configure, start, stop, and remove VMs as needed.

**5. Testing and development**

Developers can test software and operating systems without changing their main environment.

**6. Scalability**

Virtualization platforms can manage additional VMs and hosts as infrastructure grows.

**7. Recovery**

Backups, templates, and supported migration or recovery features can help restore services after failures.

### Limitations

- VMs consume physical CPU, RAM, storage, and network resources.
- Poor resource allocation can reduce performance.
- A host failure can affect several VMs.
- Virtualization requires security updates and monitoring.
- High availability and disaster recovery require additional configuration.

---

## 4. Types of Virtualization

### 4.1 Server Virtualization

Server virtualization allows multiple VMs to run on one physical server.

Each VM usually has its own guest operating system and virtual hardware.

**Examples:** KVM, VMware ESXi, Hyper-V, and Xen.

**Use cases:**
- Hosting application servers.
- Running Linux and Windows servers.
- Building testing environments.
- Consolidating physical servers.

### 4.2 Operating-System-Level Virtualization

Operating-system-level virtualization uses containers to isolate applications and processes while sharing the host kernel in typical container configurations.

**Examples:** Docker and LXC.

**Use cases:**
- Application deployment.
- Microservices.
- CI/CD pipelines.
- Lightweight development environments.

Containers are different from full virtual machines because they generally do not run a separate guest OS kernel.

### 4.3 Network Virtualization

Network virtualization creates logical networks using software.

Common technologies include:

- Virtual switches.
- Virtual routers.
- VLANs.
- Software-defined networking (SDN).
- Network overlays.

**Use cases:**
- Isolating workloads.
- Connecting VMs.
- Creating private cloud networks.
- Separating management and application traffic.

### 4.4 Storage Virtualization

Storage virtualization combines or abstracts physical storage resources into logical storage pools and volumes.

Examples include:

- LVM.
- Virtual disk images.
- Network-attached storage.
- Distributed storage systems.

**Use cases:**
- Managing VM disks.
- Allocating storage dynamically.
- Centralizing storage management.
- Supporting migration and recovery workflows.

### 4.5 Desktop Virtualization

Desktop virtualization provides desktop environments through a centralized infrastructure or remote delivery system.

**Use cases:**
- Remote work.
- Computer laboratories.
- Centralized desktop administration.
- Shared organizational environments.

### 4.6 Application Virtualization

Application virtualization isolates or packages applications to reduce dependencies on the underlying system.

Its exact implementation depends on the technology used.

---

## 5. What Is a Hypervisor?

A **hypervisor** is a software layer that creates and manages virtual machines.

It allocates physical resources and presents virtual hardware to guest operating systems.

### Type 1 Hypervisor — Bare-Metal

A Type 1 hypervisor operates directly on server hardware or as a tightly integrated hypervisor layer.

Examples:

- VMware ESXi.
- Microsoft Hyper-V.
- Xen.
- KVM-based Linux virtualization stacks.

Typical use cases:
- Data centers.
- Enterprise servers.
- Private clouds.
- Production infrastructure.

### Type 2 Hypervisor — Hosted

A Type 2 hypervisor runs as an application on a conventional host operating system.

Examples:

- Oracle VirtualBox.
- VMware Workstation.

Typical use cases:
- Personal computers.
- Development environments.
- Training laboratories.
- Operating-system testing.

### Comparison

| Feature | Type 1 | Type 2 |
|---|---|---|
| Architecture | Hypervisor layer on hardware | Runs on a host OS |
| Typical use | Servers and data centers | Desktop labs and development |
| Examples | ESXi, Xen, KVM-based stacks | VirtualBox, VMware Workstation |
| Management | Often designed for centralized infrastructure | Often managed from the host desktop |

The Type 1 and Type 2 distinction is a useful introductory model. Modern virtualization architectures can be more complex than this classification suggests.

---

## 6. KVM, QEMU, and libvirt

These technologies are essential for understanding virtualization on Ubuntu Linux.

### 6.1 KVM

**KVM (Kernel-based Virtual Machine)** is Linux kernel virtualization infrastructure.

It allows a Linux system with compatible hardware support to run virtual machines using hardware virtualization extensions.

Responsibilities:
- CPU virtualization.
- Memory virtualization.
- Kernel-level virtualization support.

### 6.2 QEMU

**QEMU** provides machine emulation and virtual-device models.

When used with KVM, QEMU can use hardware-assisted virtualization for guest CPU execution.

Responsibilities:
- Running VM processes.
- Presenting virtual devices.
- Supporting virtual machine configurations.
- Emulating hardware when required.

### 6.3 libvirt

**libvirt** is a management layer for virtualization technologies.

It provides tools and APIs to configure and manage VMs, networks, and storage pools.

Common tools:
- `virsh`: Command-line management.
- `virt-manager`: Graphical VM management.

### Architecture

```text
Physical Computer
       |
       v
Linux Host
       |
       +-------------------------+
       |                         |
       v                         v
   KVM Kernel                 libvirt
       |                         |
       v                         v
      QEMU                  VM Management
       |
       +-------------------+
       |                   |
       v                   v
      VM 1                VM 2
 Ubuntu Server           Linux Guest
```

In practice, libvirt manages the VM lifecycle and configuration, while QEMU and KVM provide the execution and virtualization components.

---

## 7. Hardware Virtualization

Modern processors include hardware virtualization extensions.

### Intel

Intel VT-x is commonly associated with the `vmx` CPU flag.

### AMD

AMD-V is commonly associated with the `svm` CPU flag.

These features may need to be enabled in the computer's BIOS or UEFI settings.

### Check virtualization support on Ubuntu

```bash
lscpu | grep -i virtualization
```

Check CPU flags:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

Check loaded KVM modules:

```bash
lsmod | grep kvm
```

Check the KVM device:

```bash
ls -l /dev/kvm
```

On an Intel system, the module is commonly `kvm_intel`. On an AMD system, it is commonly `kvm_amd`.

If `/dev/kvm` is missing, check firmware settings, CPU support, kernel modules, and device permissions.

### Nested virtualization

Nested virtualization allows a VM to run another hypervisor inside it.

For example:

```text
Physical Computer
       |
       v
    KVM Host
       |
       v
  Ubuntu VM
       |
       v
 Nested KVM
       |
       v
   Inner VM
```

The outer hypervisor must expose suitable virtualization extensions to the guest.

---

## 8. Important Virtualization Concepts

### Virtual CPU (vCPU)

A virtual processor presented to a VM. It is not necessarily a dedicated physical core.

### Memory allocation

RAM assigned to VMs. Excessive allocation can cause memory pressure on the host.

### Resource overcommitment

Allocating more virtual CPU or memory than the physical capacity available, relying on workload usage patterns.

Overcommitment can improve utilization but may cause performance problems when many VMs compete for resources.

### Thin provisioning

Storage capacity is allocated as data is written, depending on the disk format and storage backend.

### Snapshot

A point-in-time state of a VM or its disks. Snapshots have platform-specific limitations and are not a complete replacement for independent backups.

### Live migration

Moving a running VM from one host to another with minimal interruption when the platform, storage, CPU compatibility, and networking support it.

### High availability (HA)

A configured system that can recover or restart workloads after certain failures. HA is not automatic merely because virtualization is enabled.

### Template

A reusable VM definition or base image used to create new VMs.

### Nested virtualization

Running a hypervisor inside a virtual machine.

---

## 9. Virtual Networking

Virtual networking connects VMs to one another, to the host, and potentially to physical networks.

### 9.1 NAT

Network Address Translation allows a guest to access external networks through a translated network connection.

Advantages:
- Convenient for testing.
- Often easy to configure.
- Does not normally require each VM to have a directly reachable LAN address.

Limitations:
- Inbound connections may require port forwarding.
- Other LAN devices may not be able to reach the guest directly.

### 9.2 Bridged Networking

Bridged networking connects a VM to a physical network through a virtual bridge.

Advantages:
- The VM can appear as a separate device on the LAN.
- Useful for server and network laboratories.

Limitations:
- Wi-Fi or managed networks may restrict bridged connectivity.
- Network firewall policies may prevent communication.

### 9.3 Isolated Networking

An isolated virtual network restricts communication according to its configuration.

It is useful for testing, security labs, and private VM-to-VM communication.

### 9.4 VLAN

A VLAN logically separates Ethernet traffic into different broadcast domains.

It is commonly used to separate management, storage, and application networks.

### 9.5 Software-Defined Networking

SDN uses software to manage network behavior and policies. Virtualization platforms may integrate with SDN technologies to create and manage logical networks.

### Inspect virtual networks

```bash
virsh net-list --all
```

Inspect a network named `default`, if it exists:

```bash
virsh net-dumpxml default
```

Inside a Linux guest, inspect network settings:

```bash
ip addr
ip route
resolvectl status
```

Troubleshoot in layers:

1. Confirm the virtual interface is connected.
2. Confirm the guest has an IP address.
3. Test the default gateway.
4. Test external IP connectivity.
5. Test DNS resolution.
6. Check virtual-network and firewall configuration.

A failed ping does not necessarily mean the entire network is unavailable because ICMP traffic may be filtered.

---

## 10. Virtual Storage

Virtualization platforms can use several storage types.

### Local storage

VM disks are stored on the host's local disks.

### LVM

Logical Volume Manager provides logical storage volumes that can be used for VM disks.

### Network storage

Storage is accessed over a network, for example through NFS or iSCSI.

### Distributed storage

Data is stored across multiple systems. Some cloud platforms integrate with distributed storage technologies.

### Key terminology

- **Datastore:** A managed storage location for images or VM disks.
- **Storage pool:** A libvirt-managed storage resource.
- **Image:** A disk image or installation image.
- **Persistent disk:** Data intended to survive VM shutdown or recreation.
- **Ephemeral disk:** Temporary storage whose lifecycle may be linked to the VM.

### Linux storage commands

Display filesystem usage:

```bash
df -h
```

List disks and partitions:

```bash
lsblk
```

Inspect LVM physical volumes:

```bash
sudo pvs
```

Inspect volume groups:

```bash
sudo vgs
```

Inspect logical volumes:

```bash
sudo lvs
```

The LVM commands are relevant when LVM is installed and used.

---

## 11. OpenNebula

**OpenNebula** is a virtualization and cloud management platform that manages virtual machines, hosts, images, networks, and datastores through a centralized control plane.

It is useful for learning private cloud infrastructure and managing multiple virtualization hosts.

### Main components

**Front-end:** Runs OpenNebula management services.

**Host:** A physical or supported virtualization server managed by OpenNebula.

**VM template:** Defines VM resources and configuration, such as CPU, RAM, disks, and network interfaces.

**Image:** A reusable disk or installation image.

**Datastore:** Storage used for images and VM disks according to the datastore's configuration and type.

**Virtual network:** A network definition used by VM network interfaces.

**Sunstone:** OpenNebula's web-based management interface.

### Simplified architecture

```text
             OpenNebula Front-end
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
   Scheduler       Images         Networks
       |
       v
  Managed Hosts
       |
       +---------------------+
       |                     |
       v                     v
   KVM Host A            KVM Host B
       |                     |
       v                     v
    VM A1                  VM B1
    VM A2
```

### How KVM and OpenNebula differ

| Technology | Main role |
|---|---|
| KVM | Provides kernel virtualization capabilities |
| QEMU | Runs VM processes and provides virtual devices |
| libvirt | Manages VM configuration and lifecycle |
| OpenNebula | Manages cloud infrastructure and virtualization resources |

OpenNebula is not itself the CPU virtualization engine. It orchestrates and manages virtualization infrastructure.

### Basic OpenNebula CLI commands

List hosts:

```bash
onehost list
```

List VMs:

```bash
onevm list
```

List images:

```bash
oneimage list
```

List datastores:

```bash
onedatastore list
```

List virtual networks:

```bash
onevnet list
```

List templates:

```bash
onetemplate list
```

Inspect a VM:

```bash
onevm show VM_ID
```

Replace `VM_ID` with the actual VM identifier.

These commands require an installed and configured OpenNebula environment and suitable permissions.

---

## 12. Virtualization Security

Virtualization needs careful security configuration.

### Best practices

- Keep the host OS, hypervisor, and management tools updated.
- Restrict access to management interfaces.
- Use strong authentication and least-privilege accounts.
- Isolate untrusted VMs from sensitive networks.
- Configure firewall rules.
- Protect VM disks, templates, and backups.
- Remove credentials and secrets before creating reusable images.
- Monitor resource consumption and administrative activity.
- Test recovery procedures regularly.

A VM boundary helps isolate workloads, but a compromised host or hypervisor can affect multiple VMs.

---

## 13. Performance and Resource Management

Important metrics include:

- CPU utilization.
- Memory usage and swapping.
- Storage capacity, latency, and throughput.
- Network throughput and packet loss.
- Host temperature and power usage.

### Good practices

1. Allocate CPU and RAM according to workload requirements.
2. Leave enough resources for the host.
3. Monitor storage space.
4. Avoid excessive overcommitment for latency-sensitive workloads.
5. Use suitable storage backends.
6. Measure actual performance before changing resource allocations.

Adding more virtual CPUs or RAM does not always improve performance if the host is already overloaded.

---

## 14. Hands-On Laboratory

### Lab 1: Check your host

Run:

```bash
lscpu
free -h
lsblk
ls -l /dev/kvm
```

Record the CPU, memory, disks, and KVM device status.

### Lab 2: Create a Linux VM

1. Install KVM and libvirt.
2. Download an Ubuntu Server ISO.
3. Create a VM with suitable CPU, RAM, and storage.
4. Configure its network.
5. Install Ubuntu Server.
6. Update the guest OS.

### Lab 3: Test virtual networking

Inside the VM:

```bash
ip addr
ip route
getent hosts example.com
```

Identify the IP address, default gateway, and DNS behavior.

Compare NAT and bridged networking where your network supports both.

### Lab 4: Manage the VM lifecycle

```bash
virsh list --all
virsh start VM_NAME
virsh dominfo VM_NAME
virsh shutdown VM_NAME
```

Replace `VM_NAME` with your VM's name.

### Lab 5: Explore OpenNebula

1. Inspect managed hosts.
2. List VMs, images, datastores, and virtual networks.
3. Review a VM template.
4. Deploy a test VM.
5. Verify its IP address and connectivity.
6. Investigate host, image, datastore, and networking errors if deployment fails.

### Lab 6: Backup and recovery

1. Create a test file inside a VM.
2. Use the platform's supported backup method.
3. Verify that the backup contains the required data.
4. Restore into a test environment.
5. Confirm that the restored data is usable.

---

## 15. Knowledge Check

Answer these questions without looking at the documentation:

1. What is virtualization?
2. What is the difference between a VM and a container?
3. What is the role of a hypervisor?
4. What is the difference between Type 1 and Type 2 hypervisors?
5. What do KVM, QEMU, and libvirt do?
6. What is the difference between NAT and bridged networking?
7. What is a datastore?
8. What is the difference between a snapshot and a backup?
9. What is resource overcommitment?
10. What is the difference between KVM and OpenNebula?
11. What is live migration?
12. Why must virtualization infrastructure be monitored and secured?

---

## 16. Official Documentation

- **KVM:** https://www.linux-kvm.org/
- **QEMU:** https://www.qemu.org/docs/master/
- **libvirt:** https://libvirt.org/docs.html
- **OpenNebula:** https://docs.opennebula.io/
- **Ubuntu Server:** https://documentation.ubuntu.com/server/
- **VirtualBox:** https://www.virtualbox.org/wiki/Documentation
- **Docker:** https://docs.docker.com/

---

## 17. Recommended Learning Order

1. Linux fundamentals.
2. Computer networking and TCP/IP.
3. Virtual machine concepts.
4. KVM and QEMU.
5. libvirt and `virsh`.
6. Virtual networking.
7. Virtual storage and disk management.
8. Snapshots and backups.
9. OpenNebula.
10. Cloud infrastructure, monitoring, and security.

**Final objective:** Build a working virtualization lab on Ubuntu, deploy Linux VMs, configure networking and storage, and understand how OpenNebula manages virtualized infrastructure.
