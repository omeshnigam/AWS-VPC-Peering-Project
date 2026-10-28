# AWS VPC Peering — Private Connectivity Between Two VPCs

## 📌 Project Overview

This project demonstrates how to establish private network connectivity between two Amazon Virtual Private Clouds (VPCs) using **AWS VPC Peering**.

The project includes VPC configuration, subnet design, route table configuration, security group rules, private EC2 instance connectivity, and controlled connectivity failure and recovery testing.

## 🎯 Objectives

- Create two separate VPCs with non-overlapping CIDR blocks.
- Establish a VPC Peering connection between VPC A and VPC B.
- Configure forward and return routes.
- Deploy private EC2 instances without public IPv4 addresses.
- Configure a bastion host for secure SSH access.
- Verify private connectivity using ICMP ping.
- Test connectivity failure by temporarily removing an ICMP security group rule.
- Restore the rule and verify connectivity recovery.

## 🏗️ Architecture

**Logical network flow:**

```text
                   Laptop
                      |
                  SSH (22)
                      |
                Bastion Host
                Public Subnet
                 VPC A
                /      \
               /        \
         App-A          VPC Peering
      Private Subnet         |
        10.10.2.10           |
                             |
                          VPC B
                             |
                           App-B
                        Private Subnet
                         10.20.1.10
```

App-A and App-B communicate using their private IP addresses through the VPC Peering connection.

## 🛠️ AWS Services Used

- Amazon VPC
- VPC Peering
- Amazon EC2
- Subnets (Public and Private)
- Route Tables
- Internet Gateway
- Security Groups
- SSH and ICMP

## 🌐 Network Configuration

| Resource | CIDR / Private IP |
|---|---|
| VPC A | `10.10.0.0/16` |
| VPC B | `10.20.0.0/16` |
| VPC A Public Subnet | `10.10.1.0/24` |
| VPC A Private Subnet | `10.10.2.0/24` |
| VPC B Private Subnet | `10.20.1.0/24` |
| Bastion Host | `10.10.1.10` |
| App-A | `10.10.2.10` |
| App-B | `10.20.1.10` |
| AWS Region | Asia Pacific (Sydney) — `ap-southeast-2` |

## 🔐 Security Configuration

- SSH access to the bastion host is restricted to the administrator's public IP address.
- SSH access to private EC2 instances is allowed from the bastion host's private IP.
- ICMP Echo Request rules are configured to test communication between App-A and App-B.
- Private EC2 instances do not require public IPv4 addresses.
- Security group rules follow the principle of allowing only the required traffic for the lab.

## 🛣️ Routing Configuration

The route tables contain routes for communication between the two VPCs:

- **VPC A:** `10.20.0.0/16` → VPC Peering Connection
- **VPC B:** `10.10.0.0/16` → VPC Peering Connection

The required routes are configured in the route tables associated with the relevant subnets. Both forward and return routes are necessary for successful communication.

## 🧪 Connectivity Testing

### 1. App-A to App-B

Run the following command from App-A:

```bash
ping -c 4 10.20.1.10
```

Expected result: Successful ICMP replies when routing and security rules allow the traffic.

### 2. App-B to App-A

Run the following command from App-B:

```bash
ping -c 4 10.10.2.10
```

Expected result: Successful replies through the VPC Peering connection.

### 3. Private SSH Access Through Bastion

SSH ProxyJump is used to access private instances through the bastion host without assigning public IPv4 addresses to App-A and App-B.

### 4. Controlled Failure and Recovery

- Temporarily remove the App-B inbound ICMP Echo Request rule for App-A.
- Repeat the ping test from App-A and verify that connectivity fails.
- Restore the ICMP rule.
- Repeat the ping test and verify that connectivity recovers.

This test demonstrates the role of security groups in controlling network traffic.

## 📚 Key Learnings

- VPC Peering enables private communication between two VPCs.
- VPC Peering does not automatically configure route tables.
- Both forward and return routes are required.
- Security groups must allow the required traffic.
- An Active peering connection alone does not guarantee successful connectivity.
- Private EC2 instances can be accessed through a bastion host.
- VPC Peering is non-transitive.
- A VPC cannot use another VPC's Internet Gateway through VPC Peering.

## 📄 Project Report

The detailed project report includes the network configuration, routing tables, security group rules, connectivity tests, viva questions, and cleanup evidence.

**Report:** `OmeshNigam_EWB_VPC_Peering.pdf`

## 🧹 Cleanup

After completing the lab, the AWS resources created for the project were cleaned up to avoid unnecessary ongoing charges.

## 👨‍💻 Author

**Omesh Nigam**

AWS Networking | Cloud Computing | Hands-on Lab Project

---

*This project was created for learning and demonstrating AWS VPC networking concepts.*
