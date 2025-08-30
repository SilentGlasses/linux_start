# Linux Onboarding Guide

This document provides step-by-step instructions for setting up a Linux workstation with essential developer tools and customizations, tailored for both **Debian-based** and **Fedora-based** systems.

## System Update

Always start with a full system update.

=== "Debian-based"
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
=== "Fedora-based"
    ```bash
    sudo dnf update -y
    ```

## Install Homebrew (Linuxbrew)

Homebrew is a popular package manager. It's optional but very useful.

Install dependencies:

- `build-essential` – Installs compilers and tools for building software (like `gcc`, `make`).
- `procps` – Provides system utilities like `ps`, `top`, `kill`, etc.
- `curl` – Transfers data from or to a server using URL syntax (`HTTP`, `FTP`, etc).
- `file` – Detects file types based on content, not extension.
- `git` – Distributed version control system for tracking code changes.

=== "Debian-based"
    ```bash
    sudo apt install build-essential procps curl file git -y
    ```
=== "Fedora-based"
    ```bash
    sudo dnf groupinstall "Development Tools" -y && sudo dnf install procps-ng curl file git -y
    ```
=== "Install Homebrew"
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
    - Add Homebrew to your shell profile (this will be prompted at the end of the install):
    ```bash
    echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
    eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
    ```
    - Test it:
    ```bash
    brew doctor
    ```

## Install Zsh

Zsh is a more powerful alternative to bash.

=== "Debian-based"
    ```bash
    sudo apt install zsh -y
    ```
=== "Fedora-based"
    ```bash
    sudo dnf install zsh -y
    ```

Then change your default shell:

```bash
chsh -s $(which zsh)
```

Log out and back in to use Zsh.

## Install Oh My Zsh

A framework for managing your Zsh configuration.

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Install Powerlevel10k Theme

Powerlevel10k is a fast, stylish Zsh theme.

- Clone the theme:
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
- Set the theme in your `~/.zshrc`:
```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```
- Then reload Zsh:
```bash
source ~/.zshrc
```
- When prompted, go through the Powerlevel10k configuration wizard.

## Optional Nice-to-Haves

Here are some other useful tools and utilities you might want:

- `bat` – A `cat` clone with syntax highlighting and Git integration.
- `exa` – A modern replacement for `ls` with more features and colors.
- `fzf` – A fast fuzzy finder for searching files, history, etc.
- `ripgrep` – A super-fast recursive search tool, like `grep` but better.
- `neovim` – A modern, extensible version of the Vim text editor.

=== "Debian-based"
    ```bash
    sudo apt install bat exa fzf ripgrep neovim -y
    ```
=== "Fedora-based"
    ```
    sudo dnf install bat exa fzf ripgrep neovim -y
    ```
=== "Via Homebrew"
    - works the same on both distros
    ```bash
    brew install bat exa fzf ripgrep neovim
    ```

## Reboot (optional)

Some changes (like default shell) take effect cleanly after a reboot:

```bash
sudo reboot
```

You're now set up with a modern, powerful Linux terminal environment!
