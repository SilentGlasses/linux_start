# Installing Python on Linux

This guide walks you through installing Python on a Linux system using the Homebrew package manager and explains how to create and manage isolated project environments using Python's built-in `venv` module.

**Why Homebrew on Linux (Linuxbrew)?**

While most Linux distributions have their own package managers (like `apt` for Debian/Ubuntu, `dnf`/`yum` for Fedora/CentOS), Homebrew (often called Linuxbrew when on Linux) offers some advantages:

- **User-space Installation:** Installs packages in your home directory, avoiding the need for `sudo` for package management after the initial Homebrew install.
- **Up-to-date Packages:** Often provides newer versions of software than might be available in default distribution repositories.
- **Consistent Environment:** Useful if you also work on macOS, as it uses the same commands.

## Why Virtual Environments (`.venv`)?

Python projects often depend on specific versions of external libraries. Installing these globally can lead to conflicts between projects requiring different versions of the same library. Virtual environments solve this by:

- Creating isolated environments for each project.
- Allowing you to install project-specific dependencies without affecting the global Python installation or other projects.
- Making projects self-contained and reproducible.


## Prerequisites

- A Linux system.
- Basic familiarity with the Linux command line (terminal).
- `curl`, `git`, and `build-essential` (or equivalent development tools for your distribution) installed. You can usually install these using your system's package manager:
    - **Debian/Ubuntu:** `sudo apt update && sudo apt install build-essential curl file git`
    - **Fedora/CentOS/RHEL:** `sudo dnf groupinstall "Development Tools" && sudo dnf install curl file git`

## Install Homebrew (Linuxbrew)

1.  **Download and Run the Installer:** Open your terminal and run the official installation script from the Homebrew website (`brew.sh`).
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
!!! note
    - Always check the official [Homebrew website](https://brew.sh/) for the most current installation command, as it might change.
    - The script will explain what it will do and prompt you for your password (for `sudo`) at certain points during the installation.
2.  **Add Homebrew to your PATH:** After the installation completes, the script will likely output instructions to add Homebrew to your system's PATH. This usually involves running a couple of commands similar to these (the exact commands will be provided by the installer output – **use those!**):
```bash
# Example commands - Follow the instructions from *your* installation!
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```
    - You might need to add this to `.bashrc`, `.zshrc`, or another shell configuration file depending on your shell and setup. The installer's output is the best guide.
    - Close and reopen your terminal or run either  `source ~/.bashrc` or `source ~/.zshrc` (or the relevant config file) for the changes to take effect.
3.  **Verify Installation:** Check that Homebrew is installed correctly.
```bash
brew --version
brew doctor
```
`brew doctor` will check for potential issues with your setup. Address any warnings if necessary.

## Install Python using Homebrew

1.  **Update Homebrew:** Ensure Homebrew has the latest package information.
```bash
brew update
```
2.  **Install Python:** Use Homebrew to install the latest stable version of Python 3.
```bash
brew install python
```
    - Homebrew typically installs Python as `python3`.
3.  **Verify Python Installation:** Check the installed Python version.
```bash
python3 --version
```
- This should output the version you just installed via Homebrew. You can also check its location:
```bash
which python3
```
    - This should point to a path within your Homebrew installation directory (e.g., `/home/linuxbrew/.linuxbrew/bin/python3` or similar).

!!! important
    Your system might still have a default Python installed (accessible via `python` or an older `python3`). Using `python3` after installing with Homebrew ensures you're using the Homebrew-managed version.

## Creating and Using a Virtual Environment

Now that you have Python installed, let's create an isolated environment for a project.

1.  **Navigate to Your Project Directory:** Create a directory for your new project (if it doesn't exist) and change into it.
```bash
mkdir my-python-project
cd my-python-project
```
2.  **Create the Virtual Environment:** Use the `venv` module included with Python 3. It's standard practice to name the environment directory `.venv`. The leading dot (`.`) makes it a hidden directory on Linux/macOS.
```bash
python3 -m venv .venv
```
    - This command creates a `.venv` directory containing a copy of the Python interpreter, the `pip` package manager, and other necessary files.
3.  **Activate the Virtual Environment:** Before you can install packages into or run scripts using the virtual environment, you need to *activate* it.
```bash
source .venv/bin/activate
```
    * Once activated, your shell prompt will usually change to indicate the active environment, for example:
```bash
(.venv) user@hostname:~/my-python-project$
```
    * Now, commands like `python` and `pip` will refer to the versions inside the `.venv` directory, not the global ones.
4.  **Install Packages:** With the environment active, you can install packages using `pip`. They will be installed *only* within this environment.
```bash
# Example: Install the 'requests' library
pip install requests

# Verify installation
pip list
```
    - `pip list` will show `requests` and its dependencies installed, but only when the `.venv` is active.
5.  **Work on Your Project:** Create your Python scripts (e.g., `main.py`) within the `my-python-project` directory. When you run `python main.py`, it will use the Python interpreter and libraries from the active `.venv`.
6.  **Deactivate the Virtual Environment:** When you're finished working on the project, you can deactivate the environment.
```bash
deactivate
```
    - Your shell prompt will return to normal.

## Best Practices

=== ".gitignore"
    Add your virtual environment directory (`.venv/`) to your project's `.gitignore` file. This prevents committing the large environment directory and its installed packages (which can be platform-specific) to your version control system (like Git).

    ```
    # .gitignore
    .venv/
    __pycache__/
    *.pyc
    ```
=== "requirements.txt"
    To share your project's dependencies with others or to easily recreate the environment, generate a `requirements.txt` file:

    -  While the virtual environment is **active**:
    ```bash
    pip freeze > requirements.txt
    ```
    -  Commit this `requirements.txt` file to your Git repository.
    -  Someone else (or you, in a new clone of the project) can recreate the environment by:
        - Creating a new virtual environment: `python3 -m venv .venv`
        - Activating it: `source .venv/bin/activate`
        - Installing dependencies: `pip install -r requirements.txt`

You've now learned how to install a modern version of Python on Linux using Homebrew and how to effectively manage project dependencies using `venv`. Using virtual environments is a fundamental practice in Python development that keeps your projects organized and avoids dependency conflicts. Happy coding!
