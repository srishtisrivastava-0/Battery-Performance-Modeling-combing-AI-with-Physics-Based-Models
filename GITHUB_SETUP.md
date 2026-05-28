# 🚀 GitHub Setup Guide

## Step-by-Step Instructions to Push Your Project to GitHub

---

## Prerequisites

1. **Git installed** on your computer
   - Check: `git --version`
   - Download from: https://git-scm.com/downloads

2. **GitHub account** created
   - Sign up at: https://github.com

---

## 📝 Step 1: Initialize Git Repository

Open terminal/command prompt in your project directory and run:

```bash
# Initialize git repository
git init

# Check status
git status
```

---

## 📦 Step 2: Add Files to Git

```bash
# Add all files to staging area
git add .

# Or add specific files
git add README.md requirements.txt

# Check what's staged
git status
```

---

## 💾 Step 3: Create First Commit

```bash
# Commit with a message
git commit -m "Initial commit: Battery Management System project"

# Verify commit
git log
```

---

## 🌐 Step 4: Create GitHub Repository

1. Go to https://github.com
2. Click the **"+"** icon (top right) → **"New repository"**
3. Fill in details:
   - **Repository name:** `battery-management-system` (or your choice)
   - **Description:** "AI/ML-Based Battery Management System for Electric Vehicles"
   - **Visibility:** Public or Private (your choice)
   - **DO NOT** initialize with README (we already have one)
4. Click **"Create repository"**

---

## 🔗 Step 5: Connect Local Repository to GitHub

GitHub will show you commands. Use these:

```bash
# Add remote repository
git remote add origin https://github.com/YOUR_USERNAME/battery-management-system.git

# Verify remote
git remote -v
```

**Replace `YOUR_USERNAME`** with your actual GitHub username!

---

## 🚀 Step 6: Push to GitHub

```bash
# Push to GitHub (first time)
git push -u origin main

# Or if your default branch is 'master'
git push -u origin master
```

**Note:** If you get an authentication error, you may need to:
- Use a Personal Access Token (PAT) instead of password
- Set up SSH keys

---

## 🔐 Authentication Options

### Option A: Personal Access Token (Recommended)

1. Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a name: "Battery Project"
4. Select scopes: `repo` (full control)
5. Click "Generate token"
6. **Copy the token** (you won't see it again!)
7. When pushing, use token as password

### Option B: SSH Keys

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Copy public key
cat ~/.ssh/id_ed25519.pub

# Add to GitHub: Settings → SSH and GPG keys → New SSH key
```

Then use SSH URL instead:
```bash
git remote set-url origin git@github.com:YOUR_USERNAME/battery-management-system.git
```

---

## 📋 Common Git Commands

### Making Changes

```bash
# Check status
git status

# Add changes
git add .

# Commit changes
git commit -m "Description of changes"

# Push to GitHub
git push
```

### Viewing History

```bash
# View commit history
git log

# View changes
git diff
```

### Branching (Optional)

```bash
# Create new branch
git branch feature-name

# Switch to branch
git checkout feature-name

# Create and switch in one command
git checkout -b feature-name

# Merge branch to main
git checkout main
git merge feature-name
```

---

## 🎯 Recommended Repository Structure

Your GitHub repository will look like:

```
battery-management-system/
├── README.md                    ← Main documentation
├── PROJECT_SUMMARY.md           ← For teacher presentation
├── GITHUB_SETUP.md              ← This file
├── requirements.txt             ← Dependencies
├── .gitignore                   ← Ignored files
├── datasets/                    ← Data (may be large!)
├── soh/                         ← SoH models
├── rul/                         ← RUL models
├── ecm/                         ← ECM models
├── hybrid/                      ← Hybrid models
└── ... (other directories)
```

---

## ⚠️ Important Notes

### Large Files Warning

If your `datasets/` folder is very large (>100MB), consider:

1. **Option A: Use Git LFS (Large File Storage)**
   ```bash
   git lfs install
   git lfs track "*.mat"
   git lfs track "*.csv"
   git add .gitattributes
   ```

2. **Option B: Don't upload datasets**
   - Add to `.gitignore`:
     ```
     datasets/
     ```
   - Provide download instructions in README instead

3. **Option C: Use external storage**
   - Upload datasets to Google Drive, Dropbox, etc.
   - Share link in README

### What NOT to Upload

Already excluded in `.gitignore`:
- `__pycache__/` - Python cache
- `.ipynb_checkpoints/` - Jupyter checkpoints
- `.kiro/` - Development artifacts
- `.pytest_cache/` - Test cache
- `.hypothesis/` - Testing artifacts

---

## 🎨 Customize Your Repository

### Add a License

1. On GitHub, click "Add file" → "Create new file"
2. Name it `LICENSE`
3. Click "Choose a license template"
4. Select MIT License (recommended for academic projects)
5. Commit

### Add Topics/Tags

On your repository page:
1. Click the gear icon next to "About"
2. Add topics: `battery-management`, `machine-learning`, `deep-learning`, `electric-vehicles`, `python`
3. Save

### Create a Nice Repository Description

In "About" section, add:
- Description: "AI/ML-Based Battery Management System for EVs using ML, DL, ECM, and Hybrid approaches"
- Website: (if you have one)
- Topics: battery, machine-learning, deep-learning, lstm, electric-vehicles

---

## 📊 Making Your Repository Stand Out

### Add Badges to README

Add these at the top of README.md:

```markdown
![Python](https://img.shields.io/badge/python-3.8+-blue.svg)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Status](https://img.shields.io/badge/status-active-success.svg)
```

### Add Screenshots

1. Create a `screenshots/` folder
2. Add images of your results
3. Reference in README:
   ```markdown
   ![SoH Prediction](screenshots/soh_prediction.png)
   ```

---

## 🔄 Updating Your Repository

After making changes:

```bash
# 1. Check what changed
git status

# 2. Add changes
git add .

# 3. Commit with descriptive message
git commit -m "Add improved hybrid model with better accuracy"

# 4. Push to GitHub
git push
```

---

## 🆘 Troubleshooting

### Problem: "fatal: not a git repository"
**Solution:** Run `git init` first

### Problem: "failed to push some refs"
**Solution:** Pull first, then push
```bash
git pull origin main --rebase
git push
```

### Problem: "large files detected"
**Solution:** Use Git LFS or remove large files
```bash
git rm --cached large_file.csv
echo "large_file.csv" >> .gitignore
git commit -m "Remove large file"
```

### Problem: Authentication failed
**Solution:** Use Personal Access Token instead of password

---

## ✅ Verification Checklist

After pushing, verify on GitHub:

- [ ] All files are uploaded
- [ ] README.md displays correctly
- [ ] Code syntax highlighting works
- [ ] Notebooks render properly
- [ ] .gitignore is working (no cache files)
- [ ] Repository description is set
- [ ] Topics/tags are added

---

## 🎓 For Teacher Presentation

Share this URL with your teacher:
```
https://github.com/YOUR_USERNAME/battery-management-system
```

They can:
- View all code
- Read documentation
- Download the project
- See commit history
- Review your work

---

## 📞 Need Help?

- **Git Documentation:** https://git-scm.com/doc
- **GitHub Guides:** https://guides.github.com
- **Git Cheat Sheet:** https://education.github.com/git-cheat-sheet-education.pdf

---

**Good luck with your presentation! 🚀**
