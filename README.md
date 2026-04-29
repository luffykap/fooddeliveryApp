# 🍔 Food Delivery Order Management System

A professional web-based application for managing food delivery orders with real-time tracking, dashboard analytics, and revenue metrics.

**Status**: ✅ Production-Ready | **Technology**: Java Web Stack | **Database**: MySQL | **Server**: Apache Tomcat

**Platform Support**: 🖥️ **macOS** | 🐧 **Linux** | 🪟 **Windows**

---

## 🪟 Windows Users?

📖 **[Read WINDOWS_AFTER_CLONE.md](WINDOWS_AFTER_CLONE.md)** for complete post-clone setup!

This guide covers:
- ✅ Installing all prerequisites (Java, Maven, MySQL, Tomcat)
- ✅ Step-by-step Windows-specific instructions
- ✅ Database creation for Windows
- ✅ Building and deploying
- ✅ Windows troubleshooting

---

## 🖥️ macOS/Linux Users?

Start with [SETUP_FOR_TEACHER.md](SETUP_FOR_TEACHER.md) for quick setup!

---

## 🎯 Features

✅ **User Authentication** - Secure login with session management  
✅ **Order Management** - Add, view, edit, and delete orders  
✅ **Dashboard Analytics** - Real-time order statistics and revenue tracking  
✅ **Premium Orders Filter** - View high-value orders (>₹1000)  
✅ **Responsive UI** - Modern, mobile-friendly interface  
✅ **Session Security** - 30-minute timeout with HttpOnly cookies  

---

## 🏗️ Technology Stack

| Component | Technology |
|-----------|-----------|
| **Backend** | Java Servlets (Jakarta API 6.0) |
| **Frontend** | JSP, HTML5, CSS3, JavaScript |
| **Database** | MySQL 5.7+ |
| **Server** | Apache Tomcat 9.x, 10.x, or 11.x |
| **Build Tool** | Apache Maven 3.6+ |
| **Packaging** | WAR (Web Application Archive) |

---

## 📋 Prerequisites

Before running, ensure you have installed:

### 1. Java Development Kit (JDK) 8+
```bash
java -version
# Should show: java version "1.8.0" or higher
```
**Download**: https://www.oracle.com/java/technologies/downloads/

### 2. Apache Maven 3.6+
```bash
mvn -version
# Should show: Apache Maven 3.6.0 or higher
```
**Download**: https://maven.apache.org/download.cgi

### 3. MySQL Server 5.7+
```bash
mysql --version
# Should show: mysql Ver 5.7.x or higher
```
**Download**: https://www.mysql.com/downloads/

### 4. Apache Tomcat 9.x / 10.x / 11.x
**Download**: https://tomcat.apache.org/download-11.cgi

---

## 🚀 Quick Start (3 Steps)

### Step 1: Setup Database

From the **project root directory**, create the MySQL database:

```bash
# macOS/Linux - If MySQL root has NO password (default on Homebrew):
mysql -u root < WebApp/database/fooddb.sql

# macOS/Linux - If MySQL root has a password:
mysql -u root -p < WebApp/database/fooddb.sql
# (Will prompt for password)
```

**Windows:**
```cmd
REM If MySQL root has NO password:
mysql -u root < WebApp\database\fooddb.sql

REM If MySQL root has a password:
mysql -u root -p < WebApp\database\fooddb.sql
```

**Verify:**
```bash
# macOS/Linux/Windows
mysql -u root -e "USE fooddb; SELECT COUNT(*) as orders FROM orders;"
```

### Step 2: Build the Application

**Option A: Using Build Script (Easiest)**

**macOS/Linux:**
```bash
cd WebApp
./build.sh build
```

**Windows:**
```cmd
build.bat build
```

**Option B: Using Maven Directly (All platforms)**

```bash
cd WebApp
mvn clean package
```

Expected output:
```
[INFO] Building war: .../target/fooddelivery.war
[INFO] BUILD SUCCESS
```

### Step 3: Deploy to Tomcat

