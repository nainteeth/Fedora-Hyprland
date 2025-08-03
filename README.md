To Add a New Folder
mv ~/.config/new-app ~/Fedora-Hyprland/home/.config/
cd ~/Fedora-Hyprland
stow home

To Remove a Folder
cd ~/Fedora-Hyprland
stow -D home
rm -rf ~/Fedora-Hyprland/home/.config/old-app
stow home
