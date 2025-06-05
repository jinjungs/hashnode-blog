---
title: "How to Back Up Hashnode to GitHub Automatically (with GitHub Actions)"
seoTitle: "Automate Hashnode Backups to GitHub"
seoDescription: "Learn to automate Hashnode backups to GitHub using GitHub Actions, keeping posts organized and filenames readable. Perfect for tech bloggers"
datePublished: Thu Jun 05 2025 21:26:26 GMT+0000 (Coordinated Universal Time)
cuid: cmbjvz98x000009ju1cav53jb
slug: hashnode-back-up-with-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749096130557/2465e8aa-5fa1-4e83-a5a9-a270b4e72195.png
tags: github, hashnode, backup, integration, github-actions-1

---

## ✍️ **Why I wrote this**

I created my Hashnode blog about six months ago and recently started posting again.

In the past, I used GitHub Pages with [github.io](https://jinjungs.github.io/) for blogging — which meant maintaining a GitHub repo, writing or updating `.md` files manually, then committing and pushing them. It was a bit tedious, but I liked that it helped me track both blog posts and GitHub activity at the same time.

That made me wonder: **Is there a way to commit to GitHub automatically when publishing on Hashnode?** Turns out — yes, there is!

This post is a walkthrough of how I set up **GitHub Actions to automatically back up my Hashnode blog**, organize posts into folders, and rename those unreadable `.md` filenames.

---

## 🧞‍♂️ **What I want**

1. Automatically commit to GitHub whenever I publish a Hashnode post
    
2. Organize posts into folders by series/topic
    
3. Rename .md files to readable filenames instead of UUIDs
    

---

## **✅ Step 1 – Connect Hashnode to GitHub**

### **1.1. Create a GitHub Repository**

Create a new GitHub repository that will store your backed-up Hashnode posts.  
It can be public or private — but I recommend public if you’re using it as a writing portfolio.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749154245768/8946829b-0a91-4e9f-ba28-35272eaff2be.png align="center")

### **1.2. Enable GitHub Integration in Hashnode**

Go to:  
Hashnode Dashboard → GitHub → GitHub Integration → Install Hashnode App → Install & Authorize

> ⚠️ Do not select “All repositories.”  
> Choose only the specific repository you created for this sync.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749154500337/8447b953-b285-45f8-979f-532cbbe2ae93.png align="center")

After install & authorizing, you will see the drafts, articles and the status of backed up.  
Click “Back up all articles” to backup.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749154828526/df83dac2-e920-4849-b4be-4b225e7939d6.png align="center")

**Congratulation!** 🎉  
**You have Successfully sync your Hashnode with Github!**

Once installed and configured, your published posts will be backed up as `.md` files to your GitHub repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749100322141/ff601a63-7c01-484e-9162-f773ecc2440e.png align="center")

---

## **✅ Step 2 – Auto-Organize with GitHub Actions**

I wanted to automatically organize my posts into folders based on topic, like this:

```plaintext
hashnode-blog/
├── python-basics/
│   └── print-formatting.md
├── coding-test/
│   ├── sorting-algorithms-1.md
│   └── essential-libraries.md
```

### **2.1. Check GitHub Action Permissions**

Go to:  
Github project (you made in Step 1) Settings -&gt; Actions -&gt; General -&gt; Workflow permissions → Make sure Read and write permissions are enabled.

> ⚠️ Without this, you’ll get an error like:

```yaml
remote: Permission to [your-repo].git denied to github-actions[bot]
fatal: unable to access [...]: 403
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749100815135/ec668cf3-aed5-4f85-9e1f-1e4f66b02a19.png align="center")

### **2.2. GitHub Actions Workflow**

Create a file at:  
.github/workflows/organize-posts.yml

```yaml
name: Organize Blog Posts by Slug

on:
  push:
    paths:
      - "*.md"

jobs:
  organize:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install Python YAML parser
        run: pip install pyyaml

      - name: Organize files into series folders
        run: |
          import os, yaml, shutil
          for filename in os.listdir("."):
              if filename.endswith(".md"):
                  with open(filename, 'r', encoding='utf-8') as f:
                      lines = f.readlines()
                  if lines[0].strip() == "---":
                      frontmatter = []
                      for line in lines[1:]:
                          if line.strip() == "---":
                              break
                          frontmatter.append(line)
                      meta = yaml.safe_load("".join(frontmatter))
                      series = meta.get("series")
                      if series:
                          os.makedirs(series, exist_ok=True)
                          dest = os.path.join(series, filename)
                          shutil.move(filename, dest)

        shell: python

      - name: Commit changes
        run: |
          git config user.name "github-actions"
          git config user.email "github-actions@github.com"
          git add .
          git commit -m "Organized posts by series" || echo "No changes to commit"
          git pull --rebase origin main
          git push
```

You can check in the “Actions Tab” to see the action is properly registered. Once the workflow is added, you’ll see it running in the **Actions tab** whenever you push `.md` files to the repo.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749101628117/811cd502-dc6f-4028-930d-861a1c331a3b.png align="center")

### **2.3. Testing the Workflow**

To test it, I restored a deleted post and triggered a new backup.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749101775110/bbc12d51-75d0-4174-b45f-0ccadab1676a.png align="center")

Then I checked if the workflow succeeded and if the posts were organized into folders.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749101817083/ad18a958-5bea-4913-95e5-5fd819b6a332.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749101924008/b1d7f33d-0a21-4c03-b856-addff8121b1e.png align="center")

**Everything worked!** 🎉  
The file is now categorized automatically when you publish a new post.

---

## **✅ Step 3 – Renaming Files to Slug**

But there’s one problem left. By default, Hashnode backups use a random UID for filenames. This makes it hard to manage or find specific posts.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749101994165/dafbd150-0871-48bb-9b03-7382e64cb4f5.png align="center")

To fix that, let’s change the workflow to rename the files based on the post’s slug.

```python
slug = meta.get("slug", "")
new_filename = f"{slug}.md"
os.rename(filename, new_filename)
```

Here’s the complete updated YAML file with file renaming and folder categorization logic.

```yaml
name: Organize Blog Posts by Slug

on:
  push:
    paths:
      - "*.md"
  
jobs:
  organize:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install PyYAML
        run: pip install pyyaml

      - name: Organize by slug keyword
        run: |
          import os, yaml, shutil

          SLUG_KEYWORDS = {
              "coding-test": "coding-test",
              "python-basics": "python-basics"
          }

          for filename in os.listdir("."):
              if filename.endswith(".md"):
                  with open(filename, 'r', encoding='utf-8') as f:
                      lines = f.readlines()

                  if lines[0].strip() == "---":
                      frontmatter = []
                      for line in lines[1:]:
                          if line.strip() == "---":
                              break
                          frontmatter.append(line)

                      meta = yaml.safe_load("".join(frontmatter))
                      slug = meta.get("slug", "")

                      # rename file to slug
                      new_filename = f"{slug}.md"
                      os.rename(filename, new_filename)

                      # Then, use new_filename from this point forward
                      matched = False
                      for keyword, folder in SLUG_KEYWORDS.items():
                          if keyword in slug:
                              os.makedirs(folder, exist_ok=True)
                              shutil.move(new_filename, os.path.join(folder, new_filename))  # ← updated here
                              matched = True
                              print(f"Moved {new_filename} to {folder}/")
                              break

                      if not matched:
                          print(f"No matching slug keyword found for: {new_filename}")
        shell: python

      - name: Commit organized files
        run: |
          git config user.name "github-actions"
          git config user.email "github-actions@github.com"
          git add .
          git commit -m "Organized posts by slug keywords" || echo "No changes"
          git pull --rebase origin main
          git push
```

I synced again in Hashnode and then checked if it was successful.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749103806572/d088ff6b-158c-4c42-a8cd-77b8cc41c72d.png align="center")

**Sync complete!** 🎉  
Your blog posts are now cleanly organized and version-controlled.

---

## ✨ Conclusion

Setting up this backup and organization workflow might seem like overkill at first — but once it’s working, it feels magical.

Every time I hit “Publish” on Hashnode, I know:

* My content is safe on GitHub
    
* Posts are categorized in folders
    
* Filenames are readable and meaningful
    
* My GitHub contribution graph is getting greener 🌱
    

This setup is perfect for anyone building a writing habit or maintaining a clean, version-controlled tech blog.

> Want to see how everything works behind the scenes?  
> Check out the full repo here:  
> 🔗 [jinjungs/hashnode-blog](https://github.com/jinjungs/hashnode-blog)

Enjoyed this post or have suggestions to make it better?  
Leave a comment — I’d love to hear from you.

Thanks for reading! 🙌

---

## Reference

* [Permission denied to github-actions\[bot\]](https://stackoverflow.com/questions/72851548/permission-denied-to-github-actionsbot)