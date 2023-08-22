# Hyprland installation guide

## Archlinux (in virt-manager KVM)

- Install archlinux with *minimal* profile (additional packages: `git`)
- Start VM with following configuration changes:
  - Enable 3D acceleration in `Video VirtIO` category
  - Enable `OpenGL` in `Display Spice` category with `Listen Type: None`
- Install `yay` (AUR helper): 
  `$ git clone --depth 1 https://aur.archlinux.org/yay-bin.git && cd yay-bin && makepkg -si`
- Install hyprland and other apps (optional):
  `$ yay -S hyprland hyprpaper kitty dolphin`
  - `hyprpaper`: Wallpapar manager for hyprland
  - `kitty`: Terminal emulator
  - `dolphin`: File manager (`nautilus` also available)
- Prepare a startup script for hyprland:
  ```
  #!/bin/bash
  
  export WLR_NO_HARDWARE_CURSORS=1
  export WLR_RENDERER_ALLOW_SOFTWARE=1

  exec Hyprland
  ```
- Exceute the script and you are into Hyprland. (`SUPER+M` to exit to console)
