# VM Prerequisites

# Virtual Machine Setup

Linux VMs:

- CentOS VM
- Ubuntu VM

These are two flavor's of LinuxOS

### VM - Prerequisites (Windows Only):

#### **Step 1: Enable Virtualization in the BIOS or:**

- VTX
- Secure Virtual Machine
- Virtualization

Why?:

A VM is basically:

> “A computer inside your computer.”
> 

Virtualization support in BIOS gives Windows permission to create that “mini computer” efficiently.

So enabling it = **allowing hardware acceleration for virtual machines**.

Without this enabled:

- VMs may fail to start
- You may get errors like:
- *“VT-x is disabled in BIOS”*
- *“Hardware virtualization unavailable”*
- *“AMD-V is disabled”*
- VM performance becomes terrible or impossible

#### **Step 2: Disable Other Windows Virtualization:**

- Microsoft Hyperv
- Windows Hypervisor Platform
- Windows Subsystem for Linux
- Docker Desktop
- Virtual Machine Platform

Why?:

Windows itself uses virtualization technologies:

- Microsoft Hyper-V
- Windows Hypervisor Platform
- Windows Subsystem for Linux (WSL2 specifically)
- Docker Desktop
- Virtual Machine Platform

The problem is:

Only one “boss” can control CPU virtualization at a time

When Hyper-V/WSL2/Docker are active, Windows takes ownership of virtualization features.

Then software like VirtualBox or VMware says:

> “Hey… the CPU virtualization is already occupied.”
> 

This causes common problems like:

- VM won’t boot
- Very slow VM performance
- Random crashes
- Network issues
- “VT-x unavailable”
- “Raw-mode unavailable”
- “Hyper-V detected”
- VirtualBox freezing