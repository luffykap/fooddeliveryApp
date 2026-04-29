# 🪟 Windows: Fix MySQL "Access Denied" Error

> **Error:** `ERROR 1045 (28000): Access denied for user 'root'@'localhost'`  
> **When:** Running `mysql -u root < WebApp\database\fooddb.sql`  
> **Status:** ✅ SOLUTION PROVIDED

---

## ⚠️ The Problem

Windows batch doesn't handle `<` redirection the same way as Linux/Mac. Also, MySQL root might have a password set.

---

## ✅ Solution 1: Check if MySQL is Running

**Open Command Prompt and run:**

```cmd
mysql --version
```

If you get command not found, MySQL isn't in PATH. Add it:

```cmd
REM Find MySQL installation
where mysql

REM If not found, add to PATH manually:
set PATH=%PATH%;C:\Program Files\MySQL\MySQL Server 8.0\bin
```

---

## ✅ Solution 2: Test MySQL Connection

```cmd
mysql -u root -p
```

When prompted for password:
- Press Enter if no password is set (just hit Enter)
- Or type your MySQL root password if you set one

If you can't connect, MySQL service isn't running.

---

## ✅ Solution 3: Start MySQL Service on Windows

### Option A: Start from Command Prompt (Admin)

```cmd
net start MySQL80
```

Or if using MySQL 5.7:
```cmd
net start MySQL57
```

Check your exact service name:
```cmd
sc query | find "MySQL"
```

### Option B: Use MySQL Installer

1. Open **MySQL Installer**
2. Click **Server** → **Configure**
3. Check if service is running

---

## ✅ Solution 4: Import Database (Windows Way)

### Method 1: Command Line (Best)

If MySQL root has **NO password**:
```cmd
mysql -u root < WebApp\database\fooddb.sql
```

If that doesn't work, use this instead:
```cmd
type WebApp\database\fooddb.sql | mysql -u root
```

### Method 2: If MySQL Root Has a Password

Ask your friend: Did they set a password for MySQL root?

If YES, use:
```cmd
mysql -u root -p < WebApp\database\fooddb.sql
```

Then type the password when prompted.

Or with password in command (not recommended but works):
```cmd
mysql -u root -pYOUR_PASSWORD < WebApp\database\fooddb.sql
```

Note: No space between `-p` and password!

### Method 3: Use PowerShell (More Reliable)

Open **PowerShell as Administrator** and run:

```powershell
Get-Content WebApp\database\fooddb.sql | mysql -u root
```

### Method 4: Full Path to MySQL

```cmd
"C:\Program Files\MySQL\MySQL Server 8.0\bin\mysql" -u root < WebApp\database\fooddb.sql
```

---

## ✅ Solution 5: Create Database Manually

If command line import fails, do it manually:

### Step 1: Open MySQL Client
```cmd
mysql -u root -p
```

### Step 2: Create Database
```sql
CREATE DATABASE IF NOT EXISTS fooddb;
USE fooddb;
```

### Step 3: Import Schema

Get content of `WebApp\database\fooddb.sql` and paste it into MySQL client, or:

```cmd
mysql -u root -p fooddb < WebApp\database\fooddb.sql
```

---

## ✅ Solution 6: Check Your Build/Deploy Script

The **build.bat** and deployment scripts now handle this automatically:

```cmd
build.bat build
```

The script will:
1. ✅ Check if Maven is installed
2. ✅ Build the WAR file
3. ✅ Show success/failure

For deployment:
```cmd
build.bat deploy
```

---

## 🔍 Debugging: Step by Step

Your friend should do this in order:

### Step 1: Check MySQL Service
```cmd
sc query MySQL80
```

Look for `STATE        : 4  RUNNING`

If not running, start it:
```cmd
net start MySQL80
```

### Step 2: Test MySQL Connection
```cmd
mysql -u root
```

Should connect successfully.

### Step 3: List Databases
```cmd
mysql -u root -e "SHOW DATABASES;"
```

### Step 4: Check if fooddb Exists
```cmd
mysql -u root -e "USE fooddb; SHOW TABLES;"
```

If `fooddb` doesn't exist, import it:

### Step 5: Import Database
```cmd
mysql -u root < WebApp\database\fooddb.sql
```

Or if that fails:
```cmd
type WebApp\database\fooddb.sql | mysql -u root
```

### Step 6: Verify Import
```cmd
mysql -u root -e "USE fooddb; SELECT * FROM users;"
```

Should show:
```
+----+----------+-----------+
| id | username | password  |
+----+----------+-----------+
|  1 | admin    | admin123  |
+----+----------+-----------+
```

---

## 🎯 Common Issues & Fixes

| Issue | Fix |
|-------|-----|
| `mysql: command not found` | Add MySQL to PATH or use full path |
| `ERROR 1045` | MySQL service not running OR root has password |
| `ERROR 1049: Unknown database 'fooddb'` | Run import command first |
| `<` not working | Use `type ... \| mysql` instead |
| Access denied with `-p` | Check if password is correct |

---

## ✨ Summary

Your friend should try in this order:

```cmd
REM 1. Check MySQL is running
net start MySQL80

REM 2. Test connection
mysql -u root

REM 3. Try import (no password)
mysql -u root < WebApp\database\fooddb.sql

REM 4. If that fails, try this
type WebApp\database\fooddb.sql | mysql -u root

REM 5. If still fails, use manual import
mysql -u root -p fooddb < WebApp\database\fooddb.sql
```

---

## 📞 If Still Stuck

Have your friend try:

```cmd
REM Run this to show system info
systeminfo | find "OS"
mysql --version
java -version
```

And send the output, plus the exact error message they're getting.

---

**The key: Make sure MySQL service is running, and use the right password!** ✅
