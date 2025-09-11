# 4. Security Group and Network ACL:

## Layered Security: The Virtual Firewalls of Your VPC

Security in AWS networking is implemented in layers. The two primary services for controlling traffic are **Security Groups** (instance-level firewall) and **Network ACLs** (subnet-level firewall). They serve different purposes and are often used together for defense in depth.

## Security Groups (SG)

A **Security Group** acts as a virtual **stateful firewall** for your EC2 instances (and other resources like RDS).

### Key Characteristics (Stateful):
- **Operates at the Instance Level:** You assign SGs to individual EC2 instances.
- **Stateful:** If you allow an inbound request, the response is automatically allowed to flow back out, regardless of outbound rules.
- **All-Implicit-Deny:** The default rule is to **DENY ALL** traffic. You must add explicit **ALLOW** rules.
- **Evaluate ALL Rules:** All rules in the SG are evaluated before deciding whether to allow traffic.
- **Can Reference Other SGs:** You can allow traffic from another security group, making rules dynamic as instances are added/removed from that group.

### Example Security Group Rules:
| Type  | Protocol | Port Range | Source     | Description |
| :---- | :------- | :--------- | :--------- | :---------- |
| SSH   | TCP      | 22         | `203.0.113.1/32` | Allow SSH from my IP |
| HTTP  | TCP      | 80         | `0.0.0.0/0`      | Allow HTTP from anywhere |
| HTTPS | TCP      | 443        | `0.0.0.0/0`      | Allow HTTPS from anywhere |
| All traffic | All | All | `sg-123abc` | Allow all traffic from instances in SG `sg-123abc` |

## Network ACLs (NACL):

A **Network Access Control List** is an optional **stateless firewall** that operates at the **subnet level**.

### Key Characteristics (Stateless):
- **Operates at the Subnet Level:** You assign NACLs to entire subnets.
- **Stateless:** Inbound and outbound rules are evaluated independently. A response to an allowed inbound request must be explicitly allowed by an outbound rule.
- **Explicit-Deny:** The default NACL allows all traffic. You add rules to **explicitly DENY** or **ALLOW** traffic.
- **Rule Number Evaluation:** Rules are processed in order from the lowest number to the highest. The first rule that matches is applied.
- **Can Allow/Deny Specific IPs:** Useful for creating a quick block for a malicious IP across an entire subnet.

### Example NACL Rules (Inbound):
| Rule # | Type  | Protocol | Port Range | Source     | Allow/Deny |
| :----- | :---- | :------- | :--------- | :--------- | :--------- |
| 100    | HTTP  | TCP      | 80         | `0.0.0.0/0` | ALLOW      |
| 200    | SSH   | TCP      | 22         | `203.0.113.0/24` | ALLOW   |
| 300    | ALL   | ALL      | ALL        | `192.0.2.0/24`   | DENY     |
| *      | ALL   | ALL      | ALL        | `0.0.0.0/0`      | DENY     | *<-- Default Catch-All Rule* |

## How They Work Together: The Stadium Analogy

Imagine your VPC is a stadium:
- The **Network ACL (NACL)** is the security at the **main gates**. They check everyone entering and leaving the entire stadium (subnet).
- The **Security Group (SG)** is the security guard at the **door to a specific luxury box** (EC2 instance). They check anyone trying to enter or leave that specific box.

A request must pass both the main gate (NACL) and the box door (SG) to reach an instance.

## Comparison Table:

| Feature | Security Group (SG) | Network ACL (NACL) |
| :--- | :--- | :--- |
| **Scope** | Instance level | Subnet level |
| **State** | **Stateful** (Return traffic is automatically allowed) | **Stateless** (Return traffic must be explicitly allowed) |
| **Rule Type** | Allow rules only | Allow AND Deny rules |
| **Evaluation** | Evaluate all rules before decision | Process rules in order (lowest number first) until a match is found |
| **Default** | **Implicit DENY** (All inbound traffic is blocked by default) | **Default NACL: Implicit ALLOW** (Custom NACL: Implicit DENY) |

## Best Practice: Defense in Depth

Use both services together for layered security:
1.  Use **NACLs** as a first line of defense for broad, subnet-level allow/deny rules (e.g., block a malicious IP range from the entire subnet).
2.  Use **Security Groups** for fine-grained, instance-level security (e.g., only allow the web server SG to talk to the database SG on port 3306).
