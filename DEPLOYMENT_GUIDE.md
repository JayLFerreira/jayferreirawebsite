# Deployment Guide for Your Portfolio Website

This guide covers multiple ways to deploy your portfolio website with blog functionality.

## File Structure

Your website should have this structure:
```
your-website/
├── portfolio.html (or index.html - your main page)
├── blog.html (blog listing page)
├── blog-post-1.html (individual blog post)
├── blog-post-2.html
├── blog-post-3.html
└── resume.pdf (your resume)
```

---

## Option 1: GitHub Pages (FREE - Recommended for Beginners)

**Pros:** Free, easy, automatic SSL, good for static sites
**Cons:** Public repository (unless you have GitHub Pro)

### Steps:

1. **Create a GitHub account** at https://github.com

2. **Create a new repository:**
   - Name it: `yourusername.github.io` (replace yourusername with your GitHub username)
   - Make it public
   - Initialize with README

3. **Upload your files:**
   - Click "Add file" → "Upload files"
   - Rename `portfolio.html` to `index.html` (important!)
   - Upload all your HTML files and resume.pdf
   - Commit the changes

4. **Enable GitHub Pages:**
   - Go to Settings → Pages
   - Source: Deploy from a branch
   - Branch: main, / (root)
   - Save

5. **Your site will be live at:** `https://yourusername.github.io`

### Custom Domain (Optional):
- Buy a domain (namecheap.com, domains.google.com)
- In GitHub Pages settings, add your custom domain
- Update DNS records at your domain provider:
  ```
  Type: A
  Host: @
  Value: 185.199.108.153
         185.199.109.153
         185.199.110.153
         185.199.111.153

  Type: CNAME
  Host: www
  Value: yourusername.github.io
  ```

---

## Option 2: Netlify (FREE - Best Overall)

**Pros:** Free, drag-and-drop, automatic SSL, custom domain, continuous deployment
**Cons:** None for static sites

### Steps:

1. **Sign up** at https://www.netlify.com

2. **Deploy via drag-and-drop:**
   - Click "Add new site" → "Deploy manually"
   - Drag your website folder into the upload area
   - Rename `portfolio.html` to `index.html` first!

3. **Your site is live!** Netlify gives you a random URL like `random-name-123.netlify.app`

4. **Custom Domain (Optional):**
   - Site settings → Domain management → Add custom domain
   - Follow DNS instructions

### Alternative - Deploy from GitHub:
- Connect your GitHub repository
- Every time you push changes, Netlify auto-deploys!

---

## Option 3: Vercel (FREE)

**Pros:** Similar to Netlify, great performance, edge network
**Cons:** Slight learning curve

### Steps:

1. **Sign up** at https://vercel.com

2. **Deploy:**
   - Click "Add New" → "Project"
   - Import from GitHub or drag-and-drop
   - Rename `portfolio.html` to `index.html`

3. **Live site:** `your-project.vercel.app`

---

## Option 4: AWS S3 + CloudFront (Your Existing Setup)

**Pros:** Scalable, professional, you're already familiar with AWS
**Cons:** Not free (but very cheap ~$1-5/month), requires AWS knowledge

### Steps:

1. **Create S3 Bucket:**
   ```bash
   aws s3 mb s3://jayferreira.com
   ```

2. **Enable Static Website Hosting:**
   - S3 Console → Your bucket → Properties
   - Static website hosting → Enable
   - Index document: `index.html`

3. **Upload Files:**
   ```bash
   aws s3 sync . s3://jayferreira.com --exclude ".git/*"
   ```

4. **Set Bucket Policy (make public):**
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::jayferreira.com/*"
           }
       ]
   }
   ```

5. **Create CloudFront Distribution:**
   - Origin: Your S3 bucket website endpoint
   - Viewer Protocol Policy: Redirect HTTP to HTTPS
   - Default Root Object: `index.html`

6. **Set up Route 53:**
   - Create hosted zone for your domain
   - Add A record (Alias) pointing to CloudFront distribution

---

## Option 5: Traditional Web Hosting (cPanel/Shared Hosting)

**Pros:** Simple, familiar interface, good support
**Cons:** Costs money ($3-10/month)

Popular providers:
- **Namecheap** (cheap, good for beginners)
- **Bluehost** (WordPress friendly)
- **SiteGround** (great support)

### Steps:

1. **Purchase hosting + domain**

2. **Access cPanel/File Manager**

3. **Upload files to `public_html` folder:**
   - Rename `portfolio.html` to `index.html`
   - Upload all files

4. **Site is live at your domain!**

---

## Recommended Approach for jayferreira.com

Since you already own jayferreira.com and have AWS experience, here's my recommendation:

### **Best Option: Netlify + Custom Domain**

1. **Deploy to Netlify:**
   - Drag-and-drop deployment (2 minutes)
   - Automatic HTTPS
   - Free forever for static sites

2. **Connect jayferreira.com:**
   - Add custom domain in Netlify
   - Update DNS at your domain registrar:
     ```
     Type: CNAME
     Host: www
     Value: your-site.netlify.app

     Type: A
     Host: @
     Value: 75.2.60.5 (Netlify's IP)
     ```

3. **Done!** Your site is live with:
   - Free HTTPS
   - Global CDN
   - Automatic deployments
   - Zero server management

---

## Updating Your Blog

To add new blog posts:

1. **Copy** `blog-post-1.html` and rename it (e.g., `blog-post-5.html`)

2. **Edit the content** - change title, date, and article content

3. **Update blog.html** - add a new blog post entry:
   ```html
   <article class="blog-post">
       <h2 class="blog-post-title">
           <a href="blog-post-5.html">Your New Post Title</a>
       </h2>
       <div class="blog-post-meta">January 20, 2025 / Category</div>
       <p class="blog-post-excerpt">
           Your post excerpt here...
       </p>
       <a href="blog-post-5.html" class="read-more">Read more →</a>
   </article>
   ```

4. **Upload/deploy** the updated files

---

## DNS Configuration (For Custom Domain)

If using jayferreira.com with any service:

**At your domain registrar** (where you bought the domain):

```
# For Netlify/Vercel
Type: CNAME, Host: www, Value: your-site.netlify.app
Type: A, Host: @, Value: [provided by host]

# For CloudFront
Type: A (Alias), Host: @, Value: CloudFront distribution
Type: CNAME, Host: www, Value: CloudFront distribution

# For GitHub Pages
Type: A, Host: @, Value: 185.199.108.153 (+ 3 others)
Type: CNAME, Host: www, Value: yourusername.github.io
```

---

## Quick Start (Fastest Way to Go Live)

**Want to be live in 5 minutes?**

1. Rename `portfolio.html` to `index.html`
2. Go to https://app.netlify.com/drop
3. Drag your folder onto the page
4. Done! You're live!

---

## Need Help?

- GitHub Pages Docs: https://docs.github.com/en/pages
- Netlify Docs: https://docs.netlify.com
- Vercel Docs: https://vercel.com/docs

Feel free to reach out if you need help with deployment!
