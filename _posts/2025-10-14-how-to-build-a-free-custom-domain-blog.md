## How to Build a Free Custom-Domain Blog Using GitHub Pages and Jekyll

### Introduction
I built this entire blog for free using **GitHub Pages** and **Jekyll**, with my own custom domain and HTTPS.  
If you want a personal blog that costs nothing to host and looks professional, this guide walks through every step.

---

### 1. Create Your GitHub Repository
1. Log in to GitHub and create a new repo named:
   ```
   yourusername.github.io
   ```
   Replace `yourusername` with your actual GitHub username.
2. Leave it public — GitHub Pages requires that for free hosting.

---

### 2. Connect Your Local Folder
If you already have files on your computer, link them to GitHub:

```bash
cd /path/to/your/blog
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<yourusername>/<yourusername>.github.io.git
git branch -M main
git push -u origin main
```

Now your local folder is connected to your GitHub repo.

---

### 3. Add Jekyll Configuration
In the root of your repo, create `_config.yml`:

```yaml
title: Your Blog Title
description: Personal tech notes and experiments
theme: minima
```

The **Minima** theme is included with GitHub Pages and is mobile-friendly.

---

### 4. Add Your Homepage
Create an `index.md` file:

```markdown
---
layout: home
title: "Welcome"
---

Hi, I’m Krishna — a tech enthusiast who loves exploring gadgets, phones, and all things digital.

This blog is where I share what I’m working on, what I’m tinkering with, and other experiments in tech.
```

---

### 5. Add Your First Post
Inside a folder named `_posts`, create a file like:

```
_posts/2025-10-13-my-first-post.md
```

```markdown
---
title: "My first post"
date: 2025-10-13
layout: post
---

**Hi everyone!**

Welcome to my new blog. This is where I’ll share what I’m working on, the tech I explore, and my thoughts on phones, gadgets, and other interesting stuff I come across.
```

Jekyll reads the date from the filename and automatically builds a post URL like:

```
https://yourusername.github.io/2025/10/13/my-first-post.html
```

---

### 6. Enable GitHub Pages
1. Go to **Settings → Pages** in your repository.  
2. Under **Source**, select:
   ```
   Deploy from a branch
   ```
3. Set:
   - **Branch:** main  
   - **Folder:** /(root)

After a short build, your site will appear at:
```
https://yourusername.github.io
```

---

### 7. Add a Custom Domain
If you have a custom domain (like `yourblog.com` or from any free DNS provider), create DNS records pointing to GitHub Pages.

Use the following A records:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Then, in your GitHub repo root, create a file named **CNAME** with:
```
yourblog.com
```

Finally, go to **Settings → Pages** and check **Enforce HTTPS**.  
Your blog will now load securely from your custom domain.

---

### 8. Adding Code Blocks in Jekyll
Jekyll supports Markdown code blocks with syntax highlighting.

```bash
git add .
git commit -m "update"
git push
```

You can specify the language for highlighting:

```python
def greet(name):
    print(f"Hello, {name}!")
```

Make sure your `_config.yml` has:
```yaml
markdown: kramdown
highlighter: rouge
```

---

### 9. Optional Enhancements
- Add an **About** page (`about.md`)
- Include a **favicon** for browser tabs  
- Customize colors or layout via `_sass/minima`  
- Add **Google Analytics** or **Plausible** for traffic stats  
- Write posts in Affine → Export as Markdown → Push to GitHub  

---

### TL;DR
- Create a repo named `yourusername.github.io`  
- Add `_config.yml`, `index.md`, and `_posts/`  
- Enable GitHub Pages in repo settings  
- Point your domain to GitHub IPs  
- Enjoy your free HTTPS-secured personal blog 🎉  

---

**And that’s it!**  
You now have a fully working, mobile-friendly, custom-domain blog built entirely on free tools — GitHub Pages and Jekyll.
