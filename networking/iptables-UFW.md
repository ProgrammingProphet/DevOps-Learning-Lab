# 🔐 VPS Firewall Configuration Guide (iptables + UFW)

This document provides a **step-by-step DevOps guide** to configure a secure firewall on an Ubuntu VPS using **iptables** and **UFW**, including testing and troubleshooting.

---

# 🎯 Objective

* Secure VPS using firewall rules
* Allow only required ports
* Prevent lockouts
* Enable debugging ports
* Provide testing methods

---

# 🧠 Core Concept (VERY IMPORTANT)

## 🔹 Policy vs Rules

### 🔒 Policy (Default Behavior)

Policy defines what happens **if no rule matches**.

Example:

```bash
iptables -P INPUT DROP
```

👉 Meaning:

> If no rule allows the traffic → it will be **blocked**

---

### ✅ Rules (Specific Allow/Deny)

Rules define **exceptions to the policy**

Example:

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

👉 Meaning:

> Allow HTTP traffic

---

## 🎯 Summary

| Type   | Meaning        |
| ------ | -------------- |
| Policy | Default action |
| Rules  | Exceptions     |

---

## ⚠️ Critical Understanding

> If you remove all rules while policy is DROP → **everything gets blocked (including SSH)**

---

# ⚠️ Firewall Reset Warning (IMPORTANT)

Running this:

```bash
iptables -F
```

❌ Removes all rules
❌ Keeps policy unchanged

👉 If policy = DROP → **SSH will disconnect immediately**

---

## ✅ Safe Reset Method

```bash
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X

nft flush ruleset
```

---

## 🛡️ Auto-Recovery Trick (Pro Tip)

```bash
sleep 60 && iptables -F &
```

👉 If something goes wrong, firewall resets after 60 seconds

---

# 🧹 STEP 1 — Clean Firewall (Safe Way)

```bash
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

iptables -F
iptables -X
nft flush ruleset
```

---

# 🔥 STEP 2 — Add Firewall Rules (Safe Order)

## ✅ 1. Allow SSH (CRITICAL)

```bash
iptables -A INPUT -p tcp --dport 22125 -j ACCEPT
```

---

## ✅ 2. Allow Established Connections

```bash
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

---

## ✅ 3. Allow Localhost

```bash
iptables -A INPUT -i lo -j ACCEPT
```

---

## ✅ 4. Allow Web Traffic

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

---

## ✅ 5. Allow Debug Ports

```bash
iptables -A INPUT -p tcp --dport 9090 -j ACCEPT
iptables -A INPUT -p tcp --dport 8090 -j ACCEPT
```

---

# 🔒 STEP 3 — Apply Default Policies

```bash
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
```

---

# 🔍 STEP 4 — Verify Rules

```bash
iptables -L -n -v
```

---

# ❌ How to Remove Rules

```bash
iptables -L INPUT --line-numbers
iptables -D INPUT <rule-number>
```

---

# ➕ Add New Port

```bash
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
```

---

# 💾 Save Rules

```bash
apt install iptables-persistent -y
netfilter-persistent save
```

---

# 🛡️ UFW (Alternative Firewall)

## Install

```bash
apt install ufw -y
```

---

## Default Policy

```bash
ufw default deny incoming
ufw default allow outgoing
```

---

## Allow Ports

```bash
ufw allow 22125/tcp
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 9090/tcp
ufw allow 8090/tcp
```

---

## Enable

```bash
ufw enable
```

---

## Status

```bash
ufw status verbose
```

---

## Remove Rule

```bash
ufw delete allow 9090/tcp
```

---

# ⚠️ Important Note

> Do NOT use iptables and UFW together — choose one.

---

# 🔍 Testing Ports

---

## 🖥️ Internal (VPS)

```bash
nc -l -p 9090
curl http://localhost:9090
```

---

## 🌍 External (Your PC)

```bash
curl http://<your-vps-ip>:9090
```

---

## 🔎 Advanced (Nmap)

```bash
nmap -p 80,443,8080,9090 <your-vps-ip>
```

---

# 📊 Expected Results

| Port | Internal | External       |
| ---- | -------- | -------------- |
| 80   | ✅        | ✅              |
| 443  | ✅        | ✅              |
| 9090 | ✅        | ✅              |
| 8080 | ✅        | ❌ (if blocked) |

---

# 🧠 Key Learnings

* Policy = default action
* Rules = exceptions
* Always allow SSH first
* Never flush firewall without safe mode
* Avoid mixing firewall systems
* Keep configuration minimal

---

# 🚀 Outcome

✔ Secure VPS
✔ Clean firewall
✔ No control panel conflicts
✔ Easy debugging

---

# 👨‍💻 Author

**Aditya Shivshankar Vishwakarma**
DevOps | AWS | Cloud

<!---

If you want next level upgrade, I can also add:

* 🔐 Fail2Ban setup
* 🌍 Geo-blocking
* 🛡️ Rate limiting (DDoS protection)

Just tell me 👍-->
