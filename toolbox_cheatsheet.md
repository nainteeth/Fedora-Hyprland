# Toolbox Cheat Sheet

## What is Toolbox?
A tool for creating isolated development environments on immutable Linux systems. Think of it as a lightweight container that shares your home directory.

## Basic Commands

### Container Management
```bash
# Create a new toolbox (one-time setup)
toolbox create                          # Creates default toolbox
toolbox create my-dev                   # Creates named toolbox
toolbox create quickshell-dev           # Create for QuickShell development

# List all toolboxes
toolbox list

# Enter a toolbox (this is where you work)
toolbox enter                           # Enter default toolbox
toolbox enter quickshell-dev            # Enter named toolbox

# Exit toolbox (go back to host)
exit                                    # Or Ctrl+D

# Remove a toolbox
toolbox rm my-dev                       # Remove named toolbox
toolbox rm --all                        # Remove all toolboxes
```

### Daily Workflow
```bash
# 1. Enter your development environment
toolbox enter quickshell-dev

# 2. Work normally (install packages, run commands, etc.)
sudo dnf install whatever
nvim myfile.qml
quickshell -p ~/.config/quickshell

# 3. Exit when done
exit
```

## Inside the Toolbox

### Package Management
```bash
# Install packages (works like normal Fedora)
sudo dnf install neovim git nodejs
sudo dnf search package-name
sudo dnf remove package-name
sudo dnf update

# Add COPR repositories
sudo dnf copr enable some/repo
```

### Key Points
- **Home directory is shared** - `~` is the same inside and outside
- **Can install anything** - full dnf access, no restrictions
- **Persistent** - changes survive reboots and system updates
- **Isolated** - doesn't affect host system
- **Network access** - can download, git clone, etc.

## Useful Tips

### Check where you are
```bash
# See if you're in toolbox or host
echo $TOOLBOX_PATH                      # Shows path if in toolbox
hostname                                # Shows toolbox name if inside
```

### Quick development session
```bash
# One-liner to enter and run commands
toolbox run --container quickshell-dev nvim ~/.config/quickshell/shell.qml
```

### File access
```bash
# These work the same inside and outside toolbox:
ls ~/.config/nvim                       # Your configs
cd ~/.dotfiles                          # Your dotfiles
git status                              # Git repos
```

## Common Gotchas

❌ **Don't do this:**
- Try to install GUI apps that need system integration
- Expect systemd services to work the same way
- Use for production services

✅ **Perfect for:**
- Development tools (nvim, git, compilers)
- Language runtimes (node, python, etc.)
- Build tools (cmake, ninja, etc.)
- CLI applications

## QuickShell Specific Workflow

```bash
# 1. Enter development environment
toolbox enter quickshell-dev

# 2. Edit config (your nvim config works here!)
nvim ~/.config/quickshell/shell.qml

# 3. Test your config
quickshell -p ~/.config/quickshell

# 4. Debug if needed
quickshell -p ~/.config/quickshell --verbose

# 5. Exit when done
exit
```

## Emergency Commands

```bash
# If toolbox won't start
podman ps -a                            # See container status
podman start toolbox-quickshell-dev     # Start manually

# Reset a broken toolbox
toolbox rm quickshell-dev
toolbox create quickshell-dev
# Then reinstall your packages

# See what's using space
toolbox list
podman system df                         # Check container disk usage
```

## Remember
- Toolbox = your development space
- Host = your clean, immutable system
- Home directory is always shared
- Exit with `exit` or Ctrl+D
- Your dotfiles and configs work everywhere!