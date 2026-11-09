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
swaylock, swaynag, swayosd, termite, GTK and Qt, then reloads them all live. Those generated
files are not tracked (see `.gitignore`); hand-written rules live next to them:
`wofi/style.base.css` for wofi, `waybar/style.css` for waybar.

## Desktop services and shortcuts

Started by `sway/config`, helper scripts in `sway/scripts/`:

| Feature | Keys |
|---|---|
| Screenshots (saved to `~/Pictures/Screenshots` and copied) | `Print` monitor, `Shift+Print` area, `$mod+Print` window, `$mod+Shift+Print` area + swappy |
| Screen recording to `~/Videos` (again to stop) | `$mod+Ctrl+Print` |
| Clipboard history (cliphist) | `$mod+c` |
| Night light 20:00-07:00 (wlsunset) | `$mod+n` or the waybar moon button |
| Volume / brightness popup (swayosd), mic mute | media keys, `XF86AudioMicMute`, `$mod+Alt+m` |
| Password prompt for admin actions | polkit-gnome |
| Screens off after 10 min idle, never during fullscreen; no auto-lock | lock with `$mod+Escape` |

## Screen layout per laptop

Both laptops share the hostname, so `sway/io/outputs` includes the layout picked by
`sway/outputs/select` from the laptop model (`/sys/class/dmi/id/product_name`, spaces
become dashes): `sway/outputs/XPS-15-9570` here, `sway/outputs/default` for any other.
To give another laptop its own layout, copy `default` to its model name.

## Restore on a new machine

1. Copy the files to the places above.
2. Install the pieces:
   ```
   sudo pacman -S --needed sway swaybg swaylock waybar wofi mako grim slurp \
       plasma-integration breeze breeze-icons sassc otf-geist-mono-nerd \
       wl-clipboard swappy playerctl brightnessctl jq swayidle \
       polkit-gnome cliphist wlsunset swayosd wf-recorder
   yay -S termite nordic-theme nordic-polar-standard-buttons-theme
   ```
3. Build the Catppuccin, Gruvbox and Tokyo Night GTK themes from
   [Fausto-Korpsvart](https://github.com/Fausto-Korpsvart)'s `*-GTK-Theme` repos:
   `cd themes && ./install.sh -d ~/.local/share/themes -c dark`
   (Catppuccin uses `-m dark` instead of `-c dark`).
4. Generate the theme files: `~/.config/theme-switcher/theme-switch apply tokyo-night`
5. Log out and back in, so `environment.d/10-qt-theme.conf` (and `.zprofile` for login shells)
   sets `QT_QPA_PLATFORMTHEME=kde` for the session. GDM only reads `environment.d`.