**Option A: Automatic Deployment**

**macOS/Linux:**
```bash
./build.sh deploy-path /path/to/tomcat
```

**Windows:**
```cmd
build.bat deploy-path "C:\path\to\tomcat"
```

**Option B: Manual Deployment**

**macOS/Linux:**
```bash
# 1. Copy WAR file
cp WebApp/target/fooddelivery.war /path/to/tomcat/webapps/

# 2. Start Tomcat
/path/to/tomcat/bin/startup.sh

# 3. Wait for deployment (3-5 seconds)
sleep 5

# 4. Open application
open http://localhost:8080/fooddelivery
```

**Windows:**
```cmd
REM 1. Copy WAR file
copy WebApp\target\fooddelivery.war C:\tomcat\webapps\

REM 2. Start Tomcat
C:\tomcat\bin\startup.bat

REM 3. Wait for deployment
timeout /t 5

REM 4. Open application
start http://localhost:8080/fooddelivery
```

---

## 🔐 Login Credentials

**Default demo credentials** (for testing):

| Field | Value |
|-------|-------|
| **Username** | `admin` |
| **Password** | `admin123` |

---

## 🗂️ Project Structure

```
fooddeliveryApp/
├── WebApp/
│   ├── pom.xml                              # Maven configuration
│   ├── src/main/
│   │   ├── java/com/fooddelivery/
│   │   │   ├── servlet/                    # HTTP request handlers
│   │   │   │   ├── LoginServlet.java
│   │   │   │   ├── AddOrderServlet.java
│   │   │   │   ├── ViewOrdersServlet.java
│   │   │   │   ├── DeleteOrderServlet.java
│   │   │   │   └── ...
│   │   │   ├── dao/                        # Database access
│   │   │   │   └── OrderDAO.java
│   │   │   ├── model/                      # Data models
│   │   │   │   └── FoodOrder.java
│   │   │   └── util/                       # Utilities
│   │   │       ├── DBConnection.java
│   │   │       └── TestDB.java
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   ├── web.xml                 # Deployment descriptor
│   │       │   └── lib/                    # Dependencies
│   │       ├── login.jsp
│   │       ├── dashboard.jsp
│   │       ├── index.jsp
│   │       ├── viewOrders.jsp
│   │       └── css/, js/                   # Static assets
│   ├── database/
│   │   └── fooddb.sql                      # Database schema
│   └── target/
│       └── fooddelivery.war                # Deployable artifact
│
├── SETUP_FOR_TEACHER.md                   # Quick start guide
├── README.md                              # This file
└── build.sh                               # Build automation script
```

---

## 🎮 Usage Guide

### Login
1. Navigate to `http://localhost:8080/fooddelivery`
2. Enter: `admin` / `admin123`
3. Click **Login**

### Add New Order
1. Click **➕ Add Order** in navbar
2. Fill in order details:
   - **Order ID**: Unique identifier (e.g., ORD001)
   - **Customer Name**: Full name of customer
   - **Restaurant**: Restaurant name
   - **Amount**: Price in rupees
   - **Status**: Select from Pending/Preparing/On the Way/Delivered/Cancelled
3. Click **Add Order** button

### View All Orders
1. Click **📋 View All Orders** in navbar
2. See table with all orders and their details
3. Click **Delete** button to remove an order

### View Premium Orders
1. Click **💎 Premium Orders** in navbar
2. See only orders with amount > ₹1000

### Dashboard
1. Click **📊 Dashboard** in navbar
2. View statistics:
   - Total orders
   - Total revenue
   - Pending orders count
   - Recent activity

### Logout
1. Click **🚪 Logout** in navbar
2. Session will be terminated
3. Redirected to login page

---

## 🔧 Database Configuration

### Default Settings (Auto-Detection)

The application **automatically detects** your MySQL password. It tries these in order:

1. `FOODDB_PASSWORD` environment variable (if set)
2. No password (empty)
3. `"root"`
4. `"password"`
5. `"mysql"`

