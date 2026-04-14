# 🎨 TomoTexture

A desktop tool for swapping UGC (User-Generated Content) canvas textures in **Tomodachi Life** save files. Replace in-game drawings with any image directly from your emulator's save data.

---

## ✨ Features

- **Auto-detect save root** — Automatically finds the Ryujinx save folder if available
- **Canvas browser** — Thumbnail grid of all UGC canvases in your save data
- **Replace textures** — Swap any canvas with a PNG, JPG, BMP, WebP, or other image format
- **Export canvases** — Save any in-game canvas as a PNG file
- **Automatic backups** — First-time replacements are backed up so you can revert anytime
- **Revert to original** — Restore any canvas to its backed-up original with one click
- **Multi-slot support** — Handles journaled save slots and lets you pick which ones to modify
- **Side-by-side preview** — See current vs. new image before confirming a replacement

---

## 📥 Download

Grab the latest `TomoTexture.exe` from the [`dist/`](dist/) folder — it's a single portable executable, no installation required.

---

## 🚀 How to Use

### 1. Set the Save Root

When you open TomoTexture, it will try to auto-detect your Ryujinx save folder at:

```
%APPDATA%\Ryujinx\bis\user\save\0000000000000004
```

If it doesn't find it (or you use a different emulator), click **Browse** and navigate to the folder that contains your Tomodachi Life save data. The app will recursively scan for `*.canvas.zs` files.

### 2. Browse Your Canvases

Once a valid save root is set, all UGC canvases appear as thumbnail cards. Click any thumbnail to see a larger preview.

### 3. Replace a Canvas

1. Click the **⬆ Replace** button on the canvas card you want to change
2. Select any image file (PNG, JPG, etc.) — it will be resized to 256×256 automatically
3. Review the side-by-side comparison (current → new)
4. Click **Replace** to confirm

The image is automatically:
- Converted to RGBA
- Resized to 256×256 pixels
- Transparent pixels are zeroed (to prevent color bleeding in-game)
- Swizzled to the Switch GPU tiling format
- Compressed with Zstandard

### 4. Export a Canvas

Click **⬇ Export** on any card to save the current in-game canvas as a standard PNG file.

### 5. Revert a Canvas

Click **↺ Revert** to restore the canvas to its original state from the automatic backup. This button is only active if a backup exists (created on first replacement).

### 6. Slot Management

If your save data has multiple journaled slots (e.g., `0/Ugc`, `1/Ugc`), you'll see checkboxes in the **Edit slots** row. Toggle them to control which slots are affected by replace/revert operations.

---

## ⚠️ Important Considerations

### Before You Start

- **Close the game/emulator before modifying save files.** Editing files while the game is running can corrupt your save data.
- **Keep a full backup of your save folder** before using this tool for the first time. While TomoTexture creates per-canvas backups, a full folder copy is the safest approach.

### Image Requirements

- Images are **resized to 256×256** regardless of their original size — use square images for best results.
- **Transparency is supported** — areas with alpha = 0 will show the in-game canvas background.
- Very detailed images may lose quality at 256×256. Use clean, simple designs for the best look in-game.

### Save File Compatibility

- This tool is designed for **Tomodachi Life on Nintendo Switch** (specifically the UGC food canvas system).
- It targets `UgcFood*.canvas.zs` files — these are the custom drawing canvases in the game.
- Default auto-detection path is for **Ryujinx**. For other emulators (Yuzu, Suyu, etc.), use Browse to manually select the save folder.

### Backups

- Backups are stored inside the save root in a `_ugc-tool-backups/` subfolder.
- A backup is created **only on the first replacement** of each canvas per slot — subsequent replacements won't overwrite the original backup.
- If you delete the `_ugc-tool-backups/` folder, you lose the ability to revert.

### Known Limitations

- Only `UgcFood` canvases are detected (food-related UGC drawings).
- The tool does **not** modify game code or executables — it only swaps texture data in save files.
- Windows only (uses tkinter with Windows-specific fonts and paths).

---

## 🛠️ Building from Source

### Requirements

- Python 3.10+
- Dependencies: `numpy`, `Pillow`, `zstandard`

```bash
pip install numpy Pillow zstandard
python app.py
```

### Building the .exe

```bash
pip install pyinstaller
python build_exe.py
```

The executable will be generated in the `dist/` folder.

---

## 📁 Project Structure

```
TomoTexture/
├── app.py              # Main application (UI + logic)
├── swizzle.py          # Switch GPU texture swizzle/deswizzle
├── build_exe.py        # PyInstaller build script
├── safezone.png        # App logo/badge image
├── safezone.ico        # App window icon
├── dist/
│   └── TomoTexture.exe # Compiled executable
└── README.md
```

---

## 📄 License

This project is provided as-is for personal use. Use at your own risk — always back up your save data.
