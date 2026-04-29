# ⚡ Quick Start: Deploy to Render in 5 Minutes

> **For detailed guide, see:** [RENDER_DEPLOYMENT.md](RENDER_DEPLOYMENT.md)

---

## 📋 What You Need

1. **Render.com Account** (free): https://render.com
2. **GitHub Account** (free): https://github.com
3. **MySQL Database** - Choose one:
   - [CleverCloud MySQL](https://www.clever-cloud.com) (free, easiest)
   - AWS RDS (has free tier)
   - JawsDB

---

## 🚀 Deploy in 5 Steps

### Step 1: Push Code to GitHub (2 min)

```bash
cd fooddeliveryApp
git add .
git commit -m "Ready for cloud deployment"
git push origin main
```

### Step 2: Create MySQL Database (2 min)

Go to https://www.clever-cloud.com → Create MySQL database → Get credentials

You'll get:
- Host: `xxx.mysql.clever-cloud.com`
- Username: `uxxx`
- Password: `your_password`
- Database: leave as default or create `fooddb`

### Step 3: Import Schema (1 min)

Run this command (replace with your CleverCloud credentials):

```bash
mysql -h xxx.mysql.clever-cloud.com -u uxxx -p fooddb < WebApp/database/fooddb.sql
```

### Step 4: Create Render Service (2 min)

1. Go to https://dashboard.render.com
2. Click **+ New** → **Web Service**
3. Choose your GitHub repo
4. Fill in:
   - **Name**: `fooddelivery-app`
   - **Build Command**: `cd WebApp && mvn clean package -DskipTests`
   - **Start Command**: `java -jar target/dependency/webapp-runner.jar --port $PORT target/fooddelivery.war`

### Step 5: Add Environment Variables (1 min)

Click **Environment** and add these (replace with your MySQL info):

```
FOODDB_URL = jdbc:mysql://xxx.mysql.clever-cloud.com:3306/fooddb?allowPublicKeyRetrieval=true&useSSL=false&serverTimezone=UTC
FOODDB_USER = uxxx
FOODDB_PASSWORD = your_password_here
PORT = 8080
```

**Done!** Click **Deploy** and wait 5-10 minutes.

---

## ✅ Verify Deployment

After deployment completes:

1. Go to Render Dashboard → your app → Copy URL
2. Open `https://your-app-name.onrender.com`
3. Login with:
   - Username: `admin`
   - Password: `admin123`
4. Try adding an order ✅

---

## 🆘 Troubleshooting

| Problem | Solution |
|---------|----------|
| Build fails | Check pom.xml exists in WebApp/ |
| Can't connect to database | Verify FOODDB_URL, FOODDB_USER, FOODDB_PASSWORD are correct |
| "Unknown database" | Import schema: `mysql ... fooddb < fooddb.sql` |
| Login doesn't work | Verify database has users table: `mysql ... -e "USE fooddb; SELECT * FROM users;"` |

---

## 🔄 Update After Changes

```bash
git add .
git commit -m "Your changes"
git push origin main
# Render auto-deploys!
```

---

## 📞 Need More Help?

See [RENDER_DEPLOYMENT.md](RENDER_DEPLOYMENT.md) for complete guide with:
- More MySQL options
- Detailed troubleshooting
- Monitoring & logging
- Best practices

---

**That's it! Your app is live on Render! 🎉**
