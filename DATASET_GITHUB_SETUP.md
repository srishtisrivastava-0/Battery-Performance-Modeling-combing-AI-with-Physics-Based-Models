# 📦 Adding Datasets to GitHub with Git LFS

## Overview

Your datasets folder is **1.2 GB** with 7,822 files. This requires **Git LFS (Large File Storage)** to upload to GitHub.

### Dataset Breakdown
| Folder | Size | Files | Description |
|--------|------|-------|-------------|
| Dataset_Li-ion | 610.65 MB | 211 | Li-ion battery data |
| archive | 558.87 MB | 7,575 | NASA cleaned dataset (CSV) |
| battery_data | 37.99 MB | 34 | NASA .mat files |
| rul_dataset | 1.09 MB | 1 | RUL dataset |
| archive (1) | 0.10 MB | 1 | Battery dataset CSV |
| **TOTAL** | **1,208.71 MB** | **7,822** | |

---

## 🎯 Recommended Approach: Git LFS

Git LFS is designed for large files and is the best solution for your datasets.

### Why Git LFS?
- ✅ Handles large files efficiently
- ✅ Keeps repository fast
- ✅ Supported by GitHub
- ✅ Free tier: 1 GB storage + 1 GB bandwidth/month
- ✅ Your datasets (1.2 GB) fit within limits

---

## 📋 Step-by-Step Setup

### Step 1: Install Git LFS

#### Windows
```bash
# Download from: https://git-lfs.github.com/
# Or use chocolatey
choco install git-lfs

# Or use scoop
scoop install git-lfs
```

#### Mac
```bash
brew install git-lfs
```

#### Linux
```bash
sudo apt-get install git-lfs
```

#### Verify Installation
```bash
git lfs version
# Should output: git-lfs/3.x.x
```

---

### Step 2: Initialize Git LFS

```bash
# Navigate to your project directory
cd "E:\AI-ML-Based-Battery-Management-System-for-EVs-main\AI-ML-Based-Battery-Management-System-for-EVs-main"

# Initialize Git LFS (one-time setup)
git lfs install
```

**Expected output:**
```
Updated git hooks.
Git LFS initialized.
```

---

### Step 3: Track Large Files with Git LFS

```bash
# Track all .mat files (MATLAB data)
git lfs track "*.mat"

# Track all .csv files in datasets
git lfs track "datasets/**/*.csv"

# Track any other large file types
git lfs track "*.h5"      # If you have Keras models
git lfs track "*.pkl"     # If you have pickle files
git lfs track "*.joblib"  # If you have joblib files

# Verify what's being tracked
git lfs track
```

This creates a `.gitattributes` file that tells Git which files to handle with LFS.

---

### Step 4: Add .gitattributes to Git

```bash
# Add the LFS configuration
git add .gitattributes

# Commit it
git commit -m "Configure Git LFS for datasets"
```

---

### Step 5: Add Your Datasets

```bash
# Add all dataset files
git add datasets/

# Check status (LFS files will show as "LFS")
git lfs status

# Commit the datasets
git commit -m "Add battery datasets (1.2GB via Git LFS)"
```

---

### Step 6: Push to GitHub

```bash
# Push everything including LFS files
git push origin main

# Git LFS will automatically upload large files
```

**Note:** This may take some time depending on your internet speed (1.2 GB upload).

---

## 🔍 Verification

### Check What's in LFS

```bash
# List all files tracked by LFS
git lfs ls-files

# Check LFS status
git lfs status
```

### Verify on GitHub

1. Go to your repository on GitHub
2. Navigate to `datasets/` folder
3. Click on a `.mat` or `.csv` file
4. You should see "Stored with Git LFS" badge

---

## 📊 Alternative Approaches

### Option 1: Selective Dataset Upload (Recommended if LFS is problematic)

Only upload essential datasets:

```bash
# Create a .gitignore entry for large datasets
echo "datasets/archive/" >> .gitignore
echo "datasets/Dataset_Li-ion/" >> .gitignore

# Only upload smaller, essential datasets
git add datasets/battery_data/
git add datasets/rul_dataset/
git add "datasets/archive (1)/"

git commit -m "Add essential datasets (40MB)"
git push
```

**Then provide download instructions in README:**

```markdown
## 📦 Full Dataset Download

Due to size constraints, full datasets are available separately:

- **NASA Cleaned Dataset (558 MB):** [Google Drive Link]
- **Li-ion Dataset (610 MB):** [Google Drive Link]

Place downloaded files in:
- `datasets/archive/` - NASA cleaned dataset
- `datasets/Dataset_Li-ion/` - Li-ion dataset
```

---

### Option 2: External Storage Links

Don't upload datasets to GitHub at all. Instead:

1. **Upload to Google Drive / Dropbox / OneDrive**
2. **Get shareable links**
3. **Add download instructions to README**

**Update .gitignore:**
```bash
# Exclude all datasets
datasets/
```

