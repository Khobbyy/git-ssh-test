#  Git SSH Setup Guide (Mac) — Complete Workflow

This guide walks you through setting up Git with SSH authentication and reusing it for all future projects — including making your setup **persist across restarts**.

---

#  1. Set Your Global Git Identity (One-Time Setup)

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Verify:

```bash
git config --global --list
```

---

#  2. Generate an SSH Key (One-Time Setup)

Check if you already have one:

```bash
ls -al ~/.ssh
```

If you don’t see `id_ed25519`, create one:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

### During prompts:

* Press **Enter** to accept default file location
* Enter a **passphrase** (recommended)

---

# ⚙️ 3. Start SSH Agent & Add Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

#  4. Store Passphrase in macOS Keychain (Important)

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

 This ensures you don’t re-enter your passphrase repeatedly.

---

# 5. Make It Persist Across Restarts (Very Important)

Create or edit your SSH config file:

```bash
nano ~/.ssh/config
```

Add the following:

```bash
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Save:

* Press `CTRL + X`
* Press `Y`
* Press `Enter`

###  What this does:

* Automatically loads your SSH key on startup
* Uses macOS Keychain to unlock it
* Prevents repeated passphrase prompts after reboot

---

#  6. Copy Public Key

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

---

#  7. Add SSH Key to GitHub (One-Time Setup)

1. Go to GitHub → Settings
2. Navigate to **SSH and GPG keys**
3. Click **New SSH key**
4. Paste your key
5. Save

---

# 8. Test SSH Connection

```bash
ssh -T git@github.com
```

Expected:

```
Hi username! You've successfully authenticated...
```

---

# 9. Create and Push a Test Project

## Create project

```bash
mkdir git-ssh-test
cd git-ssh-test
echo "Hello Git SSH " > hello.txt
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

# 10. Connect to GitHub Using SSH

```bash
git remote add origin git@github.com:your-username/git-ssh-test.git
```

---

# 11. Push Code

```bash
git branch -M main
git push -u origin main
```

---

# 12. Verify Remote Uses SSH

```bash
git remote -v
```

Should show:

```
git@github.com:your-username/repo.git
```

---

# ♻️ Using SSH for Future Projects

Once SSH is set up, you **do NOT repeat setup again**.

---

##  For a NEW project:

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

```bash
git remote -v
git remote set-url origin git@github.com:your-username/repo.git
```

---

## 📥 For CLONING repos (use SSH)

```bash
git clone git@github.com:your-username/repo.git
```

---

#  Troubleshooting

## ❌ Permission denied (publickey)

```bash
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

Check:

* SSH key added to GitHub
* SSH agent running

---

## ❌ Still asking for passphrase repeatedly

Ensure:

* You ran:

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

* Your config file contains:

```bash
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

---

# Key Concepts

* **Public key** → stored on GitHub
* **Private key** → stays on your Mac
* **SSH** → enables secure, password-free authentication
* **Keychain** → remembers your passphrase securely

---

# Final Result

After setup:

* No username/password prompts
* No repeated passphrase prompts (even after restart)
* Seamless Git workflow

---

#  Summary

You only do this ONCE:

* Generate SSH key
* Add to GitHub
* Add to Keychain
* Configure SSH config for persistence

After that:

* Use SSH (`git@github.com:...`)
* Push normally
* Everything works automatically

---

Happy coding 
