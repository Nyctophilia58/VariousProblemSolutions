# PostgreSQL Setup Guide

This guide provides step-by-step commands to install PostgreSQL on Linux (Arch-based or similar), initialize it, create users and databases, and prepare it.

---

## Table of Contents

- [Step 1: Install PostgreSQL](#step-1-install-postgresql)
  - [Solution 1: Verify SSH Fingerprint](#solution-1-verify-ssh-fingerprint)
  - [Solution 2: Check Remote URL and SSH Key](#solution-2-check-remote-url-and-ssh-key)
- [Step 2: Switch to Your Branch](#step-2-switch-to-your-branch)
- [Step 3: Remove a File from Git History](#step-3-remove-a-file-from-git-history)
- [Step 4: Verify Commit Removal](#step-4-verify-commit-removal)
- [Step 5: Add the File to .gitignore](#step-5-add-the-file-to-gitignore)
- [Step 6: Verify on GitHub](#step-6-verify-on-github)
- [Note](#note)

---

## Step 1: Pull from GitHub

On Arch Linux:

```bash
sudo pacman -Syu postgresql
```

On Ubuntu/Debian:

```
sudo apt update
sudo apt install postgresql postgresql-contrib
```

### Solution 1: Verify SSH Fingerprint

1. Check that the fingerprint matches [GitHub’s official SSH key fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints).
2. If it matches, type `yes` and press **Enter**. You should see:

```
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
```

If you still see:

```
git@github.com: Permission denied (publickey)
fatal: Could not read from remote repository.
Please make sure you have the correct access rights and the repository exists.
```

Move to Solution 2.

---

### Solution 2: Check Remote URL and SSH Key

1. Check your remote URL:

```bash
git remote -v
```

It should show:

```
origin  git@github.com:username/snadders.git (fetch)
origin  git@github.com:username/snadders.git (push)
```

2. Test your SSH key:

```bash
ssh -T git@github.com
```

- Success message:

```
Hi <your-username>! You've successfully authenticated, but GitHub does not provide shell access.
```

- If you get `Permission denied (publickey)`, fix your SSH key:

```bash
# Generate a new SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Start the SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copy the public key
cat ~/.ssh/id_ed25519.pub
```

3. Add the public key to GitHub:

- Go to [GitHub → Settings → SSH and GPG keys → New SSH key](https://github.com/settings/keys)
- Title: `MyPC`
- Paste your copied key → **Save**

4. Test again:

```bash
ssh -T git@github.com
```

You should now see:

```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Step 2: Switch to Your Branch

If you’re not on a branch (detached HEAD), run:

```bash
git checkout main
```

---

## Step 3: Remove a File from Git History

```bash
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE" --prune-empty --tag-name-filter cat -- --all
```

Replace `PATH-TO-YOUR-FILE` with the file path you want to remove.

---

## Step 4: Verify Commit Removal

```bash
git log
```

Check your Git history to confirm the file has been removed.

---

## Step 5: Add the File to .gitignore

```bash
echo "PATH-TO-YOUR-FILE" >> .gitignore
git add .gitignore
git commit -m "Adding gitignore file"
git push origin --force --all
```

This prevents the file from being tracked in future commits.

---

## Step 6: Verify on GitHub

Check your GitHub repository commits to ensure the file is removed from history.

---

## Note

- Be careful with `--force` pushes as they **overwrite remote history**.  
- Always double-check the SSH fingerprint before accepting a new host.  
- Use `git revert` if you want to safely undo a commit **without rewriting history**.
