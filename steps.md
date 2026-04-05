# Git SSH Setup Guide (Mac) — Complete Workflow

This guide walks you through setting up Git with SSH authentication and reusing it for all future projects.

---

# 1. Set Your Global Git Identity (One-Time Setup)

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Verify:

```bash
git config --global --list
```

---

# 2. Generate an SSH Key (One-Time Setup)

Check if you already have one:

```bash
ls -al ~/.ssh
```

If you don’t see `id_ed25519`, create one:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

### أثناء prompts:

* Press **Enter** to accept default file location
* Enter a **passphrase** (recommended)

---

# 3. Start SSH Agent & Add Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

(Optional — persist in macOS Keychain):

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

---

# 4. Copy Public Key

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

---

# 🌐 5. Add SSH Key to GitHub (One-Time Setup)

1. Go to GitHub → Settings
2. Navigate to **SSH and GPG keys**
3. Click **New SSH key**
4. Paste your key
5. Save

---

# 🧪 6. Test SSH Connection

```bash
ssh -T git@github.com
```

Expected:

```
Hi username! You've successfully authenticated...
```

---

# 📁 7. Create and Push a Test Project

## Create project

```bash
mkdir git-ssh-test
cd git-ssh-test
echo "Hello Git SSH 🚀" > hello.txt
```

## Initialize Git

```bash
git init
```

## Commit

```bash
git add .
git commit -m "Initial commit"
```

---

# 🔗 8. Connect to GitHub Using SSH

Create a repo on GitHub, then run:

```bash
git remote add origin git@github.com:your-username/git-ssh-test.git
```

---

# 🚀 9. Push Code

```bash
git branch -M main
git push -u origin main
```

---

# 🔍 10. Verify Remote Uses SSH

```bash
git remote -v
```

Should show:

```
git@github.com:your-username/repo.git
```

---

# ♻️ Using SSH for Future Projects

Once SSH is set up, **you do NOT repeat the setup**. Just follow:

## For a NEW project:

```bash
mkdir project-name
cd project-name
git init
git add .
git commit -m "Initial commit"
git remote add origin git@github.com:your-username/project-name.git
git push -u origin main
```

---

## For an EXISTING repo (switch HTTPS → SSH)

Check remote:

```bash
git remote -v
```

If it shows HTTPS:

```bash
git remote set-url origin git@github.com:your-username/repo.git
```

---

## For CLONING repos (use SSH)

```bash
git clone git@github.com:your-username/repo.git
```

---

# ⚠️ Troubleshooting

## ❌ Permission denied (publickey)

Fix:

```bash
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

Check:

* SSH key added to GitHub
* SSH agent running

---

## ❌ Still asking for password

Cause:

* Remote is using HTTPS instead of SSH

Fix:

```bash
git remote set-url origin git@github.com:your-username/repo.git
```

---

# 🧠 Key Concepts

* **Public key** → stored on GitHub
* **Private key** → stays on your Mac
* **SSH** → allows secure, password-free authentication

---

# ✅ Final Result

After setup:

* No username/password prompts
* Only passphrase (once, if using Keychain)
* Faster and secure workflow

---

# 🎯 Summary

You only do this ONCE:

* Generate SSH key
* Add to GitHub
* Configure SSH agent

After that, every repo:

* Use SSH remote (`git@github.com:...`)
* Push normally

---
