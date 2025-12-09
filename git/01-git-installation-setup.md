# Installing and Configuring Git

Getting Git installed and properly configured is the first step to using version control.

## Installing Git

### Windows

#### Method 1: Using the Official Installer (Recommended)

1. **Download Git**
   - Visit [git-scm.com](https://git-scm.com)
   - Click "Download for Windows"
   - The installer will automatically detect your Windows version (32-bit or 64-bit)

2. **Run the Installer**
   - Open the downloaded `.exe` file
   - Click "Next" through the installation wizard
   - Keep default settings unless you have specific preferences
   - **Important options**:
     - "Git from the command line and also from 3rd-party software" (recommended)
     - "Use Windows' default console window" or "Use MinTTY" (both work)
   - Click "Install" and wait for completion

3. **Verify Installation**
   - Open Command Prompt or PowerShell
   - Type: `git --version`
   - You should see the Git version number (e.g., `git version 2.42.0.windows.2`)

#### Method 2: Using Chocolatey (If Already Installed)

```bash
choco install git
```

#### Method 3: Using Windows Package Manager

```bash
winget install Git.Git
```

#### Method 4: Using Scoop (If Already Installed)

```bash
scoop install git
```

### macOS

#### Method 1: Using Homebrew (Recommended)

1. **Install Homebrew** (if not already installed)
   - Open Terminal
   - Paste: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
   - Press Enter and follow the prompts

2. **Install Git**
   - In Terminal, type: `brew install git`

3. **Verify Installation**
   - Type: `git --version`
   - You should see the Git version number

#### Method 2: Using the Official Installer

1. Download the macOS installer from [git-scm.com](https://git-scm.com)
2. Run the `.dmg` file
3. Follow the installation wizard
4. Verify: Open Terminal and type `git --version`

#### Method 3: Using MacPorts

```bash
sudo port install git
```

#### Command Line Tools (Xcode)

macOS may prompt you to install Xcode Command Line Tools. If so:
```bash
xcode-select --install
```

### Linux

#### Ubuntu/Debian-Based Systems

1. **Update Package Manager**
   ```bash
   sudo apt update
   sudo apt upgrade
   ```

2. **Install Git**
   ```bash
   sudo apt install git
   ```

3. **Verify Installation**
   ```bash
   git --version
   ```

#### Fedora/RHEL/CentOS

1. **Update Package Manager**
   ```bash
   sudo dnf update
   ```

2. **Install Git**
   ```bash
   sudo dnf install git
   ```

3. **Verify Installation**
   ```bash
   git --version
   ```

#### Arch Linux

```bash
sudo pacman -S git
```

## Configuring Local Git

After installing Git, configure your identity so that your commits are properly attributed.

### Setting Global Configuration

These settings will apply to all repositories on your machine.

#### Set Your Name

```bash
git config --global user.name "Your Full Name"
```

Example:
```bash
git config --global user.name "John Doe"
```

#### Set Your Email

```bash
git config --global user.email "your_email@example.com"
```

Example:
```bash
git config --global user.email "john.doe@example.com"
```

### Setting Repository-Specific Configuration

To use different credentials for a specific repository, navigate to that repository and use the same commands without the `--global` flag:

```bash
cd /path/to/your/repository
git config user.name "Your Name"
git config user.email "your_email@example.com"
```

### Viewing Configuration

#### View All Configuration

```bash
git config --global --list
```

#### View Specific Setting

```bash
git config --global user.name
git config --global user.email
```

#### View Repository-Specific Configuration

```bash
cd /path/to/your/repository
git config --list
```

### Other Useful Global Configuration

#### Set Default Branch Name

```bash
git config --global init.defaultBranch main
```

#### Set Default Text Editor

**For VS Code**:
```bash
git config --global core.editor "code --wait"
```

**For Nano**:
```bash
git config --global core.editor "nano"
```

**For Vim**:
```bash
git config --global core.editor "vim"
```

#### Configure Line Endings (Recommended for Cross-Platform Work)

```bash
git config --global core.autocrlf true
```

(macOS/Linux should use `false` instead of `true`)
