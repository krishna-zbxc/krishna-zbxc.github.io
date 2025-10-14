## How to Build a Free Blog Using GitHub Pages and Jekyll

### Introduction
You can build a complete, secure, and professional-looking blog for free using GitHub Pages and Jekyll.  
All you need is a GitHub account — no servers, no paid hosting, and no setup costs.

---

### 1. Create a GitHub Repository
1. Log in to GitHub and create a new repository named:
   yourusername.github.io
   Replace "yourusername" with your GitHub username.  
   Example: if your username is alex123, name your repo alex123.github.io.
2. Keep the repository public — that’s required for GitHub Pages.

---

### 2. Clone the Repository to Your Computer
If you don’t have any files yet, start fresh by cloning the empty repo:

git clone https://github.com/<yourusername>/<yourusername>.github.io.git
cd <yourusername>.github.io

Now you can create your blog files inside this folder.

---

### 3. Add Jekyll Configuration
Create a file named _config.yml in the root of your folder.  
This tells GitHub Pages how to build your site.

title: My Blog
description: A simple blog built with GitHub Pages and Jekyll
theme: minima

The Minima theme comes preinstalled with GitHub Pages and works perfectly on desktop and mobile.

---

### 4. Add a Homepage
Create a file called index.md:

---
layout: home
title: "Welcome"
---

Welcome to my new blog!  
This site is powered by Jekyll and hosted for free on GitHub Pages.  

Stay tuned for updates and new posts.

---

### 5. Add Your First Blog Post
Create a folder named _posts, then add a file with this format:

_posts/YYYY-MM-DD-title.md

Example:
_posts/2025-10-15-my-first-post.md

Inside the file, add:

---
title: "My First Post"
date: 2025-10-15
layout: post
---

This is my very first post!  
Jekyll will automatically generate a page for it and display it on the homepage.

Jekyll uses the date from the filename to sort posts chronologically.

---

### 6. Enable GitHub Pages
1. Go to your repository on GitHub.  
2. Click Settings → Pages.  
3. Under Source, choose "Deploy from a branch".  
4. Select:
   - Branch: main  
   - Folder: /(root)
5. Click Save.

After a minute or two, your site will be live at:
https://yourusername.github.io

---

### 7. (Optional) Add a Custom Domain
If you have your own domain (for example, myblog.com), you can point it to GitHub Pages.

1. Go to your domain provider’s DNS settings.  
2. Add a CNAME record:  
   - Name: yourblog.com  
   - Value: yourusername.github.io  
3. In your repository root, create a file named CNAME containing your domain name:  
   myblog.com  
4. In Settings → Pages, check "Enforce HTTPS".

Your site will now be available securely at your custom domain.

---

### 8. Adding Code Blocks
You can include code snippets in your posts using Markdown’s fenced code blocks.

Example:

git add .
git commit -m "First post"
git push

To highlight a specific language:

def greet(name):
    print(f"Hello, {name}!")

Make sure your _config.yml includes:

markdown: kramdown
highlighter: rouge

---

### 9. Optional Enhancements
- Add an About page (about.md)  
- Include a favicon for your site  
- Customize colors or layout in _sass/minima  
- Add analytics (Google Analytics or Plausible)  
- Write posts locally → commit → push → site updates automatically  

---

### TL;DR
- Create a public repo named yourusername.github.io  
- Add _config.yml, index.md, and _posts/  
- Enable GitHub Pages in Settings  
- (Optional) Add a custom domain with a CNAME record  
- Every time you push a post, it goes live automatically 🎉  

---

You now have a fully functional, free, mobile-friendly blog built with GitHub Pages and Jekyll.  
Happy blogging!
