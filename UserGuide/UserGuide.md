# Radio Starlight — **Official User Guide**

Design, generate, and publish living radio programs from the web’s freshest content. This guide walks you through setup, content preparation, program design (Basic or Custom), generation, playback, and publishing.

> **Note:** UI labels may vary slightly by platform and version. Where you see `Library → X`, read it as navigating within the app’s sidebar and panels.

---

## Table of contents

1. [Quick start](#quick-start)
2. [Before you begin](#before-you-begin)
3. [Prepare your Library](#prepare-your-library)  
   3.1 [Personalities](#personalities) · 3.2 [Sound Sets](#sound-sets) · 3.3 [Feeds](#feeds) · 3.4 [APIs](#apis) · 3.5 [Pages](#pages) · 3.6 [Generative AIs](#generative-ais) · 3.7 [Credentials](#credentials)
4. [Build a Program](#build-a-program)  
   4.1 [Basic Program](#basic-program) · 4.2 [Custom Program](#custom-program)
5. [Generate & Play](#generate--play)
6. [Publish (optional)](#publish-optional)
7. [Prompt mini‑guide](#prompt-mini-guide)
8. [Troubleshooting](#troubleshooting)
9. [Glossary](#glossary)

---

## Quick start

### Import a starter Catalog

1. In the app, open **Library → Catalogs → Import → URL...**
2. Paste the following URL. 

```
https://catalog.radio-starlight.com/Catalogs/Featured/Starter-Catalog/
```

3. Click **Import**, and confirm.

> **What this does:** Imports starter Personalities, Sound Sets, source definitions, and program templates so you can try Radio Starlight immediately.

---

## Before you begin

#### Voices for speech (recommended)

Download high‑quality system voices used by the **Pre‑defined personalities** for natural narration.

* Nathan (Enhanced)
* Samantha (Enhanced)
* Zoe (Enhanced)
* Ava (Enhanced)

**How to download**

* **iOS / iPadOS:** `Settings → Accessibility → Spoken Content` *(or* `Read & Speak`\*) → Voices → English → \[select a voice] → Download\`
* **macOS:** `System Settings → Accessibility → Spoken Content` *(or* `Read & Speak`\*) → System Voice → English → \[select a voice] → Download\`

After downloading, return to the **Personality** view; the notice will clear automatically.
*Note: Voice availability can vary by device and OS version.*

#### **Music playback (optional):**

Apple Music playback requires an active subscription and authorization in the app.

#### **Model access:**

If you plan to use cloud models (e.g., OpenAI, Gemini), have your API keys ready. For on‑device generation on Apple hardware, ensure your device supports Apple Foundation Models / Image Playground.

---

## Prepare your Library

Your **Library** holds everything programs reference: Personalities, Sound Sets, Feeds, APIs, Pages, Generative AIs, and Credentials.

### Personalities

**Use a predefined personality (fastest):**

1. Open **Library → Personalities**.
2. Ensure a high‑quality system voice is downloaded (see **Before you begin**).

**Create a new personality:**

1. **Library → Personalities → Add** (top‑right).
2. Fill out fields.
3. **Save**.

**Roles**

* **DJ** and **Sidekick** speak in talk segments.
* **Newsreader** speaks in news segments.
* **System** provides openings, transitions, or guidance.
* You specify personality for specific segment in Custom Program. 

**Description**

* The description informs the script generator and keeps the on‑air persona consistent across segments and episodes. Write it as if you’re briefing talent (e.g., “Warm, witty, keeps segments tight. Loves local culture facts.”).

---

### Sound Sets

Create the sound of your station—beds, bumpers.

1. **Library → Sound Sets → Add**.
2. Name the set, then add **sound elements** (BGM, Jingle).
3. **Save**.

> **Tip:** Keep names descriptive so it’s easy to target from programs.

---

### Feeds

Connect RSS/Atom/JSON feeds for headlines and updates.

1. **Library → Feeds → Add**.
2. **Import From Website** (recommended)
   * Enter the **Site Home URL** and click **Fetch feed info**.
   * Click **Use These** to auto‑populate if a feed is advertised.
3. Or **fill fields manually**.
4. Click **Test Load** to verify.
5. **Save**.

**Preview**

* Use **Preview This Feed** to inspect actual items before using them in a program.

**Details panel**

* **Original content extraction:** If a feed item is thin, enable loading the item’s webpage and extract the main article body.
* **Summarization:** If items are long, enable summarization before passing to the LLM.
* **Translation:** Optionally translate to your program language.

> **Model note:** Extraction, summarization, and translation require a generative model with a sufficiently large model with large context window. Assign an appropriate model in **Generative AIs**. 

---

### APIs

Use any accessible API that returns JSON.

1. **Library → APIs → Add API Content**.
2. Fill fields as needed.
3. Define **Prompt Arguments** (see below) so segments can reference returned data.
4. **Save**

**Prompt Arguments**

* Map JSON fields to named arguments that your prompts can reference.
* Example JSON:

  ```json
  {
    "contents": [
      { "article": "content 1" },
      { "article": "content 2" }
    ]
  }
  ```
* To pass `"content 1"` under the name `article`, use the data path:

  ```
  ["contents"][0]["article"]
  ```

**Special argument keys**

* `link` — URL for attribution in the player during the related segment.
* `image` — Image URL shown in the player during the related segment.

> **Tip:** Name arguments clearly. Your prompt reads better and is easier to maintain.

---

### Pages

Turn any public webpage into segment input.

1. **Library → Pages → Add Page Content**.
2. Fill fields as needed.
3. Choose content processing:
   * **Extract main content** (boilerplate removal)
   * **Summarize**
   * **Translate** to target language
4. **Save**

> **Model note:** Extraction, summarize and translate also require a large model with a larger context window and multilingual capability.

---

### Generative AIs

Declare the text and image engines you plan to use.

1. **Library → Generative AIs → Add**.
2. Fill fields as needed.
3. For cloud services needing auth, **use placeholders** for credentials (see below).
4. **Save**.

> **Tip:** Create multiple entries (e.g., a long‑context model for content processing and a fast model for small segment), then assign per phase in Program Settings.

---

### Credentials

Store secrets in the system Keychain and reference them safely.

* **Never** paste API keys directly into headers or body.
* Use the `credentials` placeholder anywhere a secret belongs:

```
Authorization: Bearer <credentials:ServiceApiKey>
```

**How to set**

1. Add your API (or Page) with the placeholder as shown above and **Save**.
2. Reopen it: the placeholder appears **highlighted in pink** with a status badge (**Missing** / **Available**).
3. Click the badge to open the **Credential sheet** and create or link the keychain entry.

You can also manage all secrets in **App Settings → Credentials**.

> **Recommendation:** Prefer adding credentials from the placeholder workflow to avoid typos and ensure correct binding.

---

## Build a Program

Choose **Basic** for an instant auto‑generated show, or **Custom** for segment‑by‑segment control.

### Basic Program

Automates most choices while letting you pick sources.

1. **Library → Programs → Add → Basic Program**.
2. Fill the General panel.
3. **Contents** panel:
   * (Optional) Select **Apple Music** playlists to include music interludes. *(Your Apple Music subscription required.)*
   * Add **Feeds**, **API Content**, and **Page Content** from your Library—or create new ones inline.
4. **Settings** panel:
   * Playback options.
   * Select **Generative AIs** for each phase (content processing, script generation, image generation).
     > Defaults inherit from **App Settings**; override here if needed.
5. **Save**.

> **Result:** A ready‑to‑generate show that cites sources and uses your default personality and sound.

---

### Custom Program

Design your format, segment order, prompts.

1. **Library → Programs → Add → Custom Program**.
2. Fill the General panel.
3. **Segments** panel: Add and arrange segments.
   * **Static Segment:** A selected personality reads predefined text (great for intros/outros, sponsor reads).
   * **Custom Segment:** One or more personalities discuss **selected content** (Feeds/APIs/Pages). Attach a **prompt** to guide generation.
   * **Templates:** Start from reusable segment templates (the same ones Basic uses under the hood) and tweak.
4. **Settings** panel:
   * Playback options.
   * Select **Generative AIs** for each phase (content processing, script generation, image generation).
5. **Save**.

See more about prompt design in [Using API Templates with Placeholders](CreateApi.md) and [Edit Prompts](EditPrompts.md). 

---

## Generate & Play

1. Open your Program and tap **Generate**.
   * If prerequisites are missing, a **Readiness Checklist** appears. Complete all items.
2. After generation, a **Broadcast** (the generated show) appears under the program.
3. Open the broadcast and tap **Play**.
   * If **Autoplay** is enabled, playback starts automatically.
4. During playback, the player shows:
   * The live **script**
   * **Related images**
   * **Source links** and **attribution**
   * (If provided) per‑item `link` and `image` metadata from your API arguments

---

## Publish (optional)

Exoprt your designed program, generated broadcast, sources, personalities, and Sound Sets then share it. 

1. Open a entity you want to export. 
2. Choose **Export**.

You can publish your data set for your listener with Starlight Catalog Service. 
User can register your catalog in **Library → Catalogs → Import**.

See also [StarlightCatalogService](https://github.com/RayKitajima/StarlightCatalogService).

--- 

## Troubleshooting

**I don’t hear speech / the voice sounds robotic**

* Ensure a high‑quality system voice is downloaded (see **Before you begin**).
* Check to select the correct voice.

**A feed won’t load or is empty**

* Use **Import From Website** again to detect the correct feed URL.
* Click **Preview This Feed** to verify items exist.
* If items are short, enable **Deep load** in **Details**.

**Generation fails or truncates**

* Assign a model with a **larger context window** in **Program → Settings → Generative AIs** for content processing and script generation.
* Reduce the number/length of source items or enable **Summarization**.

**Credentials show “Missing”**

* Open the source, click the **status badge**, and add the key in the **Credential sheet**.
* Or manage secrets in **App Settings → Credentials**.

**Apple Music tracks don’t play**

* Confirm your **Apple Music subscription** and that the app has playback authorization.
* Re‑select playlists in **Program → Contents**.

**Attribution isn’t appearing in the player**

* For APIs, map `link` and (optionally) `image` in **Prompt Arguments**.
* For Pages and Feeds, ensure the source item has a valid URL after extraction.

---

## Glossary

* **Broadcast:** A generated, playable show (an instance of a Program).
* **Catalog:** A portable collection of programs, sources, personalities, and sound assets.
* **Custom Program:** A fully designed format with per‑segment prompts, sources, and roles.
* **Basic Program:** Quick, auto‑generated format built from your chosen sources.
* **Details panel:** Advanced feed/page processing options: extraction, summarization, translation.
* **Generative AIs:** Configured text/image engines (cloud or on‑device) referenced by Programs.
* **Personality:** On‑air voice definition (role, tone, quirks) used by segments.
* **Sound Set:** Music beds, stingers used throughout your show.
* **Sources:** Feeds, APIs, and Pages that supply content to segments.

---

### You’re ready to broadcast

Build your first show in minutes:

* Use **Basic** to go fast.
* Use **Custom** to craft your signature format.
  Generate, play, and (optionally) publish to the open catalog. 
