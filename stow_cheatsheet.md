# GNU Stow Cheat Sheet

## What is GNU Stow?
A symlink farm manager that helps organize configuration files by creating symbolic links from a single directory structure to their target locations.

## Basic Concepts

### Directory Structure
```
~/Fedora-Hyprland/          # Your dotfiles repository
├── home/                   # Stow package
│   ├── .config/
│   │   ├── nvim/
│   │   ├── quickshell/
│   │   └── git/
│   └── .bashrc
└── scripts/                # Another stow package
    └── bin/
        └── my-script.sh
```

### How Stow Works
- **Package**: A directory containing files to be symlinked (e.g., `home/`)
- **Target**: Where symlinks will be created (usually `~`)
- **Stow directory**: Where packages are stored (e.g., `~/Fedora-Hyprland`)

## Basic Commands

### Stowing (Creating Symlinks)
```bash
# Navigate to your dotfiles directory
cd ~/Fedora-Hyprland

# Stow a package (creates symlinks)
stow home                   # Links everything in home/ to ~/

# Stow specific package to custom target
stow -t /usr/local scripts  # Links scripts/ to /usr/local

# Dry run (see what would happen)
stow -n home               # Shows what would be linked without doing it

# Verbose output
stow -v home               # Shows what's being linked
```

### Unstowing (Removing Symlinks)
```bash
# Remove symlinks for a package
stow -D home               # Removes all symlinks created by 'home' package

# Remove and recreate (useful after changes)
stow -R home               # Same as: stow -D home && stow home
```

### Common Options
```bash
stow -v home               # Verbose: show what's being done
stow -n home               # Dry run: simulate without doing
stow -t ~ home             # Target: specify target directory (~ is default)
stow --ignore='.DS_Store' home  # Ignore specific files/patterns
```

## Managing Your Dotfiles

### Initial Setup
```bash
# Create dotfiles repository
mkdir ~/Fedora-Hyprland
cd ~/Fedora-Hyprland
git init

# Create package structure
mkdir -p home/.config
```

### Adding New Configuration

#### Method 1: Move Existing Config
```bash
# Move existing config to dotfiles repo
mv ~/.config/new-app ~/Fedora-Hyprland/home/.config/

# Navigate to repository and stow
cd ~/Fedora-Hyprland
stow home

# Now ~/.config/new-app is a symlink to ~/Fedora-Hyprland/home/.config/new-app
```

#### Method 2: Create New Config
```bash
# Create config in dotfiles repo
mkdir -p ~/Fedora-Hyprland/home/.config/new-app
nvim ~/Fedora-Hyprland/home/.config/new-app/config.yml

# Stow to create symlink
cd ~/Fedora-Hyprland
stow home
```

### Removing Configuration
```bash
# Navigate to repository and unstow
cd ~/Fedora-Hyprland
stow -D home

# Remove the folder you no longer need
rm -rf ~/Fedora-Hyprland/home/.config/old-app

# Restow the remaining folders
stow home
```

### Updating Configuration
```bash
# Edit config files (they're already linked)
nvim ~/.config/nvim/init.lua

# Commit changes
cd ~/Fedora-Hyprland
git add .
git commit -m "Update nvim config"
git push
```

## Advanced Usage

### Multiple Packages
```bash
# Stow multiple packages at once
stow home scripts fonts

# Different packages for different purposes
stow desktop-apps          # GUI application configs
stow terminal-tools        # CLI tool configs
stow development           # Development environment
```

### Conditional Stowing
```bash
# Stow only specific subdirectories
stow --ignore='unwanted-*' home

# Stow with custom ignore patterns
stow --ignore='*.log' --ignore='cache' home
```

### Cross-Platform Management
```bash
# Different packages for different systems
stow linux-specific       # Linux-only configs
stow macos-specific       # macOS-only configs
stow universal            # Cross-platform configs
```

## Troubleshooting

### Common Issues

#### Conflict Errors
```bash
# Error: existing file blocks stow
# Solution: Move or remove conflicting file
mv ~/.config/nvim ~/.config/nvim.backup
stow home
```

#### Broken Symlinks
```bash
# Check for broken symlinks
find ~ -maxdepth 3 -type l -exec test ! -e {} \; -print

# Remove broken symlinks and restow
stow -D home
stow home
```

#### Permission Issues
```bash
# Make sure you own the target directory
sudo chown -R $USER:$USER ~/.config

# Then restow
cd ~/Fedora-Hyprland
stow -R home
```

### Debugging Commands
```bash
# See what stow would do without doing it
stow -n -v home

# Check current symlinks
ls -la ~/.config/

# Find all symlinks pointing to your dotfiles
find ~ -type l -lname "*Fedora-Hyprland*"
```

## Best Practices

### Directory Organization
```bash
# Good structure
~/Fedora-Hyprland/
├── home/                  # Main configs
│   ├── .config/
│   ├── .bashrc
│   └── .gitconfig
├── scripts/               # Custom scripts
├── fonts/                 # Custom fonts
└── systemd/               # Service files
```

### Git Integration
```bash
# Initialize git repo
cd ~/Fedora-Hyprland
git init
git add .
git commit -m "Initial dotfiles commit"

# Add to remote repository
git remote add origin <your-repo-url>
git push -u origin main
```

### Ignore Files
Create `.stowrc` in your dotfiles directory:
```bash
# ~/.stowrc
--ignore='\.DS_Store'
--ignore='\.git'
--ignore='README.*'
--ignore='LICENSE'
```

## Quick Workflow

### Daily Usage
```bash
# Edit configs (files are already symlinked)
nvim ~/.config/nvim/init.lua

# Commit changes
cd ~/Fedora-Hyprland
git add .
git commit -m "Update configuration"
git push
```

### New Machine Setup
```bash
# Clone dotfiles
git clone <your-repo> ~/Fedora-Hyprland

# Deploy all configs
cd ~/Fedora-Hyprland
stow home

# Done! All your configs are now symlinked
```

### Adding New App Config
```bash
# Move config to dotfiles
mv ~/.config/new-app ~/Fedora-Hyprland/home/.config/

# Restow
cd ~/Fedora-Hyprland
stow -R home

# Commit
git add .
git commit -m "Add new-app configuration"
```

## Remember
- Always work from your dotfiles directory when stowing
- Use `-n` flag to test before making changes
- Symlinks point FROM target TO source
- Your configs live in the dotfiles repo, not in their final locations
- `stow -R` is your friend when you change the structure