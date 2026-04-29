# 🪟 Windows: After Cloning - Complete Setup Guide

> **For Windows users who just cloned the repository from GitHub**

---

## 📥 Step 1: Clone the Repository (if not done yet)

Open **Command Prompt** or **PowerShell** and run:

```cmd
git clone https://github.com/your-username/fooddeliveryApp.git
cd fooddeliveryApp
```

Or if you downloaded as ZIP, extract it and navigate to the folder.

---

## ✅ Step 2: Install Prerequisites (Do This FIRST!)

### 2.1 Install Java (JDK 8+)

**Check if already installed:**
```cmd
java -version
```

**If not installed:**
1. Go to: https://www.oracle.com/java/technologies/downloads/
2. Download: **Windows x64 Installer** for JDK 8 or higher
3. Run the installer with default settings
4. **RESTART Command Prompt**
5. Verify: `java -version`

### 2.2 Install Maven 3.6+

**Check if already installed:**
```cmd
mvn -version
```

**If not installed:**
1. Go to: https://maven.apache.org/download.cgi
2. Download: **apache-maven-3.9.x-bin.zip** (Binary zip archive)
3. Extract to: `C:\maven` (create this folder if needed)
4. Add to PATH:
   - Press `Win + X` → Choose **System (Settings)**
   - Or open: **Settings** → **System** → **About** → **Advanced system settings**
   - Click **Environment Variables**
   - Under "System variables", click **New**
     - Variable name: `M2_HOME`
     - Variable value: `C:\maven\apache-maven-3.9.x` (your actual version)
   - Click **New** again
     - Variable name: `PATH`
     - Variable value: Add `;C:\maven\apache-maven-3.9.x\bin` to the end
   - Click **OK** on all dialogs
5. **Restart Command Prompt**
6. Verify: `mvn -version`

### 2.3 Install MySQL 5.7+

**Check if already installed:**
```cmd
mysql --version
```

**If not installed:**
1. Go to: https://dev.mysql.com/downloads/mysql/
2. Choose: **MySQL Community Server** → **Windows (x86, 64-bit)**
3. Download and run the installer
4. During installation:
   - **Config Type**: Development Machine
   - **TCP Port**: 3306
   - **MySQL Root Password**: Set a password (or leave blank for no password)
   - Make sure "Install as Windows Service" is checked
5. Finish installation
6. Verify: 
   ```cmd
   mysql -u root
   mysql> exit
   ```

### 2.4 Install Apache Tomcat 9/10/11

**Download:**
1. Go to: https://tomcat.apache.org/download-11.cgi
2. Click **Core** → **Binary Distributions** → **zip**
3. Extract to: `C:\tomcat` (create this folder first)

**Verify:**
```cmd
dir C:\tomcat\bin\startup.bat
```

---

## 🗄️ Step 3: Setup Database

**Open Command Prompt** in the project folder:

```cmd
cd C:\path\to\fooddeliveryApp
```

**Create database:**

```cmd
REM If MySQL root has NO password:
mysql -u root < WebApp\database\fooddb.sql

REM If MySQL root has a password:
mysql -u root -p < WebApp\database\fooddb.sql
REM (Will prompt for password)
```

**Getting "Access denied" error?**

See [WINDOWS_MYSQL_FIX.md](WINDOWS_MYSQL_FIX.md) for solutions. Common fixes:
- Start MySQL service: `net start MySQL80`
- Use alternative syntax: `type WebApp\database\fooddb.sql | mysql -u root`
- Provide password: `mysql -u root -p < WebApp\database\fooddb.sql`

**Verify it worked:**
```cmd
mysql -u root -e "USE fooddb; SELECT COUNT(*) as orders FROM orders;"
```

Expected output:
```
+--------------+
| orders       |
+--------------+
| 5            |
+--------------+
```

---

## 🏗️ Step 4: Build the Application

**In Command Prompt**, from project root:

```cmd
build.bat build
```

**Expected output:**
```
[INFO] Building war: ...\target\fooddelivery.war
[INFO] BUILD SUCCESS
```

