# Neovim Cheat Sheet for QuickShell Development

## Basic Navigation

### Moving Around
```
h j k l                     # Left, Down, Up, Right (basic movement)
w b                         # Next word, Previous word
0 $                         # Start of line, End of line
gg G                        # Top of file, Bottom of file
Ctrl+u Ctrl+d               # Page up, Page down
:{number}                   # Go to line number (e.g., :42)
```

### Modes
```
i                           # Insert mode (start typing)
a                           # Append (insert after cursor)
o O                         # Open new line below/above
v                           # Visual mode (select text)
V                           # Visual line mode (select lines)
Esc                         # Back to normal mode
```

## File Operations

### Basic File Commands
```
:w                          # Save file
:q                          # Quit
:wq                         # Save and quit
:q!                         # Quit without saving
Ctrl+s                      # Save (from our config)
```

### File Navigation (with our config)
```
Space+e                     # Open file explorer
Space+t                     # Toggle file tree (NvimTree)
:e filename.qml             # Open/edit file
:find *.qml                 # Find QML files
```

## Editing

### Basic Editing
```
x                           # Delete character
dd                          # Delete line
yy                          # Copy line
p P                         # Paste after/before cursor
u                           # Undo
Ctrl+r                      # Redo
r{char}                     # Replace character
```

### Advanced Editing
```
ci"                         # Change inside quotes
ci{                         # Change inside braces {}
di(                         # Delete inside parentheses
yi[                         # Yank (copy) inside brackets
```

## Search and Replace
```
/{pattern}                  # Search forward
?{pattern}                  # Search backward
n N                         # Next/previous search result
*                           # Search word under cursor
:%s/old/new/g               # Replace all 'old' with 'new'
:%s/old/new/gc              # Replace with confirmation
```

## LSP Features (Language Server)

### Code Navigation
```
gd                          # Go to definition
K                           # Show hover information/documentation
Space+vrr                   # Find references
Space+vrn                   # Rename symbol
Space+vca                   # Code actions
```

### Diagnostics (Error checking)
```
Space+vd                    # Show diagnostic popup
[d ]d                       # Previous/next diagnostic
:lua vim.diagnostic.open_float()  # Show error details
```

## Completion and Snippets
```
Ctrl+Space                  # Trigger completion manually
Tab                         # Select next completion
Shift+Tab                   # Select previous completion
Enter                       # Accept completion
Ctrl+e                      # Close completion menu
```

## Window Management
```
:split                      # Horizontal split
:vsplit                     # Vertical split
Ctrl+w h j k l              # Move between windows
Ctrl+w =                    # Make windows equal size
Ctrl+w q                    # Close current window
```

## Plugin Commands

### Lazy Plugin Manager
```
:Lazy                       # Open plugin manager
:Lazy update                # Update plugins
:Lazy clean                 # Remove unused plugins
```

### File Tree (NvimTree)
```
Space+t                     # Toggle file tree
o                           # Open file/folder (in tree)
a                           # Create new file (in tree)
d                           # Delete file (in tree)
r                           # Rename file (in tree)
```

### Tree-sitter
```
:TSInstall qmljs            # Install QML syntax highlighting
:TSUpdate                   # Update parsers
:TSInstallInfo              # Show installed parsers
```

## QuickShell Specific

### Working with QML Files
```
# Our config automatically:
# - Highlights QML syntax
# - Provides autocomplete for QML properties
# - Shows errors and warnings
# - Enables go-to-definition for QML components
```

### Project Setup
```
# In your QuickShell project directory:
touch .qmlls.ini            # Enable LSP support
nvim shell.qml              # Start editing your config
```

### Common QML Patterns to Remember
```qml
import Quickshell            # Always needed
import QtQuick 2.15          # For basic QML items

ShellRoot {                  # Main QuickShell component
    // Your shell config
}

PanelWindow {                # For panels/bars
    // Panel configuration
}

Rectangle {                  # Basic visual element
    width: 100
    height: 50
    color: "blue"
}
```

## Useful Commands for Development

### Terminal inside Neovim
```
:terminal                   # Open terminal in split
:term quickshell -p ~/.config/quickshell  # Test your config
```

### Git Integration (if you add git plugins later)
```
:Git status                 # Git status
:Git add %                  # Add current file
:Git commit                 # Commit changes
```

## Troubleshooting

### If LSP isn't working
```
:LspInfo                    # Check LSP status
:checkhealth                # Check Neovim health
:Mason                      # Open Mason (LSP installer)
```

### If syntax highlighting is broken
```
:TSInstall qmljs            # Reinstall QML parser
:syntax on                  # Enable syntax highlighting
```

### If completion isn't working
```
:CmpStatus                  # Check completion status
Ctrl+Space                  # Manually trigger completion
```

## Quick Development Workflow

1. **Open project:** `nvim ~/.config/quickshell/shell.qml`
2. **Edit code:** Use `i` to enter insert mode
3. **Save:** `Ctrl+s` or `:w`
4. **Test:** `:term quickshell -p ~/.config/quickshell`
5. **Navigate:** `gd` for definitions, `K` for docs
6. **Fix errors:** `Space+vd` to see diagnostics

## Remember
- `Space` is your leader key for custom shortcuts
- `Esc` gets you back to normal mode
- `:` starts command mode
- `u` is undo, your best friend!
- When in doubt, `:help {topic}` for built-in help