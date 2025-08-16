# 🚀 Deploy with CI/CD using GitHub Actions and SSH

## 🧰 VPS Requirements

Ensure your VPS is ready for deployment:

-  Node.js & npm installed  
-  PM2 or any Node.js process manager  
-  SSH access enabled  

---

## 🔐 Step 1: Generate SSH Key

### 1️. Create SSH Key on Your Local Machine

Open your terminal and run:

```bash
ssh-keygen -t rsa -b 4096 -C "example@mail.com"
```

- **Prompt 1**: When asked where to save the key, press `Enter` to accept the default path.
- **Prompt 2**: When asked for a passphrase, simply press `Enter` to skip it.

> 🔑 This will create two files:  
> `~/.ssh/id_rsa` (private key) and  
> `~/.ssh/id_rsa.pub` (public key)

---

### 2️. Copy Your Public Key

Run this to get your public key:

```bash
cat ~/.ssh/id_rsa.pub
```

---

### 3️. Add Public Key to VPS

Connect to your VPS, then:

```bash
nano ~/.ssh/authorized_keys
```

- Paste the full public key as a **new line**
- Save and exit

---

### 4️. Test the Connection

From your local terminal, run:

```bash
ssh root@YOUR_SERVER_IP
```

If you connect **without entering a password**, it's working! 🎉

---

### 5️. Copy Your Private Key for GitHub Secret

Run the following:

```bash
cat ~/.ssh/id_rsa
```

You'll get output like:

```bash
-----BEGIN OPENSSH PRIVATE KEY-----
(long key content)
-----END OPENSSH PRIVATE KEY-----
```

---

## 🔐 Step 2: Add GitHub Secrets

Go to:  
**GitHub Repo > Settings > Secrets and Variables > Actions**

Add the following secrets:

| Name            | Value                      |
|-----------------|----------------------------|
| `DO_HOST`       | VPS IP (e.g. `160.168.x.x`)|
| `DO_USER`       | `root`                     |
| `DO_PRIVATE_KEY`| Your full private SSH key  |

---

## 🧠 Step 3: Create GitHub Action Workflow

📁 File path: `.github/workflows/deploy.yml`

```yaml
name: Deploy Backend to VPS

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: 📥 Checkout Code
        uses: actions/checkout@v3

      - name: 🚀 Deploy to VPS via SSH
        uses: appleboy/ssh-action@v1.0.0
        with:
          host: ${{ secrets.DO_HOST }}
          username: ${{ secrets.DO_USER }}
          key: ${{ secrets.DO_PRIVATE_KEY }}
          port: 22
          script: |
            export NVM_DIR="$HOME/.nvm"
            [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
            nvm use 24

            node -v
            npm -v
            pm2 -v

            cd ~/ryanschirmer-backend
            git pull origin main
            rm -rf node_modules
            npm install -g pm2
            npm install
            pm2 restart dayf-server || pm2 start npm --name dayf-server -- run dev

 
```

---

## ✅ You're All Set!

Now every time you push to the `main` branch, your project will auto-deploy to your VPS like magic. 🧙‍♂️✨

Need a coffee? You've earned it. ☕😉

---

