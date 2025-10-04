**Azure Availability Sets** are a logical grouping capability for virtual machines (VMs) within a single Azure datacenter. Their purpose is to ensure application **high availability** by distributing the VMs across separate physical hardware, which mitigates the risk of a single point of failure affecting all instances.

***

## How Availability Sets Work: Domains

When you place two or more VMs into an Availability Set, the Azure platform automatically spreads them across isolated infrastructure groups known as **Fault Domains** and **Update Domains**.

### 1. Fault Domains (FDs)
A Fault Domain is a group of underlying physical hardware that shares a common **power source, cooling, and network switch**.
* **Protection:** By placing VMs in different Fault Domains (up to 3 in most regions), Azure ensures that a hardware failure in one rack (like a power or switch outage) only affects the VMs in that specific domain, leaving the others running.

### 2. Update Domains (UDs)
An Update Domain is a logical grouping of VMs and their underlying physical hardware that can be **rebooted simultaneously** during planned maintenance by Azure.
* **Protection:** Azure performs planned maintenance (such as OS patching for the host) on only one Update Domain at a time. This rolling update approach ensures that a portion of your application remains available while other parts are being updated. Azure waits at least 30 minutes for a domain to recover before moving to the next one.

***

## Importance and Benefits

| Feature | Description |
| :--- | :--- |
| **High Availability SLA** | Using two or more VMs in an Availability Set provides a **99.95%** Azure VM uptime Service Level Agreement (SLA), protecting against single-machine failures and planned maintenance. |
| **Resilience** | Protects your application from both **unplanned downtime** (hardware failures) and **planned downtime** (Azure host OS updates). |
| **Latency** | VMs in an Availability Set are physically close within the same datacenter, offering **lower VM-to-VM latency** compared to Availability Zones. |
| **Cost** | There is **no extra cost** for using an Availability Set itself; you only pay for the VM instances you create. |

***

## Availability Sets vs. Availability Zones

It is important to understand that Availability Sets only protect you from failures **within a single datacenter**. For greater resilience against an entire datacenter or region-wide failure, you would use **Availability Zones**.

| Feature | Availability Set | Availability Zone |
| :--- | :--- | :--- |
| **Scope of Protection** | Hardware failures and planned maintenance **within a single datacenter/cluster**. | Entire datacenter failures across **multiple physically separate zones** in a region. |
| **Physical Separation** | VMs are in separate racks (Fault Domains). | VMs are in separate buildings/datacenters with independent power/cooling. |
| **SLA** | 99.95% | 99.99% (when using multiple zones) |



## Azure Virtual Machine Scale Sets (VMSS)

**Azure Virtual Machine Scale Sets (VMSS)** allow you to create and manage a group of identical, load-balanced Virtual Machines (VMs). They provide the ability to automatically increase or decrease the number of VM instances in response to demand or a defined schedule.

Scale Sets are designed to provide **high availability**, **application resiliency**, and **simplified management** for large-scale applications.

***

## Key Features and Benefits

* **Autoscaling:** This is the most significant feature. VMSS can automatically scale **horizontally** (scale out by adding VMs, or scale in by removing VMs) based on performance metrics like CPU usage, memory consumption, or network traffic. This helps you efficiently meet fluctuating customer demand and optimize costs.
* **High Availability and Resiliency:** By running multiple instances of your application across different VMs and distributing them across **Availability Zones** or **Fault Domains**, VMSS ensures that if one VM instance fails, customers can still access your application through the others with minimal disruption.
* **Centralized Management:** All VM instances in a scale set are created from the same base image and configuration. This consistency simplifies central management, configuration, and updates for hundreds or even thousands of VMs.
* **Load Balancing:** Scale Sets natively integrate with Azure Load Balancer (for Layer-4 traffic distribution) and Azure Application Gateway (for more advanced Layer-7 traffic distribution and TLS termination) to evenly distribute incoming traffic across all VM instances.
* **Two Orchestration Modes:**
    * **Uniform:** All VM instances are identical. Ideal for stateless web workloads and large-scale container orchestration (like Azure Kubernetes Service).
    * **Flexible:** Allows diverse configurations (different VM sizes and images) and gives you full control over individual VMs, making it suitable for mixed-use or stateful applications.

***

## Common Use Cases

Azure Virtual Machine Scale Sets are an ideal choice for:

* **Large-scale Web Applications and APIs:** Scaling a web front-end based on user traffic.
* **Batch Processing Jobs:** Spinning up many VMs to run a high-volume compute task and then scaling down when finished.
* **Microservices:** Deploying containerized or mixed-role applications, often using the Flexible orchestration mode.
* **Big Data Processing:** Scaling compute resources dynamically during heavy data crunching tasks.

***

This video introduces Azure Virtual Machine Scale Sets and explains how they help to increase application availability and scalability.

[Increase app availability and scalability | Azure Virtual Machine Scale Sets](https://www.youtube.com/watch?v=lE2xJXYHnB8)
http://googleusercontent.com/youtube_content/0

For mission-critical applications, the best practice is often to use **Availability Zones** to deploy across multiple physical locations, or to use **Virtual Machine Scale Sets** with an option for multi-zone deployment.

The video below offers a detailed comparison between Availability Sets and Availability Zones for achieving high availability in Azure. [Azure Availability Sets vs. Availability Zones: Which Should You Use?](https://www.youtube.com/watch?v=Fcfl4XyLlNc)
http://googleusercontent.com/youtube_content/0
