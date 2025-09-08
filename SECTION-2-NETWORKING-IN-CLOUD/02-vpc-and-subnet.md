# 2. VPC and Subnet

## Your Virtual Data Center in the Cloud

An Amazon Virtual Private Cloud (VPC) is your very own logically isolated section of the AWS cloud. Think of it as your **private data center** or a **virtual network** where you have complete control over the IP address range, subnets, routing, and security.

## What is a VPC?

A VPC allows you to launch AWS resources (like EC2 instances) into a virtual network that you define. It provides:
- **Logical Isolation:** Your VPC is private and isolated from all other VPCs and users on AWS by default.
- **Custom IP Address Range:** You define the primary IP address range (CIDR block) for your VPC (e.g., `10.0.0.0/16`).
- **Complete Control:** You have full control over the virtual networking environment, including routing, security, and access.

## What is a Subnet?

A **subnet** is a subdivision of your VPC's IP address range. You place resources into subnets, typically based on their function and connectivity requirements. A subnet must reside within a single **Availability Zone (AZ)**.

### Public vs. Private Subnets

The key difference is **internet connectivity**.

| | Public Subnet | Private Subnet |
| :--- | :--- | :--- |
| **Purpose** | Holds resources that need direct access to the internet (e.g., web servers, NAT gateways). | Holds resources that should not be directly accessible from the internet (e.g., databases, application servers). |
| **Routing** | Has a **route** in its route table that sends internet-bound traffic (`0.0.0.0/0`) to an **Internet Gateway (IGW)**. | Does **not** have a direct route to an IGW. It may have a route to a **NAT Gateway** for outbound-only internet access. |
| **Example CIDR** | `10.0.1.0/24` | `10.0.2.0/24` |

## The Default VPC

For getting started quickly, AWS automatically creates a **default VPC** in every region for your account. It comes with:
- A default CIDR block (`172.31.0.0/16`).
- A public subnet in each Availability Zone.
- An Internet Gateway already attached and configured.
- Default route tables and security groups.

While convenient for learning, **production workloads should always use a custom VPC** for precise control over networking and security.

## Best Practices for VPC and Subnet Design

1.  **Plan Your IP Addressing:** Choose a CIDR block large enough for future growth but that doesn't overlap with any other networks you plan to connect to (e.g., your corporate network).
2.  **Use Multiple Availability Zones:** Create at least one subnet in multiple AZs for high availability and fault tolerance.
3.  **Use a Consistent Strategy:** Use a clear naming convention (e.g., `public-subnet-a`, `private-db-subnet-b`) and tagging strategy for your subnets.

## Visualizing a VPC
+-----------------------------------------------------------------------+
| My VPC (10.0.0.0/16) |
| |
| +-----------------------------+ +-----------------------------+ |
| | Availability Zone A | | Availability Zone B | |
| | | | | |
| | +-----------------------+ | | +-----------------------+ | |
| | | Public Subnet | | | | Public Subnet | | |
| | | 10.0.1.0/24 | | | | 10.0.3.0/24 | | |
| | | <----->| | | | |
| | | [Web Server] | | | | [Web Server] | | |
| | +-----------------------+ | | | +-----------------------+ | |
| | | | | | |
| | +-----------------------+ | | | +-----------------------+ | |
| | | Private Subnet | | | | | Private Subnet | | |
| | | 10.0.2.0/24 | | | | | 10.0.4.0/24 | | |
| | | <----->| | | | | |
| | | [Database] | | | | | [App Server] | | |
| | +-----------------------+ | | | +-----------------------+ | |
| +-----------------------------+ +-----------------------------+ |
| |
+-----------------------------------------------------------------------+
^ ^
| |
| (via Internet Gateway) | (No direct internet access)
| |
+-----------------------------------------------------------------------+
| The Internet |
+-----------------------------------------------------------------------+

text

---

**Next:** [Route Table and NAT Gateway](./03-route-table-and-nat-gateway.md)
