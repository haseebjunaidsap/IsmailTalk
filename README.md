# Talk with Pip

A talking buddy that helps children aged 5 to 8 practise speaking in whole sentences.
Pip asks fun questions, picks topics from the child's answers, reads everything aloud,
listens with the microphone after it speaks, and gently nudges a quiet child.

## What's in the folder

- `index.html` — the whole app (page, voice, microphone, nudges).
- `server.js` — an optional tiny server that shows the page and keeps your AI key on your computer.
  No installs needed. Not used with GitHub Pages.
- `README.md` — this file.

## Quick start on a computer

1. Install Node.js 18 or newer from https://nodejs.org if you don't have it.
2. Get an Anthropic API key from https://console.anthropic.com.
3. Open a terminal in this folder and run:

   **Mac / Linux**
   ```
   ANTHROPIC_API_KEY=sk-ant-your-key node server.js
   ```
   **Windows (PowerShell)**
   ```
   $env:ANTHROPIC_API_KEY="sk-ant-your-key"; node server.js
   ```
4. Open **http://localhost:8787** in Chrome or Edge.
5. Allow the microphone when the browser asks.

## Using Pip on an iPhone or iPad

Talking out loud needs Pip opened in **Safari** at an **https://** address. It won't work
inside the Claude app (the app blocks the microphone), and it won't work at a plain `http://` address.

**Option A: GitHub Pages (free, no computer left running)**

*Get a Claude API key*
1. Go to https://console.anthropic.com and sign in. This is separate from a regular Claude
   subscription; API use is billed on its own.
2. Under **Billing**, add a small amount of credit (for example $5). Optionally set a monthly
   spending limit under **Limits**.
3. Under **API Keys**, click **Create Key**, name it "Pip", and copy it (it starts with `sk-ant-`).
   It's only shown once, so keep it somewhere safe.

*Put Pip on GitHub*
1. Sign in at https://github.com, click **+** → **New repository**.
2. Name it `talk-with-pip`, choose **Public** (free accounts need public repositories for
   GitHub Pages; your key is never in it), and click **Create repository**.
3. Click **uploading an existing file**, drag in `index.html` and `README.md`, and click
   **Commit changes**. (`server.js` isn't needed; GitHub Pages can't run it.)
4. Go to **Settings → Pages**. Set **Source** to **Deploy from a branch**, **Branch** to
   **main** and **/ (root)**, then click **Save**.
5. After a minute or two, your address appears at the top of that page:
   `https://YOUR-USERNAME.github.io/talk-with-pip/`

*Connect Pip to Claude (once per device)*
1. Open your address in **Safari** on the iPhone (not inside the Claude app).
2. Tap **Grown-up settings** on the start screen, paste your API key, and tap **Save key**.
   It should say "AI is on (using the key saved in this browser)."
3. Optional: tap Share → **Add to Home Screen** for a Pip icon.

The key is saved only in that browser on that device. Never put it in a file on GitHub. Anyone
who finds your address can open the page, but they can't use your key. If you think the key
was seen by someone else, delete it in the Anthropic console and make a new one.

*Updating Pip later:* in the repository, click **Add file → Upload files**, drag in the new
`index.html`, and commit. The site updates within a couple of minutes.

**Option B: Netlify Drop**
1. On a computer, go to https://app.netlify.com/drop and drag this whole folder onto the page.
   Sign in with a free account so the site stays up. You get an `https://….netlify.app` address.
2. Open that address in **Safari** on the iPhone, then add your key under **Grown-up settings**
   as in Option A.

**Option C: keep the key on your computer**
1. Start the server as in Quick start.
2. Install Cloudflare's free tunnel tool, `cloudflared`
   (Mac: `brew install cloudflared`, Windows: `winget install Cloudflare.cloudflared`).
3. In a second terminal run:
   ```
   cloudflared tunnel --url http://localhost:8787
   ```
   It prints an address like `https://something.trycloudflare.com`. Open it in Safari on the iPhone.
   The address changes each time you run the command.

**On the iPhone, check once:**
- Settings → General → Keyboard → **Enable Dictation** is on (Safari's listening uses it).
- Settings → Apps → Safari → **Microphone** is set to **Ask** or **Allow**.
- When Pip first asks for the microphone, tap **Allow**.

**How it works:** tap your age, and the iPhone asks for the microphone once. After that the mic
turns on by itself every time Pip asks something, and turns off when your child is done.
If the iPhone ever wants a tap, the green mic button pulses. Tap it and talk.

## Without the server

You can double-click `index.html`, open **Grown-up settings** on the start screen, and paste
an API key. The key is saved only in that browser. The microphone may not work when the page
is opened as a file, so the server is the better choice.

With no key at all, Pip still works in a simple scripted mode.

## Settings on the page

- **Talking speed:** Very very slow, Very slow, Slow, Normal, Fast.
- **Voice:** choose any English voice on your device. Edge's "Natural" voices and Chrome's
  Google voices sound best.
- **Nudge after:** how long Pip waits in silence before encouraging the child (3–20 seconds, or Never).
- **Mic on after Pip talks:** turns listening on automatically when Pip finishes speaking.
  When the child stops talking, their words are sent for them.
- **Wait for pauses:** Short, Medium or Long. Choose Long if Pip answers before your child is finished.

## How Pip knows your child is finished

- It keeps listening through short "thinking" pauses.
- It waits longer when the words sound unfinished ("I like dogs because…"), for very short
  answers, and for younger ages.
- After a short pause, a quick AI check reads the question and the answer so far and decides
  whether your child sounds done. If yes, Pip answers right away. If not, Pip waits longer.
- On computers, a sound meter hears "ummm" and quiet talking, so Pip doesn't cut in.
- Your child can also say "I'm done" or tap the mic.
- When Pip decides they're done, the microphone turns off.

## Good to know

- **Microphone:** works in Chrome, Edge and Safari. Firefox doesn't support speech
  recognition, so children type there instead. Browsers only allow the microphone on
  `localhost` or an `https://` address (see "Using Pip on an iPhone or iPad").
- **If the mic can't be used,** Pip says so and switches to typing. On a phone, your child can
  still talk by tapping the text box and then the microphone key on the keyboard; Pip sends
  the answer when they stop.
- **Model:** uses `claude-haiku-4-5-20251001` for quick replies. With the server, set `PIP_MODEL`
  to change it.
- **Port:** with the server, set `PORT` to use something other than 8787.
- **Privacy:** Pip is told never to ask for last names, addresses, school names or passwords.
  Chat text is sent to Anthropic to generate replies; nothing is stored by Pip.
- **If AI shows as off:** open Grown-up settings and save the key again. Clearing Safari's
  history and website data removes the saved key.
- **Cost:** each of Pip's replies is one short API call, so a typical session costs a few cents.
