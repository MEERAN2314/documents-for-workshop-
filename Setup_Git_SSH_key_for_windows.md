# Setting Up SSH for Git on Windows

## Why Set Up SSH?

- **No repeated logins:** Once set up, you can push, pull, and clone without typing your username or password/token every time.
- **More secure:** SSH uses a public/private key pair. Your private key never leaves your computer, and nothing sensitive is sent over the network.
- **No expiring tokens:** Unlike HTTPS personal access tokens, SSH keys don't expire unless you remove them.
- **Easy to manage:** You can add a separate key per device and revoke any single key from GitHub without affecting the others.
- **Industry standard:** Most developers and teams use SSH for Git access.

---

## Prerequisites

- A Windows PC
- A GitHub account

---

## Step 1: Install Git Bash

1. Download Git for Windows: https://git-scm.com/install/windows
2. Run the installer and keep the default options.

## Step 2: Generate the SSH Key

1. Open **Git Bash**.
2. Run the command below (replace the email with your GitHub email):

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

3. When asked **"Enter file in which to save the key"**, press **Enter** to accept the default location.
4. When asked for a passphrase, either enter one (recommended) or press **Enter** to skip.
5. **Note the file path** shown. It is usually:

```
C:\Users\<your-username>\.ssh\id_ed25519
```

## Step 3: Start the ssh-agent

1. Open **Windows PowerShell as Administrator** (right-click > *Run as administrator*).
2. Run:

```powershell
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
```

## Step 4: Add Your Key to the ssh-agent

In the same PowerShell window, run (replace `<your-username>` with your Windows username):

```powershell
ssh-add C:\Users\<your-username>\.ssh\id_ed25519
```

This registers your identity on the device.

## Step 5: Copy Your Public Key

1. Open File Explorer and go to `C:\Users\<your-username>\.ssh\`
2. Open the **`id_ed25519.pub`** file in a text editor (Notepad works).
3. Copy the **entire contents** of the file.

> **Tip:** Only share the `.pub` file. Never share the file without the `.pub` extension. That is your private key.

Alternatively, copy it straight to the clipboard from PowerShell:

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```

## Step 6: Add the Key to GitHub

1. Log in to GitHub.
2. Go to **Profile picture > Settings > SSH and GPG keys**.
3. Click **New SSH key**.
4. Enter a **Title** (e.g., `My Windows Laptop`).
5. Paste the copied key into the **Key** field.
6. Click **Add SSH key**.

## Step 7: Verify the Connection

In PowerShell (or Git Bash), run:

```bash
ssh -T git@github.com
```

- If asked *"Are you sure you want to continue connecting?"*, type `yes` and press Enter.
- A successful setup shows a message like:

```
Hi <your-username>! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `Permission denied (publickey)` | Make sure the key was added with `ssh-add` and the correct `.pub` key is on GitHub. |
| `ssh-agent` won't start | Re-run PowerShell as **Administrator** and repeat Step 3. |
| Can't find the `.ssh` folder | Enable *View > Show > Hidden items* in File Explorer. |
| Existing repo still asks for password | Switch the remote to SSH: `git remote set-url origin git@github.com:<user>/<repo>.git` |

---

## Quick Command Summary

```bash
# 1. Generate key (Git Bash)
ssh-keygen -t ed25519 -C "your_email@example.com"

# 2. Start agent (PowerShell as Admin)
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent

# 3. Add key
ssh-add C:\Users\<your-username>\.ssh\id_ed25519

# 4. Test connection
ssh -T git@github.com
```