**Add to README.md:**
```markdown
## 📦 Dataset Setup

Download datasets from:
- [NASA Battery Dataset (600 MB)](https://drive.google.com/...)
- [Li-ion Dataset (610 MB)](https://drive.google.com/...)

Extract to `datasets/` folder:
```
project/
├── datasets/
│   ├── battery_data/
│   ├── archive/
│   └── Dataset_Li-ion/
```
```

---

### Option 3: Use Dataset Hosting Services

Upload to specialized dataset hosting:

1. **Kaggle Datasets** - https://www.kaggle.com/datasets
2. **Zenodo** - https://zenodo.org/ (academic, with DOI)
3. **Figshare** - https://figshare.com/
4. **Hugging Face Datasets** - https://huggingface.co/datasets

**Benefits:**
- ✅ Designed for large datasets
- ✅ Version control
- ✅ DOI for citations
- ✅ Better for academic work

---

## 🎯 My Recommendation

### For Your Teacher Presentation:

**Use Git LFS** - It's the most professional approach and shows you understand modern development practices.

### Setup Commands (Complete Workflow):

```bash
# 1. Install Git LFS (if not already installed)
git lfs install

# 2. Track dataset files
git lfs track "*.mat"
git lfs track "*.csv"
git lfs track "datasets/**/*"

# 3. Add .gitattributes
git add .gitattributes
git commit -m "Configure Git LFS for datasets"

# 4. Add datasets
git add datasets/
git commit -m "Add battery datasets (1.2GB via Git LFS)"

# 5. Push to GitHub
git push origin main
```

---

## ⚠️ Important Notes

### GitHub LFS Limits

**Free Account:**
- Storage: 1 GB
- Bandwidth: 1 GB/month

**Your Usage:**
- Storage: 1.2 GB (slightly over)
- You may need to upgrade or use selective upload

### If You Exceed Limits

**Option A:** GitHub Pro ($4/month)
- 50 GB storage
- 50 GB bandwidth/month

**Option B:** Buy LFS Data Pack ($5/month)
- +50 GB storage
- +50 GB bandwidth

**Option C:** Use selective upload (only essential datasets)

---

## 🔧 Troubleshooting

### Problem: "This exceeds GitHub's file size limit"

**Solution:** Make sure Git LFS is installed and tracking the files
```bash
git lfs install
git lfs track "*.mat"
git lfs track "*.csv"
git add .gitattributes
```

### Problem: "Git LFS bandwidth limit exceeded"

**Solution:** 
- Wait until next month (bandwidth resets)
- Upgrade to GitHub Pro
- Use external storage

### Problem: "Upload is very slow"

**Solution:**
- This is normal for 1.2 GB
- Estimated time: 10-30 minutes depending on internet speed
- Can pause and resume with `git push`

### Problem: "git lfs: command not found"

**Solution:** Install Git LFS first
```bash
# Windows
choco install git-lfs

# Mac
brew install git-lfs

# Linux
sudo apt-get install git-lfs
```

---

## 📝 Update README.md

Add this section to your README.md:

```markdown
## 📦 Dataset Information

This project uses two main datasets:

### 1. NASA Battery Dataset
- **Size:** 600 MB
- **Batteries:** 56 Li-ion batteries (B0005-B0056)
- **Format:** MATLAB .mat files + CSV
- **Location:** `datasets/battery_data/` and `datasets/archive/`
- **Source:** NASA Prognostics Data Repository

### 2. Li-ion Battery Dataset
- **Size:** 610 MB
- **Batteries:** 211 battery cycles
- **Format:** Various formats
- **Location:** `datasets/Dataset_Li-ion/`

### 3. RUL Dataset
- **Size:** 1 MB
- **Format:** CSV
- **Location:** `datasets/rul_dataset/`
- **Source:** Hawaii Natural Energy Institute

### Dataset Setup

Datasets are stored using **Git LFS**. When you clone the repository:

```bash
# Clone with LFS
git lfs clone https://github.com/yourusername/battery-management-system.git

# Or if already cloned
git lfs pull
```

**Note:** First-time download may take 10-15 minutes due to dataset size (1.2 GB).
```

---

## ✅ Final Checklist

Before pushing datasets:

- [ ] Git LFS installed (`git lfs version`)
- [ ] Git LFS initialized (`git lfs install`)
- [ ] Files tracked (`.gitattributes` created)
- [ ] .gitattributes committed
- [ ] Datasets added to git
- [ ] Datasets committed
- [ ] Ready to push
- [ ] README updated with dataset info
- [ ] Verified GitHub account has sufficient LFS quota

---

## 🚀 Quick Start (Copy-Paste Commands)

```bash
# Complete setup in one go
git lfs install
git lfs track "*.mat"
git lfs track "*.csv"
git lfs track "datasets/**/*"
git add .gitattributes
git commit -m "Configure Git LFS for datasets"
git add datasets/
git commit -m "Add battery datasets (1.2GB via Git LFS)"
git push origin main
```

---

## 📞 Need Help?

- **Git LFS Documentation:** https://git-lfs.github.com/
- **GitHub LFS Guide:** https://docs.github.com/en/repositories/working-with-files/managing-large-files
- **Check LFS quota:** https://github.com/settings/billing

---

**Good luck with your dataset upload! 🚀**
