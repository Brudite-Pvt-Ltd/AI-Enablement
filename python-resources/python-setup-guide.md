# Python Installation and Setup Guide

A comprehensive guide to help students install Python, set up virtual environments, and resolve common setup issues.

---

## Table of Contents

1. [Python Installation](#python-installation)
2. [Setting Up Virtual Environments](#setting-up-virtual-environments)
3. [Configuring Python Path](#configuring-python-path)
4. [Common Issues and Solutions](#common-issues-and-solutions)

---

## Python Installation

### Windows

#### Method 1: Using the Official Installer (Recommended)

1. **Download Python**
   - Visit [python.org](https://www.python.org/downloads/)
   - Click the "Download Python" button (latest stable version)
   - The installer will automatically detect your Windows version (32-bit or 64-bit)

2. **Run the Installer**
   - Open the downloaded `.exe` file
   - **IMPORTANT**: Check the box "Add Python to PATH" at the bottom of the installer window
   - Choose "Install Now" for default settings, or "Customize installation" for more control
   - Wait for the installation to complete

3. **Verify Installation**
   - Open Command Prompt (search for `cmd`)
   - Type: `python --version`
   - You should see the Python version number (e.g., `Python 3.12.0`)

#### Method 2: Using Microsoft Store

1. Open Microsoft Store
2. Search for "Python"
3. Select the official Python release from the Python Software Foundation
4. Click "Install"
5. Verify installation by opening PowerShell and typing: `python --version`

#### Method 3: Using Chocolatey (If Already Installed)

```
choco install python
```

---

### macOS

#### Method 1: Using Homebrew (Recommended)

1. **Install Homebrew** (if not already installed)
   - Open Terminal
   - Paste: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
   - Press Enter and follow the prompts

2. **Install Python**
   - In Terminal, type: `brew install python@3.12`
   - Replace `3.12` with the latest stable version if needed

3. **Verify Installation**
   - Type: `python3 --version`
   - You should see the Python version number

#### Method 2: Using the Official Installer

1. Download the macOS installer from [python.org](https://www.python.org/downloads/)
2. Run the `.pkg` file
3. Follow the installation wizard
4. Verify: Open Terminal and type `python3 --version`

#### Method 3: Using MacPorts

```
sudo port install python3.12
```

**Note**: macOS comes with Python 2.7 pre-installed (deprecated). Always use `python3` instead of `python` to ensure you're using Python 3.

---

### Linux

#### Ubuntu/Debian-Based Systems

1. **Update Package Manager**
   ```
   sudo apt update
   sudo apt upgrade
   ```

2. **Install Python**
   ```
   sudo apt install python3 python3-pip python3-venv
   ```

3. **Verify Installation**
   ```
   python3 --version
   pip3 --version
   ```

#### Fedora/RHEL/CentOS

1. **Update Package Manager**
   ```
   sudo dnf update
   ```

2. **Install Python**
   ```
   sudo dnf install python3 python3-pip python3-venv
   ```

3. **Verify Installation**
   ```
   python3 --version
   pip3 --version
   ```

#### Arch Linux

```
sudo pacman -S python python-pip
```

**Note**: On Linux, `python` often points to Python 2.7 (if installed). Always use `python3` for Python 3 code.

---

## Setting Up Virtual Environments

A virtual environment is an isolated Python workspace that allows you to install packages specific to a project without affecting your system-wide Python installation.

### Why Use Virtual Environments?

- Isolate project dependencies
- Avoid version conflicts between projects
- Keep your global Python installation clean
- Make projects reproducible and shareable

### Creating and Activating Virtual Environments

#### Windows

1. **Create Virtual Environment**
   - Open Command Prompt
   - Navigate to your project directory: `cd path\to\your\project`
   - Create virtual environment: `python -m venv venv`

2. **Activate Virtual Environment**
   ```
   venv\Scripts\activate
   ```
   - You should see `(venv)` prefix in your terminal

3. **Install Packages** (Inside Active Virtual Environment)
   ```
   pip install package_name
   ```

4. **Deactivate Virtual Environment**
   ```
   deactivate
   ```

#### macOS and Linux

1. **Create Virtual Environment**
   ```
   cd /path/to/your/project
   python3 -m venv venv
   ```

2. **Activate Virtual Environment**
   ```
   source venv/bin/activate
   ```
   - You should see `(venv)` prefix in your terminal

3. **Install Packages** (Inside Active Virtual Environment)
   ```
   pip install package_name
   ```

4. **Deactivate Virtual Environment**
   ```
   deactivate
   ```

### Using requirements.txt for Dependency Management

1. **Generate requirements.txt** (from active virtual environment)
   ```
   pip freeze > requirements.txt
   ```

2. **Install Dependencies from requirements.txt**
   ```
   pip install -r requirements.txt
   ```

### Alternative: Using Poetry (Modern Approach)

1. **Install Poetry**
   ```
   curl -sSL https://install.python-poetry.org | python3 -
   ```

2. **Initialize a New Project**
   ```
   poetry new my_project
   cd my_project
   ```

3. **Add Dependencies**
   ```
   poetry add package_name
   ```

4. **Activate Project Environment**
   ```
   poetry shell
   ```

---

## Configuring Python Path

### Understanding Python Path

The Python path is the list of directories where Python looks for modules and packages. You can view and modify it to help Python find your custom modules.

### Viewing Python Path

#### Windows, macOS, and Linux

```python
import sys
print(sys.path)
```

Or from terminal:
```
python -c "import sys; print('\n'.join(sys.path))"
```

### Adding Directories to Python Path

#### Method 1: Temporary (Current Session Only)

```python
import sys
sys.path.append('/path/to/your/modules')
```

#### Method 2: Environment Variable (Persistent)

**Windows**
1. Right-click "This PC" or "My Computer" and select "Properties"
2. Click "Advanced system settings"
3. Click "Environment Variables"
4. Under "User variables" or "System variables", click "New"
5. Variable name: `PYTHONPATH`
6. Variable value: `C:\path\to\your\modules;C:\another\path`
7. Click "OK" and restart Command Prompt

**macOS and Linux**
1. Open Terminal
2. Edit your shell configuration file:
   - For bash: `nano ~/.bashrc`
   - For zsh: `nano ~/.zshrc`
3. Add the line:
   ```
   export PYTHONPATH="${PYTHONPATH}:/path/to/your/modules"
   ```
4. Save (Ctrl+O, Enter, Ctrl+X)
5. Apply changes: `source ~/.bashrc` or `source ~/.zshrc`

#### Method 3: Using .pth Files (Persistent)

1. Find your site-packages directory:
   ```python
   import site
   print(site.getsitepackages())
   ```

2. Create a `.pth` file in the site-packages directory:
   ```
   mymodules.pth
   ```

3. Add paths to the file (one per line):
   ```
   /path/to/your/modules
   C:\another\path\to\modules
   ```

---

## Common Issues and Solutions

### Issue 1: "python: command not found" or "python is not recognized"

**Cause**: Python is not installed or not in the system PATH.

**Solutions**:
- **Windows**: Reinstall Python and ensure "Add Python to PATH" is checked
- **macOS**: Verify Python is installed with `python3 --version`. Add alias:
  ```
  echo "alias python=python3" >> ~/.zshrc
  source ~/.zshrc
  ```
- **Linux**: Install Python: `sudo apt install python3` (Ubuntu/Debian)

---

### Issue 2: "pip: command not found" or "pip is not recognized"

**Cause**: pip is not installed or not in the PATH.

**Solutions**:
- **Windows**: 
  ```
  python -m pip --version
  python -m pip install package_name
  ```
- **macOS/Linux**:
  ```
  python3 -m pip --version
  sudo apt install python3-pip
  ```

---

### Issue 3: Virtual Environment Not Activating

**Cause**: Incorrect path or the venv folder doesn't exist.

**Solutions**:
- **Windows**:
  - Check if `venv` folder exists in your project directory
  - Use full path: `C:\full\path\to\project\venv\Scripts\activate`
  - For PowerShell: `venv\Scripts\Activate.ps1` (may require execution policy change)
    ```
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    ```

- **macOS/Linux**:
  - Check if `venv` folder exists: `ls -la`
  - Ensure you're in the correct directory: `pwd`
  - Try: `source ./venv/bin/activate`

---

### Issue 4: "No module named 'package_name'"

**Cause**: Package is not installed or installed in a different Python environment.

**Solutions**:
- Verify virtual environment is activated (should see `(venv)` prefix)
- Install the package: `pip install package_name`
- Check installed packages: `pip list`
- Verify correct Python interpreter: `which python` (macOS/Linux) or `where python` (Windows)

---

### Issue 5: Permission Denied When Installing Packages

**Cause**: Attempting to install packages globally without proper permissions.

**Solutions**:
- **Always use virtual environments** (recommended approach)
- **Linux/macOS**: Use `sudo` cautiously:
  ```
  sudo pip install package_name
  ```
- **Better approach**: Install with user flag:
  ```
  pip install --user package_name
  ```

---

### Issue 6: Multiple Python Versions Installed

**Cause**: System has multiple Python installations, and the wrong one is being used.

**Solutions**:
- **Windows**:
  - Use specific version: `python3.12` or `py -3.12`
  - Check all versions: `py --list-paths`

- **macOS/Linux**:
  - List installed versions: `ls /usr/bin/python*` or `ls /usr/local/bin/python*`
  - Use specific version: `python3.12` instead of `python3`
  - Set preferred version in virtual environment creation:
    ```
    python3.12 -m venv venv
    ```

---

### Issue 7: "pip install" Downloads But Doesn't Extract

**Cause**: Incomplete download or corrupted cache.

**Solutions**:
- Clear pip cache:
  ```
  pip cache purge
  ```
- Install with no cache:
  ```
  pip install --no-cache-dir package_name
  ```

---

### Issue 8: Path Issues in Virtual Environment

**Cause**: Incorrect Python path configuration.

**Solutions**:
- Verify Python location inside virtual environment:
  ```
  which python  # macOS/Linux
  where python  # Windows
  ```
- Check sys.path:
  ```python
  python -c "import sys; print(sys.path)"
  ```
- Reinstall virtual environment if corrupted:
  ```
  rm -rf venv  # macOS/Linux
  rmdir /s venv  # Windows
  python -m venv venv
  ```

---

### Issue 9: ModuleNotFoundError After Installing Package

**Cause**: Package installed with different Python version or in wrong environment.

**Solutions**:
- Verify virtual environment activation
- Verify Python version: `python --version`
- Reinstall package: `pip uninstall package_name && pip install package_name`
- Check for typos in import statement (Python is case-sensitive)

---

### Issue 10: Terminal Not Recognizing Changes After Environment Variable Update

**Cause**: Terminal needs to reload configuration.

**Solutions**:
- **Windows**: Close and reopen Command Prompt or PowerShell
- **macOS/Linux**: 
  ```
  source ~/.bashrc  # for bash
  source ~/.zshrc   # for zsh
  ```
- Or restart the terminal application

---

## Best Practices

1. **Always Use Virtual Environments**: Never install packages globally
2. **Version Control**: Keep `requirements.txt` in your repository
3. **Documentation**: Include setup instructions in your project's README
4. **Python Version**: Specify the Python version used in your project
5. **Regular Updates**: Keep Python and packages updated for security
6. **Clean Projects**: Remove virtual environment folder before sharing projects
7. **Consistent Naming**: Use conventional names like `venv`, `env`, or `.venv` for virtual environments

---

## Additional Resources

- [Official Python Documentation](https://docs.python.org/3/)
- [Python Virtual Environments Guide](https://docs.python.org/3/library/venv.html)
- [pip Documentation](https://pip.pypa.io/en/stable/)
- [Python Package Index (PyPI)](https://pypi.org/)

---

**Last Updated**: December 2025

For questions or additional issues, refer to the official Python documentation or community forums like Stack Overflow.