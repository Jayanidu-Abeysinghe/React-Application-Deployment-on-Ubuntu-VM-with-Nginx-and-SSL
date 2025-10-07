# 🚀 React Application Deployment on Ubuntu with Nginx and SSL (DuckDNS + Certbot)
This guide explains how to **deploy a React application** on an Ubuntu server (e.g., AWS EC2), serve it using **Nginx**, and secure it with a **free SSL certificate** via **DuckDNS** and **Let’s Encrypt (Certbot)**.

---

## 🧩 Prerequisites

- Ubuntu server (e.g., AWS EC2)
- Nginx installed and running
- Node.js and npm installed
- Public IP address (e.g., `44.249.138.187`)
- Git installed

---

## 🛠️ 1. Install Node.js and npm

Since React requires Node.js and npm, install them first:

```bash
sudo apt update
sudo apt install -y nodejs npm
```

Verify the installation:

```bash
node -v
npm -v
```
<img src="https://github.com/Jayanidu-Abeysinghe/React-Application-Deployment-on-Ubuntu-VM-with-Nginx-and-SSL/raw/main/images/5.png" width="400" alt="SNS Config">


---

## 🌐 2. Install and Configure Nginx
Install Nginx:
```bash
sudo apt install -y nginx
```

Start and enable it:
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Check Nginx status:
```bash
systemctl status nginx
```

<img src="https://github.com/Jayanidu-Abeysinghe/React-Application-Deployment-on-Ubuntu-VM-with-Nginx-and-SSL/raw/main/images/6.png" width="900" alt="SNS Config">

---

## 🧬 3. Clone the React Application
Clone your React project from GitHub:
```bash
git clone <repo-url>
cd <project-name>
```

---

## ⚙️ 4. Install Dependencies & Build React App
Install required dependencies:
```bash
npm install
```

Build the React app for production:
```bash
npm run build
```
This generates a build/ folder containing production-ready files.


---

## 🧱 5. Deploy Build Files to Nginx Web Directory
Remove existing files (if any):
```bash
sudo rm -rf /var/www/html/*
```

Copy the React build files to Nginx’s root directory:
```bash
sudo cp -r build/* /var/www/html/
```

<img src="https://github.com/Jayanidu-Abeysinghe/React-Application-Deployment-on-Ubuntu-VM-with-Nginx-and-SSL/raw/main/images/7.png" width="1200" alt="SNS Config">


Set proper permissions:
```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## ⚙️ 6. Configure Nginx for React Routing
Create or edit the default Nginx configuration:

```bash
echo 'server {
    listen 80;
    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    error_page 404 /index.html;
}' | sudo tee /etc/nginx/sites-available/default > /dev/null
```

<img src="https://github.com/Jayanidu-Abeysinghe/React-Application-Deployment-on-Ubuntu-VM-with-Nginx-and-SSL/raw/main/images/8.png" width="600" alt="SNS Config">


Restart Nginx:
```bash
sudo systemctl restart nginx
```

---

## 🌍 7. Access the Application
Find your server’s public IP:

```bash
curl ifconfig.me
```

Then open in your browser:
```bash
http://<your-public-ip>
```

Example:
``
http://203.0.113.25
``

---

## 🔐 8. Setup Free SSL with DuckDNS + Let’s Encrypt
This section explains how to configure a free domain with DuckDNS and secure it with SSL.

### 🦆 Step 1: Register & Configure DuckDNS

1. Visit https://www.duckdns.org
2. Sign in with GitHub/Google/Twitter.
3. Add a new subdomain (e.g., example).
4. Note down:
   - Subdomain: example.duckdns.org
   - Token: 4139ae81-7ed2-420e-a0ea-1fdff9c4f2db
5. Set the IP to your EC2’s public IP (44.249.138.187) and click Update IP.

### 🔑 Step 2: Install Certbot & Generate SSL Certificate
Install Certbot:

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```

Run Certbot:
```bash
sudo certbot --nginx -d example.duckdns.org
```
Follow the prompts:
- Enter your email address
- Agree to Terms of Service
- Decline EFF newsletter (optional)

You should see:

``
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/example.duckdns.org/fullchain.pem
Key is saved at: /etc/letsencrypt/live/example.duckdns.org/privkey.pem
``

<img src="https://github.com/Jayanidu-Abeysinghe/React-Application-Deployment-on-Ubuntu-VM-with-Nginx-and-SSL/raw/main/images/3.png" width="800" alt="SNS Config">


---

### ✅ Step 3: Verify HTTPS Access

Visit your domain:
``
https://example.duckdns.org
``

You should see the secure padlock icon 🔒 in your browser.

---

### 🔁 Step 4: Test Auto-Renewal
Certbot sets up a cron job for automatic renewals. You can test it:

```bash
sudo certbot renew --dry-run
```

Expected output:
``
Congratulations, all simulated renewals succeeded.
``

---

### 🎯 Final Results
✅ Your React site is live at
👉 https://example.duckdns.org

✅ Nginx serves both frontend and backend securely

✅ SSL certificate is installed and auto-renewal configured


<img src="https://github.com/Jayanidu-Abeysinghe/React-Application-Deployment-on-Ubuntu-VM-with-Nginx-and-SSL/raw/main/images/1.jpg" width="600" alt="SNS Config">


### 🎉 Congratulations!

Your react application is now live, secure, and production-ready using Nginx, DuckDNS, and Let’s Encrypt SSL.