### Custom Configuration

If your MySQL uses a different password or host, set environment variables **before** starting Tomcat:

**macOS/Linux:**
```bash
export FOODDB_URL="jdbc:mysql://your-host:3306/fooddb?useSSL=false&serverTimezone=UTC"
export FOODDB_USER="your_username"
export FOODDB_PASSWORD="your_password"

# Then start Tomcat:
/path/to/tomcat/bin/startup.sh
```

**Windows (Command Prompt):**
```cmd
set FOODDB_URL=jdbc:mysql://your-host:3306/fooddb?useSSL=false&serverTimezone=UTC
set FOODDB_USER=your_username
set FOODDB_PASSWORD=your_password

REM Then start Tomcat:
C:\path\to\tomcat\bin\startup.bat
```

**Windows (PowerShell):**
```powershell
$env:FOODDB_URL="jdbc:mysql://your-host:3306/fooddb?useSSL=false&serverTimezone=UTC"
$env:FOODDB_USER="your_username"
$env:FOODDB_PASSWORD="your_password"

# Then start Tomcat:
C:\path\to\tomcat\bin\startup.bat
```

**Windows (Permanent - Set via UI):**
1. Right-click **This PC** → **Properties** → **Advanced system settings**
2. Click **Environment Variables** → **New** (under System variables)
3. Add each variable:
   - `FOODDB_URL` = `jdbc:mysql://your-host:3306/fooddb?useSSL=false&serverTimezone=UTC`
   - `FOODDB_USER` = `your_username`
   - `FOODDB_PASSWORD` = `your_password`
4. Restart Command Prompt and Tomcat

---

## 📦 Building Without Tomcat (For Development)

Quick testing with embedded Tomcat:

```bash
cd WebApp
mvn tomcat9:run
```

Then access: `http://localhost:8080/fooddelivery`

Press `Ctrl+C` to stop.

---

## 🐛 Troubleshooting

### Issue: "Can't connect to database"
```
✅ Solution:
1. Check if MySQL is running:
   mysql -u root -e "SELECT 1;"

2. If you get "ERROR 2002", MySQL is not running - start it first
   
   On Windows:
   - Check Services: Services.msc
   - Look for "MySQL80" or similar
   - If not running: net start MySQL80

3. Verify database exists:
   mysql -u root -e "SHOW DATABASES;" | grep fooddb

4. If missing, run:
   mysql -u root < WebApp/database/fooddb.sql    (macOS/Linux)
   mysql -u root < WebApp\database\fooddb.sql    (Windows)
```

### Issue: "ERROR 1045: Access denied for user 'root'"
```
✅ Solution:
MySQL password doesn't match. Set correct password:

macOS/Linux:
export FOODDB_PASSWORD="your_actual_password"

Windows (Command Prompt):
set FOODDB_PASSWORD=your_actual_password

Windows (PowerShell):
$env:FOODDB_PASSWORD="your_actual_password"

Then restart Tomcat.
```

### Issue: "Application not accessible (HTTP 404 or 500)"
```
✅ Solution:
1. Check if Tomcat is running:
   macOS/Linux: curl http://localhost:8080/
   Windows: start http://localhost:8080 or use browser

2. Check Tomcat logs:
   macOS/Linux: tail -50 /path/to/tomcat/logs/catalina.out
   Windows: type C:\tomcat\logs\catalina.2026-04-29.log

3. Verify WAR deployed:
   macOS/Linux: ls /path/to/tomcat/webapps/fooddelivery*
   Windows: dir C:\tomcat\webapps\fooddelivery*
```

### Issue: "Login fails or session timeout immediately"
```
✅ Solution:
1. Verify database is running and has users table:
   mysql -u root -e "USE fooddb; SELECT * FROM users;"

2. Check default credentials are correct:
   Username: admin, Password: admin123

3. Clear browser cookies and try again

4. Windows only - Check Tomcat logs:
   type C:\tomcat\logs\catalina.2026-04-29.log
```

