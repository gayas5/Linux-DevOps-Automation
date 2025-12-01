

We’ll create a **step-by-step project guide with execution, screenshots/log evidence, and folder structure**.

---

# 📂 **Project: Linux DevOps Automation **

---

## **1️⃣ Repository Structure**

Here’s a recommended structure:

```
linux-devops-project/
├── README.md
├── level1/
│   ├── users_groups.sh
│   ├── permissions.sh
│   ├── install_packages.sh
│   ├── system_info.sh
│   └── evidence/
├── level2/
│   ├── cron_backup.sh
│   ├── log_cleanup.sh
│   ├── restart_service.sh
│   ├── health_check.sh
│   ├── log_usage.sh
│   └── evidence/
├── level3/
│   ├── projectA.service
│   ├── ssh_hardening.sh
│   ├── lvm_setup.sh
│   ├── firewall_setup.sh
│   ├── logrotate_projectA
│   └── evidence/
└── scripts/
```

* Each level has **scripts** to automate tasks.
* **evidence/** folder will store **screenshots or log outputs**.
* `README.md` explains what was done and how to execute.

---

## **2️⃣ Level 1 – Basic Assignments with Execution & Evidence**

### **a) Users & Groups**

**Script:** `level1/users_groups.sh`

```bash
#!/bin/bash

# Create group
sudo groupadd devteam

# Create users
sudo useradd -m -g devteam dev1
sudo useradd -m -g devteam dev2
sudo useradd -m -g devteam qa1

# Set passwords (you will be prompted)
echo "Set password for dev1"
sudo passwd dev1
echo "Set password for dev2"
sudo passwd dev2
echo "Set password for qa1"
sudo passwd qa1

# Verify
id dev1
groups dev2
```

**Execution:**

```bash
chmod +x level1/users_groups.sh
./level1/users_groups.sh
```

**Evidence:**

```bash
id dev1 > level1/evidence/users_groups.txt
groups dev2 >> level1/evidence/users_groups.txt
cat level1/evidence/users_groups.txt
```

---

### **b) Directory Permissions**

**Script:** `level1/permissions.sh`

```bash
#!/bin/bash

# Create project directory
sudo mkdir -p /opt/projectA

# Assign ownership
sudo chown -R dev1:devteam /opt/projectA

# Set permissions
sudo chmod -R 770 /opt/projectA

# Verify
ls -ld /opt/projectA > level1/evidence/permissions.txt
cat level1/evidence/permissions.txt
```

**Execution:**

```bash
chmod +x level1/permissions.sh
./level1/permissions.sh
```

**Evidence:** `permissions.txt` contains the directory details.

---

### **c) Install Packages**

**Script:** `level1/install_packages.sh`

```bash
#!/bin/bash

sudo dnf update -y
sudo dnf install -y git nginx java-17-openjdk

# Verify
git --version > level1/evidence/packages.txt
nginx -v >> level1/evidence/packages.txt 2>&1
java --version >> level1/evidence/packages.txt
cat level1/evidence/packages.txt
```

**Execution:**

```bash
chmod +x level1/install_packages.sh
./level1/install_packages.sh
```

---

### **d) System Info**

**Script:** `level1/system_info.sh`

```bash
#!/bin/bash

mkdir -p level1/evidence
echo "CPU Info:" > level1/evidence/system_info.txt
lscpu >> level1/evidence/system_info.txt
echo "Memory Info:" >> level1/evidence/system_info.txt
free -h >> level1/evidence/system_info.txt
echo "Disk Info:" >> level1/evidence/system_info.txt
df -h >> level1/evidence/system_info.txt
echo "Network Info:" >> level1/evidence/system_info.txt
ip a >> level1/evidence/system_info.txt

cat level1/evidence/system_info.txt
```

---

## **3️⃣ Level 2 – Intermediate Tasks**

### **a) Cron Backup Script**: `level2/cron_backup.sh`

```bash
#!/bin/bash

# Create backup directory
sudo mkdir -p /backup

# Add cron job (manual step: run `crontab -e` and paste)
echo "0 2 * * * tar -czf /backup/projectA-\$(date +\%F).tar.gz /opt/projectA" >> cron_backup.txt
echo "Check cron jobs with: crontab -l"

# Evidence
ls -lh /backup > level2/evidence/backups.txt
cat level2/evidence/backups.txt
```

### **b) Log Cleanup Script**: `level2/log_cleanup.sh`

```bash
#!/bin/bash
mkdir -p /var/log/projectA
find /var/log/projectA/ -type f -mtime +7 -delete
echo "Log cleanup completed" > level2/evidence/log_cleanup.txt
cat level2/evidence/log_cleanup.txt
```

### **c) Health Check Script**: `level2/health_check.sh`

```bash
#!/bin/bash
if ! curl -s http://localhost > /dev/null; then
  systemctl restart nginx
  echo "$(date) nginx restarted due to failure" >> /var/log/restart.log
else
  echo "$(date) nginx is running fine" >> /var/log/restart.log
fi

cat /var/log/restart.log > level2/evidence/health_check.txt
```

---

## **4️⃣ Level 3 – Advanced Tasks**

### **a) Systemd Service File**: `level3/projectA.service`

```ini
[Unit]
Description=ProjectA App
After=network.target

[Service]
User=dev1
ExecStart=/usr/bin/java -jar /opt/projectA/app.jar
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Execution:**

```bash
sudo mv projectA.service /etc/systemd/system/projectA.service
sudo systemctl daemon-reload
sudo systemctl enable --now projectA
sudo systemctl status projectA > level3/evidence/systemd_status.txt
```

### **b) SSH Hardening Script**: `level3/ssh_hardening.sh`

```bash
#!/bin/bash
sudo sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo bash -c 'echo "AllowUsers dev1 dev2" >> /etc/ssh/sshd_config'
sudo sed -i 's/#Port 22/Port 2222/' /etc/ssh/sshd_config
sudo systemctl restart sshd
echo "SSH Hardened" > level3/evidence/ssh_hardening.txt
cat level3/evidence/ssh_hardening.txt
```

### **c) LVM Setup Script**: `level3/lvm_setup.sh`

```bash
#!/bin/bash
sudo pvcreate /dev/xvdb
sudo vgcreate app_vg /dev/xvdb
sudo lvcreate -L 5G -n data_lv app_vg
sudo mkfs.ext4 /dev/app_vg/data_lv
sudo mkdir -p /mnt/data
sudo mount /dev/app_vg/data_lv /mnt/data
echo "/dev/app_vg/data_lv /mnt/data ext4 defaults 0 0" | sudo tee -a /etc/fstab
df -h > level3/evidence/lvm_status.txt
cat level3/evidence/lvm_status.txt
```

### **d) Firewall Script**: `level3/firewall_setup.sh`

```bash
#!/bin/bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all > level3/evidence/firewall_status.txt
cat level3/evidence/firewall_status.txt
```

### **e) Logrotate Config**: `level3/logrotate_projectA`

```
/var/log/projectA/*.log {
    daily
    rotate 10
    compress
    missingok
    notifempty
    postrotate
        systemctl restart projectA
    endscript
}
```

**Evidence:** `sudo logrotate -d logrotate_projectA > level3/evidence/logrotate_status.txt`

---

## ✅ **5️⃣ Adding Everything to Git Repo**

```bash
# Initialize repo
git init
git add .
git commit -m "Initial Linux DevOps project setup"
git branch -M main
git remote add origin <your_repo_url>
git push -u origin main
```

---

With this structure, **you have:**

* Scripts for **automation**
* **Evidence/logs** captured in the repo
* **Step-by-step execution commands**
* **Level-wise separation**

---

