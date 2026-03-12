# 🚀 Complete Guide: Docker + NGINX + SSL (Let’s Encrypt)

This README is designed to help you **set up HTTPS properly**, understand each step, and **debug issues in the future**.

---

# 🧠 1. What You Are Building

You are setting up:

```
User → Domain → Server → Docker (NGINX) → Website
                     ↓
                SSL Certificate
```

Goal:

* `http://ilikeyou.in` → redirects to → `https://ilikeyou.in`
* Secure 🔒 connection using Let’s Encrypt

---

# 🌐 2. Prerequisites

### ✅ Domain setup (DNS)

```
A      @      65.1.124.160
CNAME  www    ilikeyou.in
```

Test:

```
ping ilikeyou.in
```

Expected:

* Resolves to your server IP

---

### ✅ Server (EC2 or VPS)

Must allow:

* Port 80 (HTTP)
* Port 443 (HTTPS)

---

### ✅ Docker installed

```
docker --version
```

---

# 🔐 3. Install Certbot (Correct Way)

Avoid old apt version.

```bash
sudo apt update
sudo apt install snapd -y
sudo snap install core
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

---

# 🌍 4. Fix Common Network Issue (IPv6)

If Certbot fails with timeout:

```bash
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
```

(Optional permanent fix):

```
sudo nano /etc/sysctl.conf
# Add:
net.ipv6.conf.all.disable_ipv6 = 1
sudo sysctl -p
```

---

# 🛑 5. Stop Services Using Port 80

Check:

```bash
sudo lsof -i :80
```

Stop anything running:

```bash
docker stop <container_id>
sudo systemctl stop nginx
```

---

# 📜 6. Generate SSL Certificate

```bash
sudo certbot certonly --standalone -d ilikeyou.in -d www.ilikeyou.in
```

Output:

```
/etc/letsencrypt/live/ilikeyou.in/fullchain.pem
/etc/letsencrypt/live/ilikeyou.in/privkey.pem
```

---

# 🐳 7. Run Docker with SSL Support

### 🔹 Important: Mount certificates

```bash
docker run -d \
-p 80:80 \
-p 443:443 \
-v /etc/letsencrypt:/etc/letsencrypt:ro \
your-image
```

---

# ⚙️ 8. NGINX Configuration (HTTPS)

```nginx
# HTTP → HTTPS redirect
server {
    listen 80;
    server_name ilikeyou.in www.ilikeyou.in;

    return 301 https://$host$request_uri;
}

# HTTPS server
server {
    listen 443 ssl;
    server_name ilikeyou.in www.ilikeyou.in;

    ssl_certificate /etc/letsencrypt/live/ilikeyou.in/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/ilikeyou.in/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~* \.(css|js|png|jpg|jpeg|gif|ico)$ {
        expires 7d;
        add_header Cache-Control "public";
    }

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
    add_header X-XSS-Protection "1; mode=block";
}
```

---

# 🔁 9. Auto Renewal

Test renewal:

```bash
sudo certbot renew --dry-run
```

⚠️ Problem:

* Renewal uses port 80
* Docker may block it

---

### ✅ Solution (manual safe way)

```bash
docker stop <container_id>
sudo certbot renew
docker start <container_id>
```

---

# 🧪 10. Debugging Guide (VERY IMPORTANT)

## 🔍 DNS Issues

```bash
ping ilikeyou.in
```

❌ No IP → DNS issue

---

## 🔍 Server Reachability

```bash
curl http://ilikeyou.in
```

❌ No response → server/firewall issue

---

## 🔍 Port Usage

```bash
sudo lsof -i :80
```

❌ Port busy → stop conflicting service

---

## 🔍 SSL API Connectivity

```bash
curl -v https://acme-v02.api.letsencrypt.org/directory
```

❌ Stuck → IPv6 problem

---

## 🔍 Container Issues

```bash
docker ps
docker logs <container_id>
```

---

## 🔍 NGINX Errors

```bash
docker exec -it <container_id> nginx -t
```

---

# ⚠️ Common Problems & Fixes

| Problem           | Cause                 | Fix                        |
| ----------------- | --------------------- | -------------------------- |
| Certbot timeout   | IPv6 issue            | Disable IPv6               |
| Port 80 in use    | NGINX/Docker conflict | Stop service               |
| HTTPS not working | Cert not mounted      | Use `-v`                   |
| Site not loading  | Wrong root path       | Check nginx config         |
| Renewal fails     | Port blocked          | Stop container temporarily |

---

# 🧠 Key Concepts You Learned

* DNS → maps domain to server
* Port 80 → HTTP
* Port 443 → HTTPS
* Certbot → proves domain ownership
* Docker volumes → share external data
* NGINX → serves website + SSL

---

# 🚀 Final Checklist

* [x] Domain resolves
* [x] Server accessible
* [x] SSL generated
* [x] Docker running with ports 80 & 443
* [x] Cert mounted
* [x] HTTPS working

---

# 🎯 Next Improvements (Optional)

* Auto-renew without downtime
* Reverse proxy (Nginx Proxy Manager / Traefik)
* CI/CD deployment
* HSTS security headers

---

# 💡 Golden Rule

> “Always debug layer by layer: DNS → Network → Port → Service → App”

---

If something breaks in future, follow the **debug section step-by-step** — you’ll always find the issue.