**Or use Maven directly:**
```cmd
cd WebApp
mvn clean package
```

---

## 🔐 Step 5: Configure Database Password (If Needed)

If your MySQL root user **has a password**, do this BEFORE deploying:

**Option A: Temporary (only for this session)**
```cmd
set FOODDB_PASSWORD=your_mysql_root_password
```

**Option B: Permanent (recommended)**
1. Press `Win + X` → **System (Settings)**
2. **Advanced system settings** → **Environment Variables**
3. Click **New** (under System variables)
   - Variable name: `FOODDB_PASSWORD`
   - Variable value: `your_mysql_root_password`
4. Click **OK** and **restart Command Prompt**

---

## 🚀 Step 6: Deploy to Tomcat

**Automatic (Easiest):**
```cmd
build.bat deploy
```

This will:
- ✓ Find Tomcat automatically
- ✓ Stop Tomcat (if running)
- ✓ Copy WAR file
- ✓ Start Tomcat
- ✓ Wait for deployment

**Manual deployment:**
```cmd
REM Stop Tomcat if running
C:\tomcat\bin\shutdown.bat

REM Wait 2 seconds
timeout /t 2

REM Copy WAR file
copy WebApp\target\fooddelivery.war C:\tomcat\webapps\

REM Start Tomcat
C:\tomcat\bin\startup.bat

REM Wait 5 seconds for deployment
timeout /t 5

REM Open in browser
start http://localhost:8080/fooddelivery
```

---

## 🎮 Step 7: Access the Application

1. **Open browser**: http://localhost:8080/fooddelivery
2. **Login** with:
   - Username: `admin`
   - Password: `admin123`
3. **Use features**:
   - ➕ Add Order
   - 📋 View Orders
   - 💎 Premium Orders
   - 📊 Dashboard

---

## ⚠️ Troubleshooting: Windows Batch Script Error

If you see: **`\tomcat was unexpected at this time`**

✅ **Solution:** Use the fixed build.bat or deploy manually

See [WINDOWS_ERROR_FIX.md](WINDOWS_ERROR_FIX.md) for details.

---

## 📋 Complete Checklist

After cloning, go through this checklist:

- [ ] **Java installed**: `java -version` works
- [ ] **Maven installed**: `mvn -version` works
- [ ] **MySQL running**: `mysql -u root -e "SELECT 1;"` connects
- [ ] **Database created**: `mysql -u root -e "SHOW DATABASES;" | findstr fooddb`
- [ ] **Tomcat extracted**: `dir C:\tomcat\bin\startup.bat` works
- [ ] **Build successful**: `build.bat build` shows BUILD SUCCESS
- [ ] **Application accessible**: http://localhost:8080/fooddelivery loads
- [ ] **Login works**: admin / admin123

---

## 🐛 Troubleshooting - Windows Specific

### Problem: "mvn: command not found" or "Maven not recognized"
```
✅ Solution:
1. Open Command Prompt as Administrator
2. Right-click → Run as Administrator
3. Maven PATH not set correctly - follow Step 2.2 again
4. RESTART Command Prompt after changing PATH
```

### Problem: "mysql: command not found"
```
✅ Solution:
1. Add MySQL to PATH:
   - Environment Variables → New
   - Variable name: PATH
   - Add: C:\Program Files\MySQL\MySQL Server 8.0\bin;
2. RESTART Command Prompt
```

### Problem: "Can't find Tomcat"
```
✅ Solution:
1. Verify Tomcat location:
   dir C:\tomcat\bin\startup.bat

2. If in different location, use:
   build.bat deploy-path "C:\Program Files\Apache Tomcat"

3. Or deploy manually (see Step 6)
```

### Problem: "MySQL connection refused"
```
✅ Solution:
1. Check if MySQL is running:
   Services.msc (Press Win+R, type it)
   Look for "MySQL80" → should show "Running"

2. If not running:
   Command Prompt as Administrator:
   net start MySQL80

3. If still fails, verify MySQL is installed correctly
```

