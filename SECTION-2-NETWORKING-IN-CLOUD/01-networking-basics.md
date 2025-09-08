# 1. Networking Basics

## The Foundation of Cloud Networking

Before diving into AWS-specific services, it's crucial to understand the universal networking concepts that cloud providers build upon. This knowledge will make complex AWS services like VPC much easier to grasp.

## Core Concepts

### IP Addresses
An IP address is a unique identifier for a device on a network, much like a street address for your house.
- **IPv4:** The most common format (e.g., `192.168.0.1`). It consists of four numbers separated by dots.
- **IPv6:** A newer format designed to overcome the limited number of available IPv4 addresses (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`).

### CIDR Blocks (Classless Inter-Domain Routing)
A CIDR block is a compact way to specify a range of IP addresses and the network's routing prefix.
- **Format:** `<base IP address>/<prefix length>` (e.g., `10.0.0.0/16`)
- The **prefix length** (`/16`) indicates how many bits of the address define the network. A smaller number (e.g., `/16`) means a larger network. A larger number (e.g., `/28`) means a smaller, more specific network.
- Example: `10.0.0.0/16` allows for 65,536 IP addresses (`10.0.0.0` - `10.0.255.255`).

### Subnets (Sub-Networks)
A subnet is a logical subdivision of a larger IP network. Breaking a large network into smaller subnets provides several benefits:
- **Improved Security & Control:** Isolate groups of resources (e.g., put web servers in one subnet and databases in another).
- **Improved Performance:** Reduces network congestion.
- **Efficient Routing:** Makes network traffic management more efficient.

### Routing
Routing is the process of selecting a path for traffic to travel across a network. A **router** (or a **route table** in the cloud) is the device that directs this traffic, making decisions based on its destination IP address.

### Internet Gateway (IGW)
A gateway is a network node that connects two different networks. An **Internet Gateway** is a specific gateway that connects a private local network (like your VPC) to the public internet.

## Bringing It All Together: A Simple Analogy

Think of networking like a city's postal system:

| Networking Concept | Postal System Analogy |
| :--- | :--- |
| **IP Address** | A specific **street address** (`123 Main St`). |
| **CIDR Block** | A **ZIP code** (`10001`). It defines a large delivery area. |
| **Subnet** | A **neighborhood** within the ZIP code (`Green Heights`). This helps the post office sort mail more efficiently. |
| **Router / Route Table** | The **local post office** that decides which mail truck to use for which neighborhood. |
| **Internet Gateway** | The **major central post office** that handles mail going to and from other cities and countries. |

## Why This Matters for AWS:

In AWS, you don't physically plug in cables. Instead, you logically define these concepts:
- You define a **VPC** and assign it a large **CIDR block** (e.g., `10.0.0.0/16`).
- You divide your VPC's IP range into smaller **subnets** (e.g., `10.0.1.0/24`, `10.0.2.0/24`) and place them in different Availability Zones for high availability.
- You use **route tables** to define the rules for where traffic from each subnet is directed.
- You attach an **Internet Gateway** to your VPC to allow public subnets to communicate with the internet.

Understanding these basics is the key to designing secure, efficient, and well-architected cloud networks.

---

**Next:** [VPC and Subnet](./02-vpc-and-subnet.md)
