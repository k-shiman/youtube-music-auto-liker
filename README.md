# 🎵 YouTube Music Auto Liker

> **Automatically like all songs from a playlist in YouTube Music**  
> A small browser script for people who migrated from Spotify and ended up with an empty *Liked Songs* section.

---

## Why this exists

I moved from Spotify to YouTube Music using a transfer tool.

Playlists transferred fine.  
My **Liked Songs** didn’t.

Everything landed in a regular playlist, and YouTube Music offers no way to like tracks in bulk.  
There is no **“Like all”** button, and clicking the 👍 icon hundreds of times manually wasn’t an option.

So I fixed an existing script, cleaned it up, and made it behave more like a real user.

---

## What this script does

- 👍 Likes all **unliked tracks** in the current playlist  
- 🧍 Scrolls to each track before clicking (required for YTM)
- ⏱ Uses a **random delay (1–2 seconds)** between likes
- 🖥 Runs directly in the **browser console**
- 📦 No installs, no extensions

---

## Important limitation

> ⚠️ **This matters**

YouTube Music only loads part of a playlist into the page.

If a track is **not loaded on screen**, the script **cannot interact with it**.  
You *must* scroll the playlist to the bottom before running the script.

---

## How to use

### Step 1 — Open YouTube Music
Use Chrome, Edge, or any Chromium-based browser.

### Step 2 — Open your playlist
This should be the playlist containing the tracks you want to like.

### Step 3 — Scroll to the bottom
Scroll until new songs stop loading.

> 💡 Skipping this step will cause some tracks to be missed.

### Step 4 — Open DevTools
- Press `F12`
- or right-click → **Inspect**
- Switch to the **Console** tab

### Step 5 — Paste the script
Copy the script below (or from `script.js`) and paste it into the console.

If Chrome shows a warning, type:

**allow pasting**




and press Enter.

### Step 6 — Run it
Press **Enter** and wait.  
The script will like songs one by one.

---

## Script

```javascript
async function autoLikeSongs() {
    console.log("Starting process...");

    const unlikedButtons = Array.from(
        document.querySelectorAll(
            'button[aria-label="Like"][aria-pressed="false"], ' +
            'button[aria-label="Like music"][aria-pressed="false"]'
        )
    );

    if (unlikedButtons.length === 0) {
        console.log('No unliked songs found.');
        console.log('Make sure you scrolled to the very bottom of the playlist.');
        return;
    }

    console.log(`Found ${unlikedButtons.length} songs to like.`);

    for (let i = 0; i < unlikedButtons.length; i++) {
        const btn = unlikedButtons[i];

        try {
            btn.scrollIntoView({ behavior: 'smooth', block: 'center' });
            await new Promise(r => setTimeout(r, 100));

            console.log(`[${i + 1}/${unlikedButtons.length}] Liking song...`);
            btn.click();

            const delay = 1000 + Math.random() * 1000;
            await new Promise(resolve => setTimeout(resolve, delay));
        } catch (e) {
            console.error(`Error on song ${i + 1}`, e);
        }
    }

    console.log('Done. All loaded songs should now be liked.');
}

autoLikeSongs();

```

## Large playlists

### For very large playlists:

- Scroll to the bottom

- Run the script

- Scroll further

- Run it again

- Repeat until everything is liked.


## Disclaimer

This is an unofficial workaround.
Use at your own risk.

## Credits

Based on a community script from Reddit, refined and documented from real usage.
