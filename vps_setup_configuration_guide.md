# VPS Setup & Configuration Guide

## 1. System Updates & Node.js Setup

```bash
sudo apt update
sudo apt upgrade -y
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install --lts
node -v
```

## 2. MongoDB Setup

Refer to the [MongoDB Community Edition Installation Guide](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/) for installation steps, then run:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
sudo systemctl status mongod
```

## 3. Git & GitHub CLI Setup

```bash
sudo apt install git -y
sudo apt install gh -y
gh auth login
```

## 4. PostgreSQL Setup

```bash
sudo apt update
sudo apt install -y postgresql postgresql-contrib
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo -u postgres psql -c "ALTER USER postgres WITH PASSWORD 'your_password';"
sudo systemctl status postgresql
```

## 5. Redis Setup

```bash
sudo apt update
sudo apt install -y redis-server
sudo systemctl start redis-server
sudo systemctl enable redis-server
sudo systemctl status redis-server
```

---

# Nginx & Next.js Upload Limit Configuration

## Prerequisites

- VPS with Ubuntu
- Nginx installed and running
- Next.js app running on port `3000` (or your app port)
- Root or `sudo` access

## Step-by-Step Configuration

### 1. Open Nginx Site Configuration

```bash
sudo nano /etc/nginx/sites-enabled/cdn.uccrangpurbranch.com.conf
```

### 2. Increase Upload Size Limit

Add the `client_max_body_size 20480M;` directive inside the `server { ... }` block to set a 20GB limit:

```nginx
server {
    listen 80;
    server_name cdn.uccrangpurbranch.com;

    client_max_body_size 20480M;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 3. Test & Reload Nginx

```bash
# Verify Nginx configuration syntax
sudo nginx -t

# Reload Nginx to apply changes
sudo systemctl reload nginx
```