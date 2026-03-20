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

# ⚠️ Important Safety Rule

> Always allow SSH BEFORE applying DROP policy, otherwise you will lose access.

---

# 🧹 STEP 1 — Reset Existing Firewall (Clean Start)

```bash
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X

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

Expected:

* SSH allowed
* HTTP/HTTPS allowed
* Localhost allowed
* Default DROP policy

---

# ❌ How to Remove Rules

## View rules with line numbers:

```bash
iptables -L INPUT --line-numbers
```

## Delete a rule:

```bash
iptables -D INPUT <rule-number>
```

Example:

```bash
iptables -D INPUT 4
```

---

# ➕ How to Add New Port

Example: Allow port 8080

```bash
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
```

---

# 💾 STEP 5 — Save Firewall Rules

Install persistence:

```bash
apt install iptables-persistent -y
```

Save rules:

```bash
netfilter-persistent save
```

---

# 🛡️ UFW (Alternative Firewall)

## Install UFW

```bash
apt install ufw -y
```

---

## Default Policies

```bash
ufw default deny incoming
ufw default allow outgoing
```

---

## Allow Required Ports

```bash
ufw allow 22125/tcp
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 9090/tcp
ufw allow 8090/tcp
```

---

## Enable UFW

```bash
ufw enable
```

---

## Check Status

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

> Do NOT use both iptables and UFW together. Choose ONE firewall system.

---

# 🔍 Testing Ports

---

## 🖥️ Internal Testing (Inside VPS)

### Using Netcat

Start listener:

```bash
nc -l -p 9090
```

Test:

```bash
curl http://localhost:9090
```

---

## 🌍 External Testing (From Local Machine)

```bash
curl http://<your-vps-ip>:9090
```

---

## 🔎 Using Telnet

```bash
telnet <your-vps-ip> 9090
```

---

## 🚀 Using Nmap (Advanced)

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

* Always allow SSH first
* Rule order matters
* Avoid mixing firewall systems
* Use minimal rules for better debugging
* Test both internally and externally

---

# 🚀 Outcome

✔ Secure VPS firewall
✔ Clean and minimal rules
✔ No control panel interference
✔ Easy debugging and maintenance

---

# 👨‍💻 Author

**Aditya Shivshankar Vishwakarma**
DevOps | AWS | Cloud

---

If you want next level upgrade, I can also add:

* 🔐 Fail2Ban setup
* 🌍 Geo-blocking
* 🛡️ Rate limiting (DDoS protection)

Just tell me 👍
