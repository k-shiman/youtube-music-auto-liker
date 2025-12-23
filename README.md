# YouTube Music Auto Liker

I recently switched from Spotify to YouTube Music using a transfer tool. The problem? All my songs ended up in a separate playlist, but my actual **"Liked Songs"** list was empty. 

YouTube Music doesn't have a "Like All" button, and I didn't want to manually click the thumbs-up button on 1,000+ tracks.

I found a script on Reddit, fixed some bugs, added support for the English/Russian UI, and added a "human-like" delay so YouTube doesn't block you.

## How to use it

You don't need to install anything. This runs right in your browser console.

1.  **Open YouTube Music** in Chrome, Edge, or whatever browser you use.
2.  **Go to the playlist** you want to transfer.
3.  **SCROLL DOWN.** This is the most important part. Scroll all the way to the bottom of the playlist until songs stop loading. The script can only "see" the songs that are actually loaded on the screen.
4.  **Open the Console.** Press `F12` on your keyboard (or right-click anywhere -> Inspect). Click the **Console** tab.
5.  **Paste the code.** Copy the code from `script.js` (or below) and paste it into the console.
    * *Note:* If the browser gives you a scary red warning about pasting code, just type `allow pasting` and hit Enter. It's a standard security measure.
6.  **Hit Enter.**

## The Script

The script waits 1-2 seconds between likes to mimic human behavior. It also scrolls to the track before clicking to make sure it registers correctly.

```javascript
async function autoLikeSongs() {
    console.log("Starting process...");

    // Finds buttons with "Like" (English) or "Нравится" (Russian) that aren't clicked yet
    const unlikedButtons = Array.from(document.querySelectorAll('button[aria-label="Like"][aria-pressed="false"], button[aria-label="Like music"][aria-pressed="false"], button[aria-label="Нравится"][aria-pressed="false"]'));

    if (unlikedButtons.length === 0) {
        console.log('❌ No unliked songs found.');
        console.log('Tip: Make sure you scrolled to the very bottom of the playlist first!');
        return;
    }

    console.log(`✅ Found ${unlikedButtons.length} songs to like.`);

    for (let i = 0; i < unlikedButtons.length; i++) {
        const btn = unlikedButtons[i];
        
        try {
            // Scrolls to the song just like a human would
            btn.scrollIntoView({ behavior: 'smooth', block: 'center' });
            await new Promise(r => setTimeout(r, 100));

            console.log(`[${i + 1}/${unlikedButtons.length}] Liking song...`);
            btn.click();

            // Random delay between 1s and 2s to avoid rate limiting
            const delay = 1000 + Math.random() * 1000; 
            await new Promise(resolve => setTimeout(resolve, delay));

        } catch (e) {
            console.error(`Error liking song ${i + 1}:`, e);
        }
    }

    console.log('🎉 Finished! All loaded songs are now in your Liked Songs.');
}

autoLikeSongs();