### Issue: "build.bat command not found" (Windows)
```
✅ Solution:
1. Make sure you're in project root directory:
   dir build.bat    (should exist)

2. Try full command:
   build.bat build

3. If still not working, use Maven directly:
   cd WebApp
   mvn clean package
```

### Issue: "Port 8080 already in use" (Windows)
```
✅ Solution:
Find and stop the process using port 8080:
1. Open Command Prompt (as Administrator)
2. Run: netstat -ano | findstr :8080
3. Note the PID (Process ID)
4. Kill it: taskkill /PID <process_id> /F
5. Restart Tomcat
```

---

## 📊 Sample Data

Database comes pre-populated with sample orders:

| Order ID | Customer | Restaurant | Amount | Status |
|----------|----------|-----------|--------|--------|
| ORD001 | Rajesh Kumar | Dominoes | ₹850.50 | Delivered |
| ORD002 | Priya Singh | Swiggy | ₹1200 | On the Way |
| ORD003 | Amit Patel | Zomato | ₹650.75 | Preparing |
| ORD004 | Neha Sharma | Biryani House | ₹1450 | Pending |
| ORD005 | Sanjay Verma | Pizza Hut | ₹799.99 | Delivered |

---

## 📚 Additional Documentation

**For Cloud Deployment:**
- **[RENDER_DEPLOYMENT.md](RENDER_DEPLOYMENT.md)** - Deploy to Render.com (free tier) ☁️

**For Windows Users (After Cloning):**
- **[WINDOWS_AFTER_CLONE.md](WINDOWS_AFTER_CLONE.md)** - Complete setup guide for Windows ⭐ **START HERE IF ON WINDOWS**
- **[WINDOWS_ERROR_FIX.md](WINDOWS_ERROR_FIX.md)** - Fix for batch script errors 🔧
- **[WINDOWS_MYSQL_FIX.md](WINDOWS_MYSQL_FIX.md)** - Fix for MySQL "Access Denied" errors 🗄️

**For All Users:**
- **[WINDOWS_SETUP.md](WINDOWS_SETUP.md)** - Windows installation & configuration guide
- **[SETUP_FOR_TEACHER.md](SETUP_FOR_TEACHER.md)** - 3-step quick start guide
- **[TEACHER_SUBMISSION.md](WebApp/TEACHER_SUBMISSION.md)** - Complete reference guide
- **[COMPLETE_GUIDE.md](WebApp/COMPLETE_GUIDE.md)** - In-depth technical documentation

---

## 🎯 Architecture Pattern

The application uses **MVC (Model-View-Controller)** pattern:

```
HTTP Request
    ↓
SessionCheckFilter (Authentication)
    ↓
Servlet (Controller) - Routes request
    ↓
DAO (Data Access) - Queries database
    ↓
FoodOrder (Model) - Data object
    ↓
JSP/HTML (View) - Renders response
    ↓
HTTP Response to Browser
```

---

## 🔐 Security Features

✅ Session-based authentication with HttpOnly cookies  
✅ Session timeout after 30 minutes of inactivity  
✅ Request filtering for protected routes  
✅ SQL injection protection using PreparedStatements  
✅ Input validation on all forms  
✅ Secure password handling (environment variables)  

---

## 🎓 Learning Resources

This project demonstrates:
- **Java Servlets** - HTTP request/response handling
- **JSP** - Server-side templating
- **JDBC** - Database connectivity
- **MVC Pattern** - Separation of concerns
- **Session Management** - User authentication
- **Maven** - Build automation
- **Web Deployment** - WAR packaging and Tomcat deployment

---

## 📝 License

Educational project for learning Java web development.

---

## 🤝 Support

For issues or questions:
1. Check [Troubleshooting](#-troubleshooting) section
2. Review logs: `tail /path/to/tomcat/logs/catalina.out`
3. Verify database: `mysql -u root -e "USE fooddb; SELECT 1;"`
4. Ensure all prerequisites are installed correctly

---

**Happy coding! 🚀**
