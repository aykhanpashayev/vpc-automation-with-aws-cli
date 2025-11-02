# VPC Automation with AWS CLI

[![AWS](https://img.shields.io/badge/AWS-CLI-orange)](https://aws.amazon.com/cli/) [![Shell](https://img.shields.io/badge/Shell-Bash-blue)]() [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A simple yet professional collection of **Bash scripts** to automate creating and deleting AWS VPC components step by step — including the VPC, Internet Gateway, Subnets, and Route Tables. Designed to be easy to follow for learners while clean and structured enough.

---

## ✨ Features

* Clear, minimal scripts with proper usage examples
* Safe deletion order to avoid dependency errors
* Comments and structure designed for learning AWS fundamentals
* Consistent and reusable naming conventions
* Uses AWS CLI only (no dependencies beyond Bash)

---

## 🧭 What You'll Build

You’ll automate a small, production-like VPC environment that includes:

* 1 VPC
* 1 Internet Gateway (IGW)
* 1 Public Subnet
* 1 Route Table with a default route to the IGW

---

## 🧰 Prerequisites

* **AWS account** and IAM user with permissions to manage EC2 and VPC resources
* **AWS CLI v2** configured with `aws configure`
* **Bash** shell (Linux/macOS/WSL)

---

## 📂 Repository Structure

```
.
├── LICENSE
├── README.md
└── vpc-automation-scripts/
├── create-vpc.sh
├── create-attach-igw.sh
├── create-subnet.sh
├── create-route-table.sh
├── delete-detach-igw.sh
├── delete-route-table.sh
└── delete-vpc.sh
```

---

## ⚙️ Step-by-Step Usage

### 1️⃣ Create a VPC

```bash
./create-vpc
```

This creates a new VPC and outputs its ID.

---

### 2️⃣ Create and Attach an Internet Gateway

```bash
./create-attach-igw <vpc-id>
```

This creates an IGW and attaches it to your VPC.

---

### 3️⃣ Create a Public Subnet

```bash
./create-subnet <vpc-id> [cidr-block]
```

Example:

```bash
./create-subnet vpc-0abc1234def567890 172.16.1.0/24
```

This creates a subnet in `us-east-2a` and automatically enables public IP mapping.

---

### 4️⃣ Create a Route Table and Add a Route to IGW

```bash
./create-route-table --vpc <vpc-id> --igw <igw-id> -s <subnet-id>
```

Example:

```bash
./create-route-table --vpc vpc-0abc1234def567890 --igw igw-0abc1234567890def -s subnet-0123456789abcdef
```

This creates a route table, adds a default route to the IGW, and associates it with your subnet.

---

## 🧹 Cleanup / Deletion Order

To safely delete everything without dependency errors, follow this order:

1. **Delete Route Table**

   ```bash
   ./delete-route-table <route-table-id>
   ```
2. **Detach and Delete Internet Gateway**

   ```bash
   ./delete-detach-igw <vpc-id> <igw-id>
   ```
3. **Delete VPC**

   ```bash
   ./delete-vpc <vpc-id>
   ```

> ⚠️ Ensure all EC2 instances, NAT gateways, and ENIs inside subnets are removed before deleting the VPC.

---

## 🧩 Script Overview

| Script                  | Description                                                             |
| ----------------------- | ----------------------------------------------------------------------- |
| `create-vpc`            | Creates a VPC and enables DNS support and hostnames.                    |
| `create-attach-igw`     | Creates and attaches an Internet Gateway.                               |
| `create-subnet`         | Creates a public subnet and enables auto-assign public IPs.             |
| `create-route-table`    | Creates a route table, adds a route to IGW/NAT, and associates subnets. |
| `delete-detach-igw`     | Safely detaches and deletes an Internet Gateway.                        |
| `delete-route-table`    | Safely deletes a route table, removing routes and associations.         |
| `delete-vpc`            | Deletes the VPC after dependencies are removed.                         |

---

## 🔐 Notes & Best Practices

* Always delete resources in the **reverse order** of creation.
* Use **private CIDR ranges** (10.x.x.x, 172.16.x.x, or 192.168.x.x).
* Avoid deleting the **main route table** — AWS will block this.
* To make scripts reusable, you can add environment variables or `.env` support.

---

## 🧠 Learning Takeaways

This project helps you understand:

* AWS VPC structure and relationships
* Order of dependency removal in network teardown
* How to automate networking tasks safely with Bash and AWS CLI

---

## 📄 License

MIT License. You’re free to reuse or modify the scripts for learning or demos.
