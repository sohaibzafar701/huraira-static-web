# GitHub Pages Deployment Guide for shopinestllc

This guide will help you deploy your static website to GitHub Pages and connect your Hostinger domain.

## Files to Upload to GitHub

### Required HTML Files (Upload all of these):
1. `index.html` - Homepage
2. `category.html` - Category page
3. `about.html` - About page
4. `contact.html` - Contact page
5. `auto-parts.html` - Auto Parts homepage
6. `auto-parts-category.html` - Auto Parts category page
7. `order-confirmation.html` - Order confirmation page

### Optional Files (If they exist):
- `robots.txt` - For SEO (if you have one)
- `sitemap.xml` - For SEO (if you have one)
- Any custom CSS files (though you're using Tailwind CDN, so not needed)

### DO NOT Upload:
- ❌ Any `.php` files
- ❌ Database files (`.sql`)
- ❌ Configuration files with passwords
- ❌ `index.php`, `admin.php`, etc.

---

## Step 1: Create GitHub Repository

1. Go to [GitHub.com](https://github.com) and sign in
2. Click the **"+"** icon in the top right → **"New repository"**
3. Repository name: `shopinestllc` (or any name you prefer)
4. Description: "shopinestllc - E-commerce website"
5. Set to **Public** (GitHub Pages free tier requires public repos)
6. **DO NOT** check "Initialize with README" (you'll upload files manually)
7. Click **"Create repository"**

---

## Step 2: Upload Files to GitHub

### Option A: Using GitHub Web Interface (Easiest)

1. After creating the repository, you'll see a page with setup instructions
2. Click **"uploading an existing file"** link
3. Drag and drop all 7 HTML files into the upload area
4. Scroll down and add a commit message: "Initial commit - Static HTML website"
5. Click **"Commit changes"**

### Option B: Using Git Command Line

```bash
# Navigate to your project folder
cd C:\xampp\htdocs\huraira

# Initialize git (if not already done)
git init

# Add all HTML files
git add *.html

# Commit
git commit -m "Initial commit - Static HTML website"

# Add your GitHub repository as remote
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

# Push to GitHub
git branch -M main
git push -u origin main
```

---

## Step 3: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click on **"Settings"** tab (top menu)
3. Scroll down to **"Pages"** in the left sidebar
4. Under **"Source"**, select:
   - Branch: `main` (or `master`)
   - Folder: `/ (root)`
5. Click **"Save"**
6. Wait 1-2 minutes for GitHub to build your site
7. You'll see a message: "Your site is live at `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/`"

**Important**: If your repository is named `shopinestllc`, your site URL will be:
`https://YOUR_USERNAME.github.io/shopinestllc/`

---

## Step 4: Configure Custom Domain (Hostinger)

### Step 4.1: Add Custom Domain to GitHub

1. In your GitHub repository → **Settings** → **Pages**
2. Scroll to **"Custom domain"** section
3. Enter your domain: `www.yourdomain.com` (replace with your actual domain)
4. Click **"Save"**
5. GitHub will create a `CNAME` file automatically (or you can create it manually - see below)

### Step 4.2: Create CNAME File (Alternative Method)

1. In your repository, click **"Add file"** → **"Create new file"**
2. File name: `CNAME` (all caps, no extension)
3. Content: Enter your domain (one per line):
   ```
   www.yourdomain.com
   yourdomain.com
   ```
4. Click **"Commit new file"**

**Note**: You can add both `www.yourdomain.com` and `yourdomain.com` (without www)

---

## Step 5: Configure DNS Settings in Hostinger

1. Log in to your **Hostinger account**
2. Go to **"Domains"** → Select your domain
3. Click on **"DNS / Nameservers"** or **"Advanced DNS"**

### Required DNS Records:

Add these DNS records:

| Type | Name | Value | TTL |
|------|------|-------|-----|
| **A** | `@` | `185.199.108.153` | 3600 |
| **A** | `@` | `185.199.109.153` | 3600 |
| **A** | `@` | `185.199.110.153` | 3600 |
| **A** | `@` | `185.199.111.153` | 3600 |
| **CNAME** | `www` | `YOUR_USERNAME.github.io` | 3600 |

### Detailed Instructions:

1. **A Records (for root domain - yourdomain.com)**:
   - Type: `A`
   - Name: `@` (or leave blank, or `yourdomain.com`)
   - Value: `185.199.108.153`
   - TTL: `3600` (or default)
   - Click **"Add Record"**
   
   Repeat for all 4 IP addresses:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`

2. **CNAME Record (for www subdomain)**:
   - Type: `CNAME`
   - Name: `www`
   - Value: `YOUR_USERNAME.github.io` (replace with your GitHub username)
   - TTL: `3600`
   - Click **"Add Record"**

### Example:
If your GitHub username is `johnsmith` and domain is `mysite.com`:
- A Records: `@` → `185.199.108.153`, etc.
- CNAME: `www` → `johnsmith.github.io`

---

## Step 6: Enable HTTPS (Automatic)

1. After DNS propagates (can take 24-48 hours, usually faster)
2. GitHub will automatically provision SSL certificate
3. In GitHub → Settings → Pages, check **"Enforce HTTPS"** checkbox
4. This will redirect all HTTP traffic to HTTPS

---

## Step 7: Verify Domain Connection

### Check DNS Propagation:
1. Visit [whatsmydns.net](https://www.whatsmydns.net/)
2. Enter your domain
3. Check if A records point to GitHub IPs (185.199.108.x)

### Test Your Website:
1. Visit `http://yourdomain.com`
2. Visit `http://www.yourdomain.com`
3. Both should redirect to your GitHub Pages site

---

## Troubleshooting

### Issue: Site shows "404 Not Found"

**Solution**:
- Make sure your `index.html` file is in the root directory
- Check that GitHub Pages is enabled (Settings → Pages)
- Wait a few minutes for GitHub to rebuild

### Issue: Domain shows "Not Configured"

**Solution**:
- Verify CNAME file exists in your repository root
- Check DNS settings in Hostinger
- Wait for DNS propagation (can take up to 48 hours)

### Issue: Mixed Content Warnings (HTTP/HTTPS)

**Solution**:
- Enable "Enforce HTTPS" in GitHub Settings → Pages
- Make sure all external links use `https://`

### Issue: CSS/JavaScript Not Loading

**Solution**:
- Check browser console for errors
- Verify CDN links are correct (Tailwind CSS, Font Awesome)
- All your pages use CDN links, so this shouldn't be an issue

---

## Quick Checklist

- [ ] Created GitHub repository
- [ ] Uploaded all 7 HTML files
- [ ] Enabled GitHub Pages (Settings → Pages)
- [ ] Added custom domain in GitHub (CNAME file)
- [ ] Configured DNS in Hostinger (4 A records + 1 CNAME)
- [ ] Waited for DNS propagation (1-48 hours)
- [ ] Enabled HTTPS in GitHub
- [ ] Tested website on both `yourdomain.com` and `www.yourdomain.com`

---

## GitHub Pages URLs Structure

Your pages will be accessible as:
- `https://yourdomain.com/` → Homepage
- `https://yourdomain.com/about.html` → About
- `https://yourdomain.com/contact.html` → Contact
- `https://yourdomain.com/category.html?id=1` → Category
- `https://yourdomain.com/auto-parts.html` → Auto Parts
- `https://yourdomain.com/order-confirmation.html` → Order Confirmation

---

## Need Help?

If you encounter any issues:
1. Check GitHub Pages status: Repository → Settings → Pages
2. Verify DNS: Use [whatsmydns.net](https://www.whatsmydns.net/)
3. Check GitHub Actions tab for build errors
4. Verify all file names are lowercase (HTML files should be lowercase)

---

**Good luck with your deployment! 🚀**
