
### MongoDB Installation Guide

#### Organize version
Here are the steps with each one bolded:

markdown
**1. Check the MongoDB supported Linux version:**
```bash
cat /etc/lsb-release
```

**3. Install sudo:**
```bash
apt install sudo
```

**4. Then install gnupg and curl:**
```bash
sudo apt-get install gnupg curl
```

**5. Run this command to download MongoDB's GPG key and create a file:**
```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
   Then check if the file was successfully created.

**6. Create a list file for MongoDB:**
```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

**7. Update package:**
```bash
sudo apt-get update
```

**8. Install the latest version of MongoDB:**
```bash
sudo apt-get install -y mongodb-org
```

**9. After successfully installing, start the MongoDB service:**
```bash
sudo systemctl start mongod.service
```

**10. Check MongoDB status:**
```bash
sudo systemctl status mongod
```

**11. After confirming that the service is running as expected, enable the MongoDB service to start up at boot:**
```bash
sudo systemctl enable mongod
```

**12. Check connection:**
```bash
mongosh
```

*** when you are userd in your project trnsictions then follow this commands

**1. Edit the MongoDB configuration file for authentication and IP configuration:**
```bash
sudo nano /etc/mongod.conf
```

**2. past it after this "storage: dbPath: /var/lib/mongodb "

```bash 
replication:
  replSetName: "rs0"
```

**3. After save then, start the MongoDB service:**
```bash
sudo systemctl start mongod.service
```
**4. Check MongoDB status:**
```bash
sudo systemctl status mongod
```

**5. then open mongosh
```bash
mongosh
```

**6. then pest this code 
```
rs.initiate()
```
**7. restart mongosh
```bash
sudo systemctl restart mongod
```

#### to check every path exist or not using ls command. ex: ls /etc/mongodb-conf
