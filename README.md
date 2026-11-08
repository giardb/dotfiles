# dotfiles

Sway desktop on Arch: sway, waybar, wofi, mako, swaylock, swaynag, termite, GTK and Qt,
all themed together by a theme switcher.

## Layout

| Repo path | Goes to |
|---|---|
| `sway/`, `waybar/`, `wofi/`, `termite/`, `wallpaper/`, `theme-switcher/` | `~/.config/<same name>/` |
| `applications/theme-switcher.desktop` | `~/.local/share/applications/` |
| `environment.d/10-qt-theme.conf` | `~/.config/environment.d/` |
| `.zshrc`, `.zprofile`, `.tmux.conf`, `.tmux.conf.local`, `.Xresources` | `~/` |

## Theme switcher

`$mod+t` (or "Theme Switcher" in the launcher) picks a theme: Nord, Nord Light,
Catppuccin, Gruvbox or Tokyo Night (the default). Each theme is a palette file in
`theme-switcher/themes/`; to add one, copy a palette and change its colors and wallpaper.

A switch regenerates the colors for sway (borders and wallpaper), waybar, wofi, mako,
swaylock, swaynag, termite, GTK and Qt, then reloads them all live. Those generated
files are not tracked (see `.gitignore`); hand-written rules live next to them:
`wofi/style.base.css` for wofi, `waybar/style.css` for waybar.

## Restore on a new machine

1. Copy the files to the places above.
2. Install the pieces:
   ```
   sudo pacman -S --needed sway swaybg swaylock waybar wofi mako grim slurp \
       plasma-integration breeze breeze-icons sassc otf-geist-mono-nerd
   yay -S termite nordic-theme nordic-polar-standard-buttons-theme
   ```
3. Build the Catppuccin, Gruvbox and Tokyo Night GTK themes from
   [Fausto-Korpsvart](https://github.com/Fausto-Korpsvart)'s `*-GTK-Theme` repos:
   `cd themes && ./install.sh -d ~/.local/share/themes -c dark`
   (Catppuccin uses `-m dark` instead of `-c dark`).
4. Generate the theme files: `~/.config/theme-switcher/theme-switch apply tokyo-night`
5. Log out and back in, so `environment.d/10-qt-theme.conf` (and `.zprofile` for login shells)
   sets `QT_QPA_PLATFORMTHEME=kde` for the session. GDM only reads `environment.d`.
