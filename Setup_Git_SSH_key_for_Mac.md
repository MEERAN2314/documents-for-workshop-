# Setting Up SSH for Git on Mac

## Why Set Up SSH?

- **No repeated logins:** Push, pull, and clone without typing your username or token every time.
- **More secure:** SSH uses a public/private key pair. Your private key never leaves your computer.
- **No expiring tokens:** Unlike HTTPS personal access tokens, SSH keys don't expire unless you remove them.
- **Easy to manage:** Add a separate key per device and revoke any one of them from GitHub.
- **Industry standard:** Most developers and teams use SSH for Git access.

---

## Prerequisites

- A Mac with Terminal (built in: press **Cmd + Space**, type **Terminal**, press Enter)
- A GitHub account

---

## Step 1: Check Whether Git Is Installed

Open Terminal and run:

```bash
git --version
```

- **If Git is installed**, you'll see something like `git version 2.45.0`. Skip to Step 2.
- **If Git is not installed**, macOS will show a popup asking to install the **Command Line Tools**. Click **Install** and wait for it to finish.

If no popup appears, install Git manually with either option below:

```bash
xcode-select --install
```

Or, if you use Homebrew:

```bash
brew install git
```

After installing, run `git --version` again to confirm it works.

## Step 2: Generate the SSH Key

Run the command below (replace the email with your GitHub email):

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

This creates a new SSH key, using your email as a label.

## Step 3: Save the Key and Choose a Passphrase

- When asked **"Enter file in which to save the key"**, press **Enter** to accept the default location.
- When asked for a passphrase, type one (recommended) or press **Enter** to skip.

## Step 4: Copy the Public Key

```bash
cat ~/.ssh/id_ed25519.pub | pbcopy
```

This copies the contents of your `.pub` file to the clipboard.

> **Tip:** Only share the `.pub` file. The file without `.pub` is your private key. Never share it.

## Step 5: Add the Key to GitHub

1. Log in to GitHub.
2. Go to **Profile picture > Settings > SSH and GPG keys**.
3. Click **New SSH key**.
4. Enter a **Title** (e.g., `My MacBook`).
5. Paste the key into the **Key** field.
6. Click **Add SSH key**.

## Step 6: Test the Connection

```bash
ssh -T git@github.com
```

- If asked *"Are you sure you want to continue connecting?"*, type `yes` and press Enter.
- Success looks like:

```
Hi <your-username>! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Quick Command Summary

```bash
git --version
xcode-select --install        # only if Git is missing
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub | pbcopy
ssh -T git@github.com
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `git: command not found` | Git isn't installed. Follow Step 1. |
| `Permission denied (publickey)` | Check that the correct `.pub` key was pasted into GitHub. |
| Existing repo still asks for a password | Switch the remote to SSH: `git remote set-url origin git@github.com:<user>/<repo>.git` |
| Asked for the passphrase every time | Run `ssh-add --apple-use-keychain ~/.ssh/id_ed25519` once to store it in your Keychain. |

---

**Reference:** [GitHub Docs: Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
