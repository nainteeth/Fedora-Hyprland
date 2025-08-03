### To Add a New Folder

1.  **Move the new folder** into your `Fedora-Hyprland` repository:
    ```bash
    mv ~/.config/new-app ~/Fedora-Hyprland/home/.config/
    ```
2.  **Navigate to the repository** and run `stow`:
    ```bash
    cd ~/Fedora-Hyprland
    stow home
    ```

---

### To Remove a Folder

1.  **Navigate to the repository** and unstow the folders:
    ```bash
    cd ~/Fedora-Hyprland
    stow -D home
    ```
2.  **Remove the folder** you no longer need:
    ```bash
    rm -rf ~/Fedora-Hyprland/home/.config/old-app
    ```
3.  **Restow the remaining folders**:
    ```bash
    stow home
    ```
