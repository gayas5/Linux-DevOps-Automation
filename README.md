Perfect! That shows your **current directory structure**:

```
./
../
Linux-DevOps-Automation/
README.md
```

* `./` → Current directory
* `../` → Parent directory
* `Linux-DevOps-Automation/` → Your main project folder
* `README.md` → Project description file

---

### Next Steps to Organize Repo for Commands & Evidence:

1. **Go into your project folder:**

```bash
cd Linux-DevOps-Automation
```

2. **Create subfolders for levels & evidence:**

```bash
mkdir -p level1/evidence level2/evidence level3/evidence scripts
```

3. **Move your existing scripts into proper folders:**

```bash
mv users_groups.sh permissions.sh install_packages.sh system_info.sh level1/
mv cron_backup.sh log_cleanup.sh restart_service.sh health_check.sh log_usage.sh level2/
mv projectA.service ssh_hardening.sh lvm_setup.sh firewall_setup.sh logrotate_projectA level3/
```

4. **Check structure:**

```bash
tree
```

You should see something like:

```
Linux-DevOps-Automation/
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

---

5. **Run scripts level by level** and **capture evidence**:

Example for Level 1:

```bash
chmod +x level1/*.sh
./level1/users_groups.sh
./level1/permissions.sh
./level1/install_packages.sh
./level1/system_info.sh
```

**Capture outputs in evidence folder**:

```bash
id dev1 > level1/evidence/users_groups.txt
ls -ld /opt/projectA > level1/evidence/permissions.txt
git --version > level1/evidence/packages.txt
df -h >> level1/evidence/system_info.txt
```

---

6. **Commit to Git**:

```bash
git add .
git commit -m "Organized scripts and added evidence structure"
git push origin main
```

---
