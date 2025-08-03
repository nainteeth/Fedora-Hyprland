# Fedora Kinoite Setup Guide for QuickShell Development

Complete setup instructions for deploying your QuickShell + Neovim development environment on Fedora Kinoite.

## Prerequisites

- Fresh Fedora Kinoite installation
- Internet connection
- Your dotfiles repository accessible (USB drive, git repo, etc.)

## Step 1: Create Development Toolbox

```bash
# Create and enter development container
toolbox create quickshell-dev
toolbox enter quickshell-dev
```

## Step 2: Install Development Packages

```bash
# Inside the toolbox, install all required packages
sudo dnf install -y \
    neovim \
    git \
    qt6-qtdeclarative-devel \
    qt6-qtsvg-devel \
    qt6-qtimageformats \
    qt6-qtmultimedia-devel \
    qt5-qtgraphicaleffects \
    cmake \
    ninja-build \
    gcc-c++ \
    pkg-config \
    stow

# Add QuickShell COPR and install
sudo dnf copr enable errornointernet/quickshell
sudo dnf install quickshell

# Exit toolbox
exit
```

## Step 3: Deploy Your Dotfiles

```bash
# Clone or copy your dotfiles repository
git clone <your-dotfiles-repo-url> ~/Fedora-Hyprland
# OR copy from USB/external drive
# cp -r /path/to/your/dotfiles ~/Fedora-Hyprland

# Navigate to dotfiles directory
cd ~/Fedora-Hyprland

# Deploy your configurations using stow
stow home  # This will stow all configs in the home folder

# Verify symlinks were created
ls -la ~/.config/nvim        # Should show symlink
ls -la ~/.config/quickshell  # Should show symlink (if you have it)
```

## Step 4: Set Up Neovim

```bash
# Enter toolbox for development
toolbox enter quickshell-dev

# Start Neovim (plugins will auto-install)
nvim

# Wait for Lazy plugin manager to install everything
# You'll see installation progress, wait for completion
# Press 'q' to quit the Lazy interface when done

# Install Tree-sitter QML parser
:TSInstall qmljs
```

## Step 5: Test Your Setup

```bash
# Still in toolbox, create a test QuickShell project
mkdir ~/test-quickshell
cd ~/test-quickshell

# Create required files
touch shell.qml
touch .qmlls.ini

# Test Neovim with QML
nvim shell.qml
```

Add this test content to verify everything works:
```qml
import Quickshell

ShellRoot {
    Rectangle {
        width: 200
        height: 100
        color: "blue"
    }
}
```

```bash
# Test QuickShell
quickshell -p ~/test-quickshell

# If it works, clean up
cd ~
rm -rf ~/test-quickshell
```

## Step 6: Set Up Your Real QuickShell Config

```bash
# Navigate to your QuickShell config (should be symlinked)
cd ~/.config/quickshell

# Edit your shell configuration
nvim shell.qml

# Test your actual config
quickshell -p ~/.config/quickshell
```

## Step 7: Optional Host System Setup

If you want to run QuickShell from the host system (without toolbox):

```bash
# Exit toolbox first
exit

# Install QuickShell on host (requires reboot)
sudo rpm-ostree install qt6-qtdeclarative qt6-qtsvg qt6-qtimageformats
curl -s https://copr.fedorainfracloud.org/coprs/errornointernet/quickshell/repo/fedora-$(rpm -E %fedora)/errornointernet-quickshell-fedora-$(rpm -E %fedora).repo | sudo tee /etc/yum.repos.d/quickshell.repo
sudo rpm-ostree install quickshell

# Reboot to apply changes
sudo systemctl reboot
```

After reboot, you can run QuickShell directly from host:
```bash
quickshell -p ~/.config/quickshell
```

## Daily Workflow

### Development Session
```bash
# Enter development environment
toolbox enter quickshell-dev

# Edit your config
nvim ~/.config/quickshell/shell.qml

# Test changes
quickshell -p ~/.config/quickshell

# Exit when done
exit
```

### Adding New Configs
```bash
# Add new application config to dotfiles
mv ~/.config/new-app ~/Fedora-Hyprland/home/.config/

# Update stow
cd ~/Fedora-Hyprland
stow -R home  # Restow to include new config

# Commit changes
git add .
git commit -m "Add new-app configuration"
git push
```

## Troubleshooting

### If toolbox won't start:
```bash
podman ps -a
podman start toolbox-quickshell-dev
```

### If Neovim plugins don't work:
```bash
toolbox enter quickshell-dev
nvim
:Lazy restore
:TSUpdate
```

### If symlinks are broken:
```bash
cd ~/Fedora-Hyprland
stow -D home  # Remove symlinks
stow home     # Recreate symlinks
```

### If QuickShell can't find config:
```bash
# Check if symlink exists
ls -la ~/.config/quickshell

# Check if .qmlls.ini exists
ls -la ~/.config/quickshell/.qmlls.ini

# Create if missing
touch ~/.config/quickshell/.qmlls.ini
```

## Verification Checklist

- [ ] Toolbox created and packages installed
- [ ] Dotfiles repository cloned/copied
- [ ] Stow successfully created symlinks
- [ ] Neovim opens without errors
- [ ] LSP works in QML files (syntax highlighting, completion)
- [ ] QuickShell runs with your config
- [ ] File tree works in Neovim (`Space+t`)
- [ ] Can save files (`Ctrl+s`)

## Quick Reference Commands

```bash
# Enter development environment
toolbox enter quickshell-dev

# Edit QuickShell config
nvim ~/.config/quickshell/shell.qml

# Test QuickShell config
quickshell -p ~/.config/quickshell

# Update dotfiles
cd ~/Fedora-Hyprland && git pull

# Restow after changes
cd ~/Fedora-Hyprland && stow -R home

# Exit development environment
exit
```

Your Fedora Kinoite system is now ready for QuickShell development with your familiar configuration!