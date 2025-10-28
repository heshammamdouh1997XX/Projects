# Generate TLS certificate

## Option A — using your internal CA (recommended in companies)

If your organization has an internal Certificate Authority, request a certificate for your n8n server’s hostname (e.g., n8n.mycompany.local).

You’ll receive two files:

```
n8n.crt   (public certificate)
n8n.key   (private key)
```

## Option B — generate a self-signed certificate (for internal use)

If you don’t have a CA, create one yourself:
```
mkdir -p ~/.n8n/certs
cd ~/.n8n/certs

openssl req -x509 -newkey rsa:4096 -sha256 -nodes \
  -keyout n8n.key \
  -out n8n.crt \
  -days 365 \
  -subj "/CN=n8n.mycompany.local"
```

Now you have:
```
~/.n8n/certs/n8n.crt
~/.n8n/certs/n8n.key
```

📎 Tip: Add n8n.mycompany.local to your local DNS or hosts file so it resolves internally.

## Configure n8n to use HTTPS/TLS

You can do this with environment variables or a config file.

### Option A — via environment variables (most common)

Run n8n with these:
```
export N8N_PROTOCOL=https
export N8N_PORT=5678
export N8N_SSL_KEY=/home/youruser/.n8n/certs/n8n.key
export N8N_SSL_CERT=/home/youruser/.n8n/certs/n8n.crt

n8n
```

Now it will serve securely at:
`https://n8n.mycompany.local:5678`

### Option B — using .env file (recommended for production) (Companies)

In your n8n directory (e.g., /home/n8n/.n8n), create a file named .env:
```
N8N_PROTOCOL=https
N8N_PORT=443
N8N_SSL_KEY=/home/n8n/.n8n/certs/n8n.key
N8N_SSL_CERT=/home/n8n/.n8n/certs/n8n.crt
N8N_HOST=n8n.mycompany.local
N8N_EDITOR_BASE_URL=https://n8n.mycompany.local
N8N_PUBLIC_API_DISABLED=false

n8n
```

## Trust the certificate

On other computers inside your company:

- If you used an internal CA, install its root certificate on those machines.

- If self-signed, import n8n.crt into their trusted root store (so browsers don’t warn about it).

## Optional — behind reverse proxy (recommended)

If you already have Nginx or Traefik in your network, it’s cleaner to:

Run n8n on `http://localhost:5678`

Let Nginx handle HTTPS + certificates.

**Example Nginx config:**
```
server {
    listen 443 ssl;
    server_name n8n.mycompany.local;

    ssl_certificate /etc/ssl/certs/n8n.crt;
    ssl_certificate_key /etc/ssl/private/n8n.key;

    location / {
        proxy_pass http://localhost:5678;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### After setup

You should be able to access:
`https://n8n.mycompany.local`

---

n8n is internal-only and will be used by other people inside your local network, the best and most secure setup is to run n8n behind Nginx (reverse proxy) handling HTTPS/TLS.
This gives you:

✅ Proper HTTPS (no browser warnings once trusted internally)

✅ Easier certificate management (no need to restart n8n when renewing certs)

✅ Optional authentication / access control via Nginx

✅ Central logging & scaling

Let’s set it up step-by-step 👇

Keep n8n running on HTTP locally

Run n8n only on localhost (no TLS inside the app):
```
export N8N_PROTOCOL=http
export N8N_PORT=5678
export N8N_HOST=localhost
n8n
```
## 2️⃣ Install Nginx
```
sudo apt update
sudo apt install nginx -y
```
Enable it
```
sudo systemctl enable nginx
sudo systemctl start nginx
```
## 3️⃣ Create internal TLS certificates

If your company has an internal CA, request a cert for:
`n8n.mycompany.local`

If not, create a self-signed certificate:
```
sudo mkdir -p /etc/nginx/ssl
cd /etc/nginx/ssl

sudo openssl req -x509 -nodes -newkey rsa:4096 \
  -keyout n8n.key \
  -out n8n.crt \
  -days 730 \
  -subj "/CN=n8n.mycompany.local"
```

## 4️⃣ Configure Nginx for n8n

Create a config file:
`sudo nano /etc/nginx/sites-available/n8n.conf`

```
server {
    listen 443 ssl;
    server_name n8n.mycompany.local;

    ssl_certificate     /etc/nginx/ssl/n8n.crt;
    ssl_certificate_key /etc/nginx/ssl/n8n.key;

    # (Optional) tighten security
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;

    # Proxy to local n8n
    location / {
        proxy_pass http://127.0.0.1:5678;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;

        # Optional security headers
        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-Content-Type-Options "nosniff";
    }

    access_log /var/log/nginx/n8n-access.log;
    error_log /var/log/nginx/n8n-error.log;
}

server {
    listen 80;
    server_name n8n.mycompany.local;
    return 301 https://$host$request_uri;
}
```
Enable it :
```
sudo ln -s /etc/nginx/sites-available/n8n.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

Now Nginx listens on HTTPS:443 and forwards to n8n:5678 internally.

## 5️⃣ Set hostname for your LAN users

Add a DNS entry (if you have an internal DNS server), or for testing add this line to each client’s `/etc/hosts` or `C:\Windows\System32\drivers\etc\hosts`:
```
192.168.x.x   n8n.mycompany.local
```
`https://n8n.mycompany.local`

## 6️⃣ Make your certificate trusted (so no warnings)

### Option A – Internal CA:
If your IT department runs an internal Certificate Authority, sign the cert with that CA and distribute the CA root certificate to all company PCs.

### Option B – Self-signed:
Manually install /etc/nginx/ssl/n8n.crt into each user’s trusted certificate store.

For example:

🪟 Windows: “Trusted Root Certification Authorities” → Import Certificate

🐧 Linux: /usr/local/share/ca-certificates/ + sudo update-ca-certificates

🍏 Mac: Keychain Access → System → Certificates → Import
