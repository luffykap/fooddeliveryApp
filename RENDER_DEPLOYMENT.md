# 🚀 Deploy Food Delivery App to Render

> **Platform:** Render.com  
> **Type:** Java Web App + MySQL Database  
> **Status:** Ready for deployment

---

## 📋 Prerequisites

1. **Render Account**: https://render.com (free tier available)
2. **GitHub Repository**: Push code to GitHub
3. **MySQL Database**: Options below
4. **Git CLI**: For pushing code

---

## 🔧 Step 1: Prepare for Render

### 1.1 Update Render Configuration

The project includes `render.yaml` which is already configured. Verify it exists:

```bash
cat render.yaml
```

### 1.2 Create MySQL Database (Choose ONE)

**Option A: Render Database Service (Recommended)**
- Render offers PostgreSQL, but NOT MySQL
- **Solution:** Use external MySQL service (see Option B)

**Option B: Use CleverCloud MySQL (Free)**
1. Go to: https://www.clever-cloud.com
2. Sign up (free account)
3. Create a new MySQL database
4. Get connection details:
   - Host
   - Port (usually 3306)
   - Username (usually `username_id`)
   - Password

**Option C: Use AWS RDS MySQL**
1. Create free tier RDS MySQL instance
2. Get endpoint, username, password

**Option D: Use JawsDB (Heroku MySQL)**
1. Go to: https://www.jawsdb.com
2. Get MySQL connection details

---

## 🔑 Step 2: Configure Environment Variables on Render

1. Go to: https://dashboard.render.com
2. Create a new **Web Service**
3. Connect to your GitHub repository
4. In **Environment** section, add these variables:

```
FOODDB_URL=jdbc:mysql://your-mysql-host:3306/fooddb?allowPublicKeyRetrieval=true&useSSL=false&serverTimezone=UTC
FOODDB_USER=your_mysql_username
FOODDB_PASSWORD=your_mysql_password
PORT=8080
```

**Example (CleverCloud):**
```
FOODDB_URL=jdbc:mysql://xxx.mysql.clever-cloud.com:3306/xxxxx?allowPublicKeyRetrieval=true&useSSL=false&serverTimezone=UTC
FOODDB_USER=uxxx
FOODDB_PASSWORD=your_password_here
```

---

## 📦 Step 3: Deploy to Render

### 3.1 Push Code to GitHub

```bash
cd fooddeliveryApp
git add .
git commit -m "Ready for Render deployment"
git push origin main
```

### 3.2 Create Render Service

1. Go to: https://dashboard.render.com
2. Click **+ New** → **Web Service**
3. Choose **Connect a repository**
4. Select your GitHub repo (fooddeliveryApp)
5. Configure:
   - **Name**: `fooddelivery-app`
   - **Runtime**: `Java`
   - **Build Command**: `cd WebApp && mvn clean package -DskipTests && mvn dependency:copy-dependencies -DoutputDirectory=target/dependency`
   - **Start Command**: `java -jar target/dependency/webapp-runner.jar --port $PORT target/fooddelivery.war`
   - **Plan**: Free (or paid for better performance)

### 3.3 Add Environment Variables

In the **Environment** section:
- Add all 4 variables from Step 2

### 3.4 Deploy

Click **Deploy** and wait for build to complete (5-10 minutes)

---

## 🗄️ Step 4: Setup MySQL Database

### 4.1 If Using CleverCloud

```bash
# Connect to CleverCloud MySQL
mysql -h xxx.mysql.clever-cloud.com -u uxxx -p

# When prompted, enter password
```

### 4.2 Import Schema

Once connected to MySQL:

```sql
CREATE DATABASE IF NOT EXISTS fooddb;
USE fooddb;

-- Then paste contents of WebApp/database/fooddb.sql
```

Or from terminal:

```bash
mysql -h your-host -u your-user -p fooddb < WebApp/database/fooddb.sql
```

### 4.3 Verify

```bash
mysql -h your-host -u your-user -p -e "USE fooddb; SHOW TABLES;"
```

Should show:
```
+--------------+
| Tables_in_fooddb |
+--------------+
| orders       |
| users        |
| premium_orders |
+--------------+
```

---

## 🎯 Step 5: Access Application