### Problem: "Can't create database - ERROR 1045"
```
✅ Solution:
Your MySQL root password is wrong

Use correct password:
mysql -u root -p < WebApp\database\fooddb.sql

Or set environment variable:
set FOODDB_PASSWORD=your_correct_password
```

### Problem: "Application shows HTTP 404"
```
✅ Solution:
1. Check if Tomcat is running:
   start http://localhost:8080
   Should load Tomcat homepage

2. Check if WAR deployed:
   dir C:\tomcat\webapps\fooddelivery.war

3. Check logs:
   type C:\tomcat\logs\catalina.2026-04-29.log

4. Restart Tomcat:
   C:\tomcat\bin\shutdown.bat
   timeout /t 2
   C:\tomcat\bin\startup.bat
```

### Problem: "Port 8080 already in use"
```
✅ Solution:
1. Find process using port:
   netstat -ano | findstr :8080

2. Kill the process (note the PID):
   taskkill /PID <PID> /F

3. Start Tomcat again:
   C:\tomcat\bin\startup.bat
```

### Problem: "Login keeps failing"
```
✅ Solution:
1. Verify database is created:
   mysql -u root -e "USE fooddb; SELECT * FROM users;"

2. Check admin user exists:
   Should show one row with admin/admin123

3. Clear browser cookies:
   Press Ctrl+Shift+Delete
   Clear all cookies from http://localhost:8080

4. Try login again
```

---

## 📁 Important Folders & Files

```
C:\path\to\fooddeliveryApp\
├── build.bat                           ← Run this to build
├── WebApp\
│   ├── target\fooddelivery.war         ← Deployed file
│   ├── database\fooddb.sql             ← Database schema
│   └── pom.xml                         ← Maven config
├── README.md                           ← Full documentation
├── WINDOWS_SETUP.md                    ← Windows guide
└── SETUP_FOR_TEACHER.md               ← Quick setup

Tomcat location:
C:\tomcat\
├── bin\startup.bat                     ← Start Tomcat
├── bin\shutdown.bat                    ← Stop Tomcat
├── webapps\fooddelivery.war            ← Deployed app
└── logs\catalina.2026-04-29.log        ← Tomcat logs
```

---

## 🎯 Quick Reference: Commands to Run (In Order)

```cmd
REM 1. Navigate to project
cd C:\Users\YourName\Desktop\fooddeliveryApp

REM 2. Create database
mysql -u root < WebApp\database\fooddb.sql

REM 3. Build
build.bat build

REM 4. Set password (if MySQL has one)
set FOODDB_PASSWORD=your_password

REM 5. Deploy
build.bat deploy

REM 6. Open in browser (automatic)
REM Or manually: start http://localhost:8080/fooddelivery

REM 7. Login
REM Username: admin
REM Password: admin123
```

---

## 🛑 Stop the Application

```cmd
REM Graceful shutdown
C:\tomcat\bin\shutdown.bat

REM Wait for shutdown
timeout /t 3

REM Force kill (if needed)
taskkill /IM java.exe /F
```

---

## 📞 Common Issues Quick Links

| Issue | Search for |
|-------|-----------|
| Java installation | "Java JDK 8 download Windows" |
| Maven setup | "Maven PATH Windows" |
| MySQL installation | "MySQL Server Windows download" |
| Tomcat setup | "Apache Tomcat Windows zip" |
| Port conflict | "Windows port 8080 already in use" |

---

## ✨ You're All Set!

**Once you complete these steps, you'll have:**

✅ Fully functional Food Delivery Order Management System  
✅ Database with sample orders  
✅ Running on Tomcat server  
✅ Accessible at http://localhost:8080/fooddelivery  
✅ Ready for development or deployment  

---

## 📚 Need More Help?

- **General setup**: See [SETUP_FOR_TEACHER.md](SETUP_FOR_TEACHER.md)
- **Full documentation**: See [README.md](README.md)
- **macOS/Linux users**: See original SETUP guides
- **Build script help**: Run `build.bat help`

---

**Happy coding! 🚀**

*Last updated: 29 April 2026*
