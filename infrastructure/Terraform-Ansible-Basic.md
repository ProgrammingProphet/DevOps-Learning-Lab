# 🚀 Infrastructure Automation with Terraform & Ansible

This project demonstrates how to:

* Provision infrastructure using **Terraform**
* Configure servers using **Ansible**
* Automate EC2 deployment securely
* Follow DevOps best practices

---

# 🏗 Architecture Overview

1. Terraform provisions:

   * VPC
   * Subnet
   * Internet Gateway
   * Security Group
   * EC2 Instance

2. Ansible:

   * Connects via SSH
   * Installs packages (e.g., Nginx)
   * Configures server

---

# 📂 Project Structure

```
project/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── terraform.tfvars
│
├── ansible/
│   ├── inventory.ini
│   └── playbook.yml
│
└── README.md
```

---

# ⚙️ Prerequisites

Install:

* Terraform
* Ansible
* AWS CLI
* SSH Key
* AWS account configured (`aws configure`)

---

# 🌍 Terraform – Infrastructure Provisioning

## 1️⃣ Initialize Terraform

```bash
terraform init
```

Downloads required providers.

---

## 2️⃣ Validate Configuration

```bash
terraform validate
```

Checks syntax correctness.

---

## 3️⃣ Preview Changes

```bash
terraform plan
```

Shows what will be created/modified.

---

## 4️⃣ Apply Infrastructure

```bash
terraform apply
```

Creates infrastructure.

---

## 5️⃣ Destroy Infrastructure

```bash
terraform destroy
```

Deletes all created resources.

---

## 6️⃣ Terraform State Commands

Check state:

```bash
terraform state list
```

Show specific resource:

```bash
terraform state show aws_instance.example
```

---

# 🔐 Example Security Group (SSH Restricted)

```hcl
ingress {
  from_port   = 22
  to_port     = 22
  protocol    = "tcp"
  cidr_blocks = ["YOUR_PUBLIC_IP/32"]
}
```

---

# 🛠 Ansible – Server Configuration

After Terraform creates EC2, use Ansible to configure it.

---

## 1️⃣ Inventory File (`inventory.ini`)

```
[web]
YOUR_EC2_PUBLIC_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/devops-key
```

---

## 2️⃣ Test Connectivity

```bash
ansible -i inventory.ini web -m ping
```

If successful:

```
pong
```

---

## 3️⃣ Sample Playbook (`playbook.yml`)

```yaml
---
- name: Configure Web Server
  hosts: web
  become: yes

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes
```

---

## 4️⃣ Run Playbook

```bash
ansible-playbook -i inventory.ini playbook.yml
ansible-playbook -i inventory.ini playbooks/deploy.yml
```

---

# 🔄 Full Workflow

1. `terraform apply`
2. Get EC2 public IP
3. Update Ansible inventory
4. `ansible-playbook playbook.yml`
5. Access web server in browser

---

# 🛡 Security Best Practices

* Do NOT commit `.pem` files
* Do NOT commit `terraform.tfstate`
* Use `.gitignore`
* Restrict SSH with `/32`
* Use SSM in production

---

# 📌 Common Terraform Commands

| Command            | Purpose            |
| ------------------ | ------------------ |
| terraform init     | Initialize project |
| terraform plan     | Preview changes    |
| terraform apply    | Apply changes      |
| terraform destroy  | Delete infra       |
| terraform fmt      | Format code        |
| terraform validate | Validate config    |

---

# 📌 Common Ansible Commands

| Command                  | Purpose          |
| ------------------------ | ---------------- |
| ansible -m ping          | Test connection  |
| ansible-playbook         | Run playbook     |
| ansible-inventory --list | Show inventory   |
| ansible-doc module       | View module docs |

---

# 🎯 Learning Goals

* Infrastructure as Code (IaC)
* Immutable Infrastructure
* Configuration Management
* SSH Security
* DevOps Automation Workflow

---

# 🔥 Next Improvements

* Use Terraform remote backend (S3)
* Automate inventory generation
* Use dynamic inventory (AWS plugin)
* Remove SSH and use SSM
* Integrate into CI/CD pipeline

---

If you want, I can now:

* Upgrade this to production-ready README
* Add CI/CD integration section
* Add dynamic inventory for Ansible
* Or explain Terraform vs Ansible differences deeply
