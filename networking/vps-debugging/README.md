

# 🚀 VPS Networking Debugging & Nginx Deployment (Real DevOps Case Study)

This repository documents a real-world debugging journey while deploying a web server on a self-managed Ubuntu VPS.

The goal was to make a website accessible over the public internet using Nginx. During the process, multiple networking and firewall-related issues were identified and resolved.

---

# 📌 Problem Statement

- Nginx was running correctly
- Website was accessible locally (`localhost`)
- But NOT accessible via public IP



# 🔍 Debugging Approach

We followed a structured DevOps troubleshooting methodology:

---

## ✅ 1. Service Verification (Application Layer)

Checked if Nginx is running and listening:

```bash
ss -tulnp | grep :80
````

✔ Confirmed:

* Nginx running
* Listening on `0.0.0.0:80`

---

## ✅ 2. Internal Connectivity Test

```bash
curl http://localhost
curl http://<public-ip>
```

✔ Result:

* Working inside VPS

---

## ❌ 3. External Connectivity Issue

From local system:

```bash
curl http://<public-ip>
```

❌ Result:

* Connection refused

---

## ✅ 4. Firewall Verification (iptables)

Checked rules:

```bash
iptables -L -n -v
```

✔ Ports allowed:

* 80 (HTTP)
* 443 (HTTPS)
* 22125 (SSH)

---

## ✅ 5. Advanced Network Testing (Netcat)

Tested open ports:

```bash
nc -l -p 8080
```

✔ External traffic received → Network working

But:

```bash
nc -l -p 80
```

❌ No traffic received

👉 Conclusion: Port-specific issue

---

## 🔥 6. Root Cause Found (NAT Redirection)

Checked nftables:

```bash
nft list ruleset
```

### ❌ Found:

```bash
tcp dport 80 redirect to :8090
tcp dport 443 redirect to :8090
```

👉 This was leftover configuration from CyberPanel / LiteSpeed

---

# 💥 Root Cause

All HTTP/HTTPS traffic was being redirected:

```
Port 80 → Port 8090
Port 443 → Port 8090
```

👉 Port 8090 was not serving the website → causing failure

---

# ✅ Solution

Removed NAT redirect rules:

```bash
nft delete rule ip nat PREROUTING tcp dport 80 redirect to :8090
nft delete rule ip nat PREROUTING tcp dport 443 redirect to :8090
```

OR full reset:

```bash
nft flush table ip nat
```

---

## 🔁 Restart Services

```bash
systemctl restart nginx
```

---

# 🎯 Final Result

✔ Website accessible via public IP
✔ Nginx working correctly
✔ No redirection issues
✔ Ports properly exposed

---

# 🧠 Key Learnings

## 🔹 Port Debugging

* Always verify if the service is listening on correct ports

## 🔹 Firewall (iptables + nftables)

* Multiple firewall layers can exist
* iptables may show "allow", but nftables can override

## 🔹 NAT Redirection

* Hidden NAT rules can silently redirect traffic
* Especially after uninstalling control panels

## 🔹 Service Binding

* Ensure services bind to `0.0.0.0`, not `127.0.0.1`

## 🔹 Network Tracing

* Use tools like:

  * `curl`
  * `nc (netcat)`
  * `ss`
  * `nft`
  * logs (`access.log`)

---

# ⚠️ Important Lesson

> Removing software like CyberPanel does NOT remove its network configurations.

Always check:

* nftables
* iptables
* NAT rules

---

# 🛠️ Tech Stack

* Ubuntu 24.04 VPS
* Nginx
* iptables
* nftables
* Netcat (nc)

---

# 🚀 Outcome

This project demonstrates real-world DevOps skills:

✔ Network debugging
✔ Firewall analysis
✔ Root cause identification
✔ Production issue resolution

---

# 👨‍💻 Author

**Aditya Shivshankar Vishwakarma**
DevOps | AWS | Cloud

---

# ⭐ If this helped you

Give this repo a star ⭐ and share with others learning DevOps!

