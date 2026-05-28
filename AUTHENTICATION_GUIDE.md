# 🔐 GitHub Authentication Guide

## Issue
You're getting a "Permission denied" error because Git is trying to use a different GitHub account (`divyanshthakur5552`) instead of yours (`srishtisrivastava-0`).

---

## ✅ Solution: Use Personal Access Token (PAT)

### Step 1: Create a Personal Access Token

1. **Go to GitHub Settings:**
   - Visit: https://github.com/settings/tokens
   - Or: GitHub → Click your profile picture → Settings → Developer settings → Personal access tokens → Tokens (classic)

2. **Generate New Token:**
   - Click "Generate new token (classic)"
   - Give it a name: "Battery Project Upload"
   - Set expiration: 90 days (or your preference)
   
3. **Select Scopes:**
   - ✅ Check **`repo`** (Full control of private repositories)
   - This gives access to push code

4. **Generate and Copy:**
   - Click "Generate token" at the bottom
   - **IMPORTANT:** Copy the token immediately (you won't see it again!)
   - It looks like: `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

---

### Step 2: Push Using the Token

When you run `git push`, Git will ask for your credentials:

```bash
git push -u origin master
```

**When prompted:**
- **Username:** `srishtisrivastava-0`
- **Password:** Paste your Personal Access Token (not your GitHub password!)

---

## 🚀 Quick Commands

```bash
# Push to GitHub (will prompt for credentials)
git push -u origin master

# When prompted:
# Username: srishtisrivastava-0
# Password: [paste your token here]
```

---

## 🔧 Alternative: Configure Git Credentials

### Option A: Store Credentials (Recommended for Windows)

```bash
# Tell Git to remember credentials
git config --global credential.helper wincred

# Then push (enter token once, it will be saved)
git push -u origin master
```

### Option B: Use Token in URL (Quick but less secure)

```bash
# Remove current remote
git remote remove origin

# Add remote with token in URL
git remote add origin https://YOUR_TOKEN@github.com/srishtisrivastava-0/Battery-Performance-Modeling-combing-AI-with-Physics-Based-Models.git

# Push
git push -u origin master
```

**Replace `YOUR_TOKEN`** with your actual token!

---

## 🔍 Verify Your GitHub Account

```bash
# Check current Git user
git config user.name
git config user.email

# Set to your GitHub account if needed
git config --global user.name "srishtisrivastava-0"
git config --global user.email "your-email@example.com"
```

---

## ⚠️ Important Notes

### About the Token:
- ✅ Treat it like a password - keep it secret!
- ✅ Don't share it or commit it to Git
- ✅ You can revoke it anytime from GitHub settings
- ✅ Create a new one if you lose it

### About the Upload:
- 📊 Total size: 1.2 GB
- ⏱️ Upload time: 15-30 minutes (depending on internet speed)
- 📶 Make sure you have stable internet
- 💾 GitHub Free: 1 GB LFS storage (you're slightly over, may need upgrade)

---

## 📋 Complete Workflow

1. **Create Personal Access Token** (see Step 1 above)

2. **Copy the token** to clipboard

3. **Run push command:**
   ```bash
   git push -u origin master
   ```

4. **Enter credentials when prompted:**
   - Username: `srishtisrivastava-0`
   - Password: [paste token]

5. **Wait for upload** (15-30 minutes)

6. **Verify on GitHub:**
   - Visit: https://github.com/srishtisrivastava-0/Battery-Performance-Modeling-combing-AI-with-Physics-Based-Models
   - Check that files are there

---

## 🆘 Troubleshooting

### "Permission denied" error
**Solution:** Make sure you're using YOUR GitHub username and token, not someone else's

### "Authentication failed"
**Solution:** 
- Verify token is correct
- Check token hasn't expired
- Ensure token has `repo` scope

### "This exceeds GitHub's file size limit"
**Solution:** 
- Make sure Git LFS is installed: `git lfs version`
- Verify LFS is tracking files: `git lfs track`
- Check .gitattributes exists

### Upload is very slow
**Solution:** 
- This is normal for 1.2 GB
- Keep terminal open
- Don't interrupt the upload
- Can resume if interrupted

### "Quota exceeded" error
**Solution:**
- You may need GitHub Pro ($4/month) for more LFS storage
- Or use selective upload (only essential datasets)
- Or use external storage (Google Drive) for datasets

---

## 🎯 After Successful Push

Once upload completes:

1. **Verify on GitHub:**
   ```
   https://github.com/srishtisrivastava-0/Battery-Performance-Modeling-combing-AI-with-Physics-Based-Models
   ```

2. **Check datasets are there:**
   - Navigate to `datasets/` folder
   - Click on a `.mat` or `.csv` file
   - Should see "Stored with Git LFS" badge

3. **Share with teacher:**
   - Send the repository URL
   - They can clone with: `git lfs clone [URL]`

---

## 💡 Pro Tips

### Save Credentials (Windows)
```bash
git config --global credential.helper wincred
```
This saves your token so you don't have to enter it every time.

### Check Upload Progress
Git will show progress like:
```
Uploading LFS objects: 100% (7822/7822), 1.2 GB | 5.2 MB/s
```

### If Upload Fails Midway
Just run `git push` again - Git LFS will resume from where it stopped!

---

## ✅ Quick Checklist

Before pushing:
- [ ] Personal Access Token created
- [ ] Token copied to clipboard
- [ ] Stable internet connection
- [ ] 30-60 minutes available for upload
- [ ] Git LFS installed and working

During push:
- [ ] Entered correct username: `srishtisrivastava-0`
- [ ] Pasted token as password
- [ ] Keeping terminal open
- [ ] Not interrupting upload

After push:
- [ ] Verified repository on GitHub
- [ ] Checked datasets are uploaded
- [ ] Tested clone in different folder
- [ ] Shared link with teacher

---

**Good luck with your upload! 🚀**

**Need help?** Check the error message and refer to the troubleshooting section above.
