# 3. Route Table and NAT Gateway

## The Traffic Directors of Your VPC

Route Tables and NAT Gateways work together to control the flow of network traffic within your VPC and to the outside world. They are what define a subnet as public or private.

## Route Tables

A **route table** contains a set of rules, called **routes**, that determine where network traffic from your subnet or gateway is directed. Every subnet in your VPC must be associated with a route table.

### How Routes Work
Each route in a route table has a **destination** (a CIDR block) and a **target** (where to send the traffic).

| Destination       | Target                  | Explanation                                                                 |
| :---------------- | :---------------------- | :-------------------------------------------------------------------------- |
| `10.0.0.0/16`     | `local`                 | **Mandatory.** Traffic for within the VPC stays local. Cannot be modified.  |
| `0.0.0.0/0`       | `igw-123abc`            | Send all other traffic (public internet) to the Internet Gateway.           |
| `0.0.0.0/0`       | `nat-456def`            | Send all other traffic to the NAT Gateway for outbound-only internet access.|
| `172.31.0.0/16`   | `pcx-789ghi`            | Send traffic destined for another VPC to a VPC Peering connection.          |

- The `local` route is what allows all subnets within a VPC to communicate with each other by default.
- The most important route for internet access is the `0.0.0.0/0` rule, which acts as the "default gateway" for the subnet.

## NAT Gateway (Network Address Translation)

A **NAT Gateway** allows resources in a **private subnet** to initiate outbound connections to the internet (e.g., to download software updates or access external APIs) while **preventing the internet from initiating inbound connections** to those resources.

### Key Characteristics of a NAT Gateway:
- **Lives in a Public Subnet:** It must be placed in a public subnet with a route to an Internet Gateway.
- **Uses Elastic IP:** It is assigned a static public IP address (an Elastic IP).
- **Managed Service:** AWS manages it for you. It is highly available and auto-scales.
- **Outbound Only:** Only allows initiated outbound traffic. An external service cannot use the NAT Gateway's IP to connect into your private instance.

## Public vs. Private Subnet Configuration

This is how the concepts come together to define a subnet's public/private nature:

**Public Subnet Route Table:**
| Destination       | Target          |
| :---------------- | :-------------- |
| `10.0.0.0/16`     | `local`         |
| `0.0.0.0/0`       | `igw-123abc`    | <-- **Route to IGW**

**Private Subnet Route Table:**
| Destination       | Target          |
| :---------------- | :-------------- |
| `10.0.0.0/16`     | `local`         |
| `0.0.0.0/0`       | `nat-456def`    | <-- **Route to NAT Gateway**

## Visualizing the Data Flow

**Public Instance (Web Server) to Internet:**
`Web Server (10.0.1.10)` -> `Route Table (route to igw)` -> `Internet Gateway` -> `Internet`

**Private Instance (App Server) to Internet:**
`App Server (10.0.2.10)` -> `Route Table (route to nat)` -> `NAT Gateway (in public subnet)` -> `Internet Gateway` -> `Internet`

**Internet to Private Instance:**
`Internet` -> `Internet Gateway` -> *...Request is dropped because no route exists...*

---

**Next:** [Security Group and Network ACL](./04-security-group-and-network-acl.md)
