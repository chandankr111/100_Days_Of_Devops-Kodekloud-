# 📌 MariaDB Service Failure – Production Debugging & Fix

## 🧩 Problem Statement

A critical issue occurred in the Nautilus application (Stratos DC) where the application was unable to connect to the database.

### 🚨 Symptoms
- Application downtime due to DB connection failure  
- MariaDB service found **inactive (dead)**  
- Service restart attempts failed  

---

## 🔍 Root Cause Analysis

Initial investigation was done using:

```bash
systemctl status mariadb
journalctl -xeu mariadb.service
❗ Key Error
Cannot change ownership of the datadir
📌 Root Cause

Incorrect ownership and permissions on the MariaDB data directory:

/var/lib/mysql
MariaDB runs under the mysql system user but did not have required access permissions
🛠️ Solution Implemented
✅ Step 1: Fix Ownership
sudo chown -R mysql:mysql /var/lib/mysql
Ensures MariaDB has ownership of its data directory
✅ Step 2: Fix Permissions
sudo chmod -R 755 /var/lib/mysql
Grants appropriate read/write/execute permissions
✅ Step 3: Start MariaDB Service
sudo systemctl start mariadb
✅ Step 4: Enable Service on Boot
sudo systemctl enable mariadb
✅ Verification
systemctl status mariadb

✔️ Expected Output:

Active: active (running)