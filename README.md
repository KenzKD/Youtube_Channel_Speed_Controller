# YouTube Channel Speed Controller
Automatically saves and restores your preferred playback speed on a per-channel basis when using the [Enhancer for YouTube™](https://www.mrfdev.com/enhancer-for-youtube) (EfYT) browser extension.

> [!CAUTION]
> - Vibe-coded at 2am. It works for me. No promises it works for you. Read the code first.
> - The script relies on YouTube's current page structure to detect channels, titles, and music badges — if YouTube changes its layout, detection may silently stop working until the script is updated.
> - Speeds are stored per browser profile. If you use multiple browsers or profiles, you'll need to export and import separately for each one.

---

## How It Works
- When you change the playback speed on a channel using EfYT's speed buttons or scrolling over the speed icon, that speed is saved automatically.
- The next time you visit a video from that channel, the saved speed is restored using EfYT's own `+` / `−` buttons — keeping EfYT's internal state fully in sync.
- If you set the speed back to your EfYT global default (I keep mine at 2x), the channel is deleted from the saved list automatically.
- Videos detected as music (via category metadata, Official Artist Channel badge, or title keywords like "official audio") are automatically forced to 1x, regardless of any saved override.

---

## Installation

1. Install the [Enhancer for YouTube™](https://www.mrfdev.com/enhancer-for-youtube) extension for Chrome, Edge, or Firefox.
2. Open the **Enhancer for YouTube™ Options** page by clicking the extension icon in your browser toolbar.
3. Navigate to the **Playback Speed** section.
4. Disable **"Override default playback speeds"**.
> [!WARNING]
> Step 4 is necessary since EfYT's implementation of **"Override default playback speeds"** can break sometimes and is not reliable.
5. Select your preferred **Default Playback Speed** from the drop-down menu. 
5. Scroll down to the **Custom Script** section.
6. Copy the entire contents of [YouTube_Channel_Speed_Controller.js](https://github.com/KenzKD/YouTube_Channel_Speed_Controller/blob/main/YouTube_Channel_Speed_Controller.js) and paste them into the Custom Script text area.
7. Near the top of the pasted script, locate the line:

   ```
   const DEFAULT_SPEED_FALLBACK = 2;
   ```
   Change the number `2` to match the Default Playback Speed you selected in step 4.
 > [!WARNING]
 > Step 8 is necessary for modern Chromium-based browsers.

8. Click **Save**.
9. Enable the **"Automatically execute the script when YouTube is loaded in a tab"** option.
10. Reload any open YouTube tabs.

> [!TIP]
> You can learn how to set up keyboard shortcuts for EfYT here: [Manage Extension Shortcuts](https://www.mrfdev.com/manage-extension-shortcuts).

---

## Backup & Restore
All saved channel speeds are stored in your browser's `localStorage`. You can export and import them from the **DevTools Console** on any YouTube page.

### Opening the Console
Press `F12` (or `Ctrl+Shift+I` on Windows/Linux, `Cmd+Option+I` on Mac) to open DevTools, then click the **Console** tab.

### Export
Run the following in the Console:
```
efytSpeed.exportChannelSpeeds();
```
Your saved channels and speeds will be exported as a JSON file.

### Import
Run the following in the Console:
```
efytSpeed.importChannelSpeeds();
```
A large blue button will appear in the top-right corner of the page:

<img width="1037" height="177" alt="image" src="https://github.com/user-attachments/assets/326305de-2edc-49da-bada-9d70d8200a45" />

Click it, then select the `.json` backup file you exported earlier. The console will confirm how many channels were imported:

```
[EfYT-ChSpeed] Imported 3 channel(s).
```
> [!NOTE]
> If you don't click the button within 8 seconds, it disappears automatically. Just run `efytSpeed.import();` again to bring it back.

> [!CAUTION]
> Importing will overwrite any existing saved speed for channels included in the backup. Channels not present in the backup are left untouched.
