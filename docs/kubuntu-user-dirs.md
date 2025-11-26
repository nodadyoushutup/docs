# Kubuntu Default User Directories

[[Home]] > Desktop Tips

Kubuntu (and most Linux desktops) follow the [XDG user directories specification](https://www.freedesktop.org/wiki/Software/xdg-user-dirs/). The spec simply defines default folders (Desktop, Documents, Downloads, etc.) so GUI apps know where to look when they offer shortcuts like “Save to Downloads.”

## Where the mappings live

The canonical list is in `~/.config/user-dirs.dirs`. Example:

```bash
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Downloads"
XDG_TEMPLATES_DIR="$HOME/Templates"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Documents"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Pictures"
XDG_VIDEOS_DIR="$HOME/Videos"
```

Every entry is just a shell path. Apps read these values to populate their Places/sidebar items.

## What each folder is used for

- **Desktop** – icons that appear on the Plasma desktop. Remove it if you do not use desktop files.
- **Downloads** – browsers, Steam, and most installers write here by default.
- **Templates** – the “Create New → From Template” option in Dolphin/Plasma reads files from this folder.
- **Public** – intended for “shared” content; mostly unused today.
- **Documents** – office apps and file pickers default here for text/office files.
- **Music** – media players auto-scan this path for libraries.
- **Pictures** – screenshot tools and image apps default here.
- **Videos** – media players and encoders use this as the default video library.

None of these directories are required for Kubuntu to boot or run—they are organizational shortcuts only. If a folder is missing, KDE or an app may recreate it when needed, but nothing critical breaks.

## Deleting or moving the folders

1. Decide which folders you want to keep. They can live anywhere (even `~`).
2. Update `~/.config/user-dirs.dirs` and point each `XDG_*` entry at the actual path you want.
3. Run `xdg-user-dirs-update` to apply the changes.
4. Remove or rename the old directories if you no longer need them, e.g. `rm -r ~/Templates`.

Because the sync uses `rsync --delete`, the Wiki will stay consistent with whatever folder structure you maintain in `docs/wiki/`.

### Example: everything directly in `$HOME`

```bash
XDG_DESKTOP_DIR="$HOME"
XDG_DOWNLOAD_DIR="$HOME"
XDG_TEMPLATES_DIR="$HOME"
XDG_PUBLICSHARE_DIR="$HOME"
XDG_DOCUMENTS_DIR="$HOME"
XDG_MUSIC_DIR="$HOME"
XDG_PICTURES_DIR="$HOME"
XDG_VIDEOS_DIR="$HOME"
```

After saving the file, run:

```bash
xdg-user-dirs-update
```

Now the standard folders will no longer be recreated unless an app explicitly makes them.

## Renaming to lowercase

If you prefer lowercase names (e.g., `~/documents`), rename the directories and update `user-dirs.dirs` to match:

```bash
mv ~/Documents ~/documents
sed -i 's|$HOME/Documents|$HOME/documents|' ~/.config/user-dirs.dirs
xdg-user-dirs-update
```

Dolphin’s sidebar labels are friendly names (Documents, Downloads, etc.). Updating the paths does **not** change the labels automatically. Right-click a shortcut in Dolphin → **Edit** to rename the visible text if you want it to show lowercase names.

## What **not** to delete casually

The dot folders in `$HOME` (`~/.config`, `~/.local`, `~/.cache`, `~/.ssh`, etc.) store application settings, keys, and runtime data. Do not remove these unless you know exactly what will happen.

---

If you need a tailored `user-dirs.dirs` template, decide which folders you use and adjust the entries before running `xdg-user-dirs-update`. That keeps Kubuntu and Dolphin aligned with your preferred layout without any hidden steps.

Back to [[Home]]