Once deployment is complete, you'll get a URL like:
```
https://fooddelivery-app.onrender.com
```

### Login

- **Username**: `admin`
- **Password**: `admin123`

### Features Available

- ➕ Add Order
- 📋 View Orders
- 💎 Premium Orders (>₹1000)
- 📊 Dashboard

---

## 🔍 Troubleshooting

### Build Fails: "Maven not found"

**Fix:** Render automatically detects `pom.xml`. Ensure it's in `WebApp/` directory.

### Deployment Fails: "Port already in use"

**Fix:** Render assigns port dynamically via `$PORT` variable (already in start command).

### Database Connection Error

**Check:**
```bash
1. Environment variables are correct
2. Database is created
3. MySQL host/user/password are correct
4. Database allows public connections (especially important!)
```

**For CleverCloud MySQL:**
- Go to dashboard → MySQL instance → Network → add Render IP to whitelist

**For AWS RDS:**
- Go to Security Groups → Inbound Rules → Allow port 3306 from Render

### "Access denied for user"

**Fix:**
1. Verify FOODDB_PASSWORD is correct
2. Check username matches your MySQL user
3. Ensure MySQL allows root login (or use your custom user)

### "Unknown database 'fooddb'"

**Fix:**
1. Connect to MySQL
2. Run: `CREATE DATABASE fooddb;`
3. Run: `mysql -u user -p fooddb < WebApp/database/fooddb.sql`
4. Restart Render deployment

---

## 📊 Monitoring

### View Logs

1. Go to Render Dashboard
2. Select your service
3. Click **Logs** tab
4. Watch for errors during build/deployment

### Common Log Errors

```
[ERROR] COMPILATION ERROR : 
```
→ Check pom.xml dependencies, likely Jakarta API issue

```
ERROR: Access denied for user 'root'@'xxx'
```
→ Check FOODDB_PASSWORD, MySQL credentials

```
ERROR: Unknown host 'xxx.mysql.clever-cloud.com'
```
→ Check FOODDB_URL is correct, MySQL is running

---

## 💡 Tips & Best Practices

1. **Keep Logs Clean**: Render free tier has limited logging
2. **Monitor Performance**: Check Dashboard → Metrics
3. **Use Custom Domain**: Render → Settings → Custom Domain
4. **Enable Auto-Deploy**: Render → Settings → Auto-deploy enabled
5. **Set Up Error Notifications**: Render → Settings → Notifications

---

## 🔄 Redeploy After Changes

1. Make changes locally
2. Commit and push:
   ```bash
   git add .
   git commit -m "Your message"
   git push origin main
   ```
3. Render auto-deploys (if enabled)
4. Or manually click **Deploy** in Render Dashboard

---

## 📝 Environment Variables Reference

| Variable | Example | Notes |
|----------|---------|-------|
| `FOODDB_URL` | `jdbc:mysql://host:3306/fooddb?allowPublicKeyRetrieval=true&useSSL=false` | MySQL connection string |
| `FOODDB_USER` | `admin` | MySQL username |
| `FOODDB_PASSWORD` | `secure_password` | MySQL password |
| `PORT` | `8080` | Auto-set by Render |

---

## ✅ Deployment Checklist

- [ ] GitHub repository is public
- [ ] `render.yaml` exists in project root
- [ ] MySQL database is created
- [ ] Database schema is imported
- [ ] Environment variables are set on Render
- [ ] Build command works locally: `mvn clean package`
- [ ] Start command is correct: `java -jar target/dependency/webapp-runner.jar --port $PORT target/fooddelivery.war`
- [ ] Application loads at `https://your-app.onrender.com`
- [ ] Can login with admin/admin123
- [ ] Can add/view orders

---

## 🆘 Need Help?

**Check These Files:**
- [README.md](../README.md) - General troubleshooting
- [COMPLETE_GUIDE.md](../WebApp/COMPLETE_GUIDE.md) - Technical details
- [DBConnection.java](../WebApp/src/main/java/com/fooddelivery/util/DBConnection.java) - Database connection logic

**Render Documentation:**
- https://render.com/docs
- https://render.com/docs/deploy-java

**MySQL Documentation:**
- https://dev.mysql.com/doc/

---

**Good luck with your Render deployment! 🚀**
