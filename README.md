# My Personal Terminal Setup for Ubuntu/GNOME

This guide documents the steps to set up my preferred terminal environment on a fresh Debian-based system (like Ubuntu) with the GNOME Desktop Environment.

<img width="920" height="604" alt="Screenshot from 2025-08-17 20-19-44" src="https://github.com/user-attachments/assets/366cf8c2-bd8a-40dc-ab0e-7fa63f8771f0" />

## Installation Steps

Follow these steps in order to replicate the setup.

### 1. Install Fish Shell

First, we install the Fish shell, which offers a more user-friendly experience out of the box.

```bash
sudo apt update
sudo apt install fish -y
```

### 2. Change Default Shell to Fish

After installation, set Fish as the default shell for your user.

```bash
chsh -s /usr/bin/fish
```
> **Note:** You must log out and log back in for this change to take effect.

### 3. Install Fira Code Nerd Font

Nerd Fonts include icons that are essential for a modern prompt like Starship.

```bash
# Create a local font directory if it doesn't exist
mkdir -p ~/.local/share/fonts

# Download the FiraCode Nerd Font and extract it to the fonts directory
wget [https://github.com/ryanoasis/nerd-fonts/releases/download/v3.2.1/FiraCode.zip](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.2.1/FiraCode.zip) -P ~/Downloads
unzip ~/Downloads/FiraCode.zip -d ~/.local/share/fonts/FiraCodeNerdFont
rm ~/Downloads/FiraCode.zip

# Update the font cache
fc-cache -f -v
```

**IMPORTANT:** After installing the font, you must **manually set it in your terminal's preferences**.
Go to `Terminal > Preferences > Your Profile > Custom Font` and select `FiraCode Nerd Font`.

### 4. Install Starship Prompt

Starship is a minimal, blazing-fast, and infinitely customizable prompt for any shell.

```bash
curl -sS [https://starship.rs/install.sh](https://starship.rs/install.sh) | sh
```

### 5. Configure Fish to Use Starship

Add the Starship initialization script to your Fish configuration file.

```bash
# This command appends the init script to your config.fish
echo 'starship init fish | source' >> ~/.config/fish/config.fish
```

### 6. (Optional) Apply Dracula Theme

These steps apply the Dracula theme to GTK applications and the GNOME Terminal itself.

#### GTK Theme (Application Windows)

```bash
# Download and set up the theme files
wget [https://github.com/dracula/gtk/archive/master.zip](https://github.com/dracula/gtk/archive/master.zip) -P ~/Downloads
unzip ~/Downloads/master.zip -d ~/Downloads
mv ~/Downloads/gtk-master ~/Downloads/Dracula
mkdir -p ~/.themes
mv ~/Downloads/Dracula ~/.themes/

# Apply GTK4 theme files for modern GNOME apps
cd ~/.themes/Dracula
mkdir -p ~/.config/gtk-4.0/
cp -r assets ~/.config/
cp gtk-4.0/gtk.css ~/.config/gtk-4.0/
cp gtk-4.0/gtk-dark.css ~/.config/gtk-4.0/
cd ~

# Clean up downloaded files
rm ~/Downloads/master.zip

# Activate the theme using gsettings
gsettings set org.gnome.desktop.interface gtk-theme "Dracula"
gsettings set org.gnome.desktop.wm.preferences theme "Dracula"
```

#### GNOME Terminal Color Palette

This script automatically creates a "Dracula" profile in GNOME Terminal with the correct colors.

```bash
# Install dependency
sudo apt install dconf-cli git -y

# Clone the repository and run the installation script
git clone [https://github.com/dracula/gnome-terminal](https://github.com/dracula/gnome-terminal) ~/Downloads/gnome-terminal-dracula
cd ~/Downloads/gnome-terminal-dracula
./install.sh

# Clean up
cd ~
rm -rf ~/Downloads/gnome-terminal-dracula
```
After running the script, go to `Terminal > Preferences` and select the `Dracula` profile.
