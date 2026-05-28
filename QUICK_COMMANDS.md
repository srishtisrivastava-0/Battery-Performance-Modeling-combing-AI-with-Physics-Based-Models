# ⚡ Quick Command Reference

## 🚀 Complete GitHub Setup with Datasets

### One-Time Setup (Run Once)

```bash
# 1. Install Git LFS (choose your platform)
# Windows: Download from https://git-lfs.github.com/
# Mac: brew install git-lfs
# Linux: sudo apt-get install git-lfs

# 2. Initialize Git and Git LFS
git init
git lfs install

# 3. Add all files
git add .

# 4. Create first commit
git commit -m "Initial commit: Battery Management System with datasets"

# 5. Create GitHub repository (do this on GitHub.com first)
# Then connect:
git remote add origin https://github.com/YOUR_USERNAME/battery-management-system.git

# 6. Push everything (including 1.2GB datasets)
git push -u origin main
```

---

## 📦 Dataset-Specific Commands

### Check Dataset Status
```bash
# See what Git LFS is tracking
git lfs track

# List all LFS files
git lfs ls-files

# Check LFS status
git lfs status

# Check dataset sizes
git lfs ls-files --size
```

### Verify LFS Setup
```bash
# Check if LFS is installed
git lfs version

# Check if LFS is initialized
git lfs env
```

---

## 🔄 Making Changes After Initial Push

### Add New Files
```bash
git add .
git commit -m "Description of changes"
git push
```

### Update Datasets
```bash
git add datasets/
git commit -m "Update datasets"
git push
```

### Check What Changed
```bash
git status
git diff
```

---

## 🆘 Troubleshooting Commands

### If Push Fails
```bash
# Pull first, then push
git pull origin main --rebase
git push
```

### If LFS Files Not Uploading
```bash
# Reinstall LFS hooks
git lfs install --force

# Re-track files
git lfs track "*.mat"
git lfs track "*.csv"

# Add and commit .gitattributes
git add .gitattributes
git commit -m "Fix LFS tracking"
git push
```

### Check GitHub LFS Quota
```bash
# Visit: https://github.com/settings/billing
# Or check locally:
git lfs env
```

### Reset if Something Goes Wrong
```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes) - CAREFUL!
git reset --hard HEAD~1

# Remove file from staging
git reset HEAD filename
```

---

## 📊 Useful Git Commands

### View History
```bash
# See commit history
git log

# See compact history
git log --oneline

# See what changed in last commit
git show
```

### Branch Management
```bash
# Create new branch
git checkout -b feature-name

# Switch branches
git checkout main

# List branches
git branch
```

### Remote Management
```bash
# View remote URL
git remote -v

# Change remote URL
git remote set-url origin NEW_URL
```

---

## 🎯 Pre-Presentation Checklist Commands

```bash
# 1. Check everything is committed
git status

# 2. Check remote is set
git remote -v

# 3. Verify LFS files
git lfs ls-files

# 4. Test clone (in different folder)
cd ..
git lfs clone https://github.com/YOUR_USERNAME/battery-management-system.git test-clone
cd test-clone
# Verify datasets are there
ls datasets/
```

---

## 📝 Common Workflows

### Daily Development
```bash
# Start of day
git pull

# Make changes...

# End of day
git add .
git commit -m "Describe what you did"
git push
```

### Before Presentation
```bash
# Make sure everything is pushed
git status
git push

# Verify on GitHub
# Open: https://github.com/YOUR_USERNAME/battery-management-system
```

### After Feedback
```bash
# Make improvements...
git add .
git commit -m "Implement teacher feedback"
git push
```

---

## 🔍 Check Repository Size

```bash
# Check local repository size
du -sh .git

# Check LFS storage usage (on GitHub)
# Visit: https://github.com/settings/billing

# See largest files
git lfs ls-files --size | sort -k2 -n -r | head -10
```

---

## 💡 Pro Tips

### Commit Message Best Practices
```bash
# Good commit messages
git commit -m "Add hybrid LSTM model with ECM features"
git commit -m "Fix dataset path in battery_loader.py"
git commit -m "Update README with installation instructions"

# Bad commit messages (avoid these)
git commit -m "update"
git commit -m "fix"
git commit -m "changes"
```

### Selective Staging
```bash
# Add specific files only
git add README.md requirements.txt
git commit -m "Update documentation"

# Add by pattern
git add *.py
git commit -m "Update Python scripts"

# Add directory
git add hybrid/
git commit -m "Update hybrid models"
```

---

## 🚨 Emergency Commands

### Undo Last Commit (Keep Changes)
```bash
git reset --soft HEAD~1
```

### Discard All Local Changes
```bash
# CAREFUL: This deletes your changes!
git reset --hard HEAD
```

### Remove File from Git (Keep Locally)
```bash
git rm --cached filename
echo "filename" >> .gitignore
git commit -m "Remove filename from tracking"
```

### Fix Wrong Commit Message
```bash
# If not pushed yet
git commit --amend -m "Correct message"

# If already pushed (avoid if possible)
git commit --amend -m "Correct message"
git push --force
```

---

## 📱 Quick Reference Card

| Task | Command |
|------|---------|
| Check status | `git status` |
| Add all files | `git add .` |
| Commit | `git commit -m "message"` |
| Push | `git push` |
| Pull | `git pull` |
| View history | `git log --oneline` |
| Check LFS | `git lfs ls-files` |
| Clone with LFS | `git lfs clone URL` |

---

## 🔗 Important URLs

- **Your Repository:** `https://github.com/YOUR_USERNAME/battery-management-system`
- **Git LFS:** https://git-lfs.github.com/
- **GitHub Billing:** https://github.com/settings/billing
- **Git Documentation:** https://git-scm.com/doc

---

## ✅ Final Push Checklist

```bash
# Run these in order:
git status                    # ✓ Nothing to commit?
git lfs ls-files             # ✓ LFS files tracked?
git remote -v                # ✓ Remote set correctly?
git push                     # ✓ Push successful?
# Open GitHub in browser     # ✓ Files visible online?
```

---

**Save this file for quick reference! 📌**
