# Setting Up SSH Keys for GitHub

SSH keys provide a secure way to authenticate with GitHub without storing passwords. This is the recommended authentication method.

## Why Use SSH Keys?

- More secure than username/password authentication
- No need to enter credentials repeatedly
- Required for some GitHub operations
- Industry standard for secure authentication

## Generating SSH Keys

### Windows, macOS, and Linux

1. **Open Terminal/Command Prompt**
   - Windows: Open PowerShell or Git Bash
   - macOS/Linux: Open Terminal

2. **Generate SSH Key**
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
   
   **Note**: Replace `your_email@example.com` with your GitHub email address
   
   **For older systems** (if Ed25519 is not supported):
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

3. **Save Key Location**
   - When prompted "Enter a file in which to save the key", press Enter to accept the default location
   - Default: `~/.ssh/id_ed25519` (macOS/Linux) or `C:\Users\YourUsername\.ssh\id_ed25519` (Windows)

4. **Set Passphrase** (Optional but Recommended)
   - When prompted "Enter passphrase", you can either:
     - Press Enter to skip (no passphrase)
     - Type a passphrase for extra security
   - Confirm the passphrase

5. **Verify Key Generation**
   - Two files should be created:
     - `id_ed25519` (private key - keep this secret!)
     - `id_ed25519.pub` (public key - share this with GitHub)
   - Check: `ls -la ~/.ssh/` (macOS/Linux) or `dir C:\Users\YourUsername\.ssh\` (Windows)

## Adding SSH Key to GitHub

### Step 1: Copy Your Public Key

**macOS/Linux**:
```bash
cat ~/.ssh/id_ed25519.pub
```

**Windows (PowerShell)**:
```bash
type C:\Users\YourUsername\.ssh\id_ed25519.pub
```

**Windows (Git Bash)**:
```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output (starts with `ssh-ed25519` and ends with your email).

### Step 2: Add Key to GitHub

1. Go to [GitHub Settings](https://github.com/settings/keys) (or GitHub → Settings → SSH and GPG keys)
2. Click "New SSH key"
3. **Title**: Enter a descriptive name (e.g., "My Laptop", "Work Machine")
4. **Key type**: Select "Authentication Key"
5. **Key**: Paste your public key (the content from `id_ed25519.pub`)
6. Click "Add SSH key"

### Step 3: Test SSH Connection

```bash
ssh -T git@github.com
```

**Expected output**:
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

**Note**: If you set a passphrase, you may be prompted to enter it. You can configure SSH Agent to remember it (see below).

## SSH Agent Configuration (Optional but Recommended)

Using SSH Agent allows you to enter your passphrase once per session instead of repeatedly.

### macOS and Linux

1. **Start SSH Agent**
   ```bash
   eval "$(ssh-agent -s)"
   ```

2. **Add Your Key**
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```
   - You'll be prompted for your passphrase (if you set one)

3. **Make SSH Agent Start Automatically**
   
   For **macOS**:
   ```bash
   nano ~/.zshrc
   ```
   Add these lines:
   ```bash
   if [ -z "$SSH_AUTH_SOCK" ]; then
     eval "$(ssh-agent -s)"
     ssh-add ~/.ssh/id_ed25519
   fi
   ```
   
   For **Linux (bash)**:
   ```bash
   nano ~/.bashrc
   ```
   Add the same lines above

   Save (Ctrl+O, Enter, Ctrl+X) and reload: `source ~/.bashrc` or `source ~/.zshrc`

### Windows (PowerShell)

1. **Start SSH Agent Service**
   ```bash
   Start-Service ssh-agent
   ```

2. **Add Your Key**
   ```bash
   ssh-add C:\Users\YourUsername\.ssh\id_ed25519
   ```

3. **Set SSH Agent to Start Automatically**
   ```bash
   Get-Service -Name ssh-agent | Set-Service -StartupType Automatic
   ```

### Windows (Git Bash)

1. **Start SSH Agent**
   ```bash
   eval "$(ssh-agent -s)"
   ```

2. **Add Your Key**
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

3. **Make SSH Agent Start Automatically**
   ```bash
   nano ~/.bashrc
   ```
   Add:
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519 2>/dev/null
   ```
   Save and reload

## SSH Key Troubleshooting

### Issue: "Permission Denied (publickey)"

**Solutions**:
1. Verify SSH key exists: `ls -la ~/.ssh/`
2. Test SSH connection: `ssh -T git@github.com`
3. Check SSH key permissions:
   ```bash
   chmod 600 ~/.ssh/id_ed25519
   chmod 644 ~/.ssh/id_ed25519.pub
   ```
4. Verify key is added to SSH Agent:
   ```bash
   ssh-add -l
   ```
   If not listed, add it: `ssh-add ~/.ssh/id_ed25519`

### Issue: "Could not open a connection to your authentication agent"

**Windows Solution**:
```bash
eval $(ssh-agent -s)
```

**macOS/Linux Solution**:
```bash
eval "$(ssh-agent -s)"
```

### Issue: Multiple SSH Keys

If you have multiple SSH keys, create an SSH config file:

**File: `~/.ssh/config`**
```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
```

## SSH vs HTTPS Comparison

| Feature | SSH | HTTPS |
|---------|-----|-------|
| Security | Very secure | Secure |
| Password | SSH key | Personal access token |
| Credentials | Once per session | Every time |
| Firewall issues | Rare | Sometimes blocked |
| Recommended | Yes | No |
