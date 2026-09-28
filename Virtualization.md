# Virtualization - My Learning Notes
I'm currently learning virtualization as part of my preparation for an infrastructure focused internship

## What is Virtualization
Virtualization is an interesting foundational concept and also practical technology that enables creation of virtual environments from single physical machine

## Why is Virtualization used
because it allows more efficient use of resources by distributing them across computing environments

## What is a Virtual Machine
AKA 'guest', is a virtual environment that simulates a physical computer in a software form.

## What is a Hypervisor
A hypervisor is software layer that cordinates the VMs, It act as a interface between VMs and Physical Hardware.

### there are Two types of Hypervisors
1. Type 1 : Runs directly on the host computer's physical hardware without needing an host operating system.
2. Type 2 : runs as a software application on top of a operating system(Windows, macOS or Linux)

### and other types
- Type 0 : not on software layer, it is a firmware based virtualization feature built directly into the physical hardware. virtualization logic is hard coded into the system's firmware. Example: IBM LPARs
- Hybrid : blends characteristics of both Type 1 and Type 2 models. they loaded like a normal application on a operating system, but once executed , they work on top of hardware architecture to achieve bare metal efficiency. similer to type 1.

## My practical Experience
i used virtualbox to create a virtual machine
- Host: Windows 11
- Guest OS: CentOS 7

### Problem 
while working with my CentOS virtual machine in VirtualBox, the guest OS was not receiving an ip address on the network interface `enp0s3`

i checked the interface using
`ip addr`

<img width="842" height="277" alt="Screenshot 2026-09-28 213009" src="https://github.com/user-attachments/assets/41f79d4b-cfe5-4729-8738-480f10920306" />

the interface `enp0s3` was available, but it did not have an ip address assigned.

### Investigation
i first checked the network interface, and noticed that enp0s3 did not have an IPv4 address

i then cheched the VirtuaBox network configuration and changed the adapter to NAT.

After changing the network mode, i requested an IP address usign:
`dhclient enp0s3`

i checked the interface again 

`ip addr`
the interface received an ip address

<img width="686" height="198" alt="image" src="https://github.com/user-attachments/assets/b05d9b69-dfba-4032-a554-5756b48b451d" />

### Solution 
the solution was
1. Change the VirtualBox network adapter to NAT
2. Run DHCP request on the enp0s3 interface
   `dhclient enp0s3`
3. Verify the assigned IP address
   `ip addr`

I changed the VirtualBox adapter to NAT and manually requested an IP using dhclient. After this, enp0s3 received an IP address and connectivity was restored

### Verification
i tested network connectivity after receiving the IP address
`ping 8.8.8.8`

<img width="547" height="187" alt="image" src="https://github.com/user-attachments/assets/9eab2812-7ba5-47ad-b08a-6e1693bb0628" />

The connection was successful



## What i learned 
This problem helped me understand that a VM needs both a correctly configured virtual network adapter and a network config inside the guest os.

also learned that `dhclient` can be used to request an ip address from DHCP server for network interface.

in this case VirtualBox NAT provided the VM with access to a DHCP service , allowing `enp0s3` to obtain an ip address
