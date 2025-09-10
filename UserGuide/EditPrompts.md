# Edit Prompts for your scripting

There are two ways to generate programs:

* **Basic Program (no prompt editing):**
  The app writes the prompt behind the scenes and **aggregates** attached **API Content**, **Page Content**, and **Feed**.
  **You still provide content.** For **API items**, **define** the system‑recognized keys in the **API Content Editor**.
* **Custom Program (prompt editing):**
  Full control. Each segment uses **one source item** (API, Page, Feed or Music). **No aggregation** in Custom.

This guide covers:

* What you can (and can’t) edit
* System‑recognized keys (Core vs Display; who sets them)
* Editing prompts (Custom Program only): types, placeholders, conditionals, templates
* How Basic Program uses your aggregated content (internal behavior)
* Tips & troubleshooting

---

## 0) What you can (and can’t) edit

| Program type       | Prompt editor        | Sources supported                                                                          | What you must do                                                                                                                                             |
| ------------------ | -------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Basic Program**  | **No** (transparent) | **Aggregated** API Content + Page Content + **Feed**                                       | Attach items. For **API Content**, set `title`, `text` (and optionally `image`, `link`) in the **API Content Editor**. Page keys are auto‑filled by the app. |
| **Custom Program** | **Yes**              | **Single source per segment** (API **or** Page **or** Music) — **no aggregation, no Feed** | Pick one source type per segment and write the prompt with `<source.*>` placeholders.                                                                        |

---

## 1) System‑recognized keys (Core + Display)

These keys have special meaning to the app and are also available as prompt placeholders in **Custom Program**.
In **Basic Program**, the engine consumes the same keys internally (no visible prompt).

### API Content — **user‑defined in API Content Editor**

* **Core** → feed the script/prompt
  `title`, `text` → Custom: `<source.api.title>`, `<source.api.text>`; Basic (internal aggregation): `<apiContent.title>`, `<apiContent.text>`.
* **Display** → power the visual card/UI
  `image`, `link` → Custom: `<source.api.image>`, `<source.api.link>`; Basic (internal): `<apiContent.image>`, `<apiContent.link>`.

> **Important (Basic):** When you attach API content to a **Basic** program, you **must** set `title` and `text` (and optionally `image`, `link`) for each API item in the **API Content Editor**. The app does **not** infer these.

### Page Content — **app‑defined**

* **Core** → `pageTitle` (fallback `title`), `content`
  Custom: `<source.page.title>`, `<source.page.text>`; Basic (internal): `<pageContent.title>`, `<pageContent.text>`.
* **Display** → `ogimage`, `finalUrl`
  Custom: `<source.page.ogimage>`, `<source.page.finalUrl>`; Basic (internal): `<pageContent.ogimage>`, `<pageContent.finalUrl>`.

**Indexing (Basic aggregation only):** append `.1`, `.2`, … to target a specific attached item (e.g., `<apiContent.image.2>`).
**Concatenated (Basic only):** `<apiContent.concatenated>`, `<pageContent.concatenated>` join each item as `title\ntext` blocks.

### Feed

* **Basic (aggregated):** internal values like `newsEnabled`, `news.feedNames`, `news.headline`.
* **Custom (single source):** placeholders available in prompts:
  `<source.feed.title>`, `<source.feed.text>`, `<source.feed.headline>`.

---

## 2) Editing prompts (Custom Program only)

In Custom Program you can open a segment and edit its prompt. Basic segments never show a prompt editor.

### Where to edit (Custom)

1. Open the **segment** (Custom Program).
2. Tap **Edit Prompt**.
3. You’ll see:

   * **Prompt Type** (System / User / Duo / Solo / Multilang)
   * **Prompt Content** (text editor)
   * **Insert Placeholder** (a + menu)

### Pick the right Prompt Type (Custom)

* **System** – Rules & style (tone, do/don’t, persona notes).
* **User** – The main instructions/content for this segment.
* **Duo Talk / Duo Talk Multilang** – Dialogue for two speakers.
* **Solo Talk / Solo Talk Multilang** – Monologue for one speaker.

**Runtime selection**
If the app needs Duo or Solo, it prefers the matching type; otherwise it falls back to **User**.

### Writing your prompt (Custom)

Use `<source.*>` and other placeholders. The editor highlights placeholders and auto‑pairs conditionals.

**Inline defaults**

```
Now playing: <source.songTitle|Unknown title>
```

**Conditionals**

```
<#source.api.title>
API: <source.api.title> — <source.api.text>
</source.api.title>
```

Allowed in tag names: letters, digits, `.`, `_`, `-`
Truthiness: empty/`false`/`no`/`0`/`off`/`disabled` → false; anything else → true.

---

## 3) Placeholder reference (Custom Program)

> In Custom, each segment has **one source**. Only placeholders for that source type will resolve.

**Program**

* `<program.name>`, `<program.generationTime>`, `<program.lang>`

**Weather** *(when available)*

* `<weather.temperature>`, `<weather.condition>`, `<weather.forecast>`
* You can also type `<weather.locationName>` manually.

**Feed source (Custom)**

* `<source.feed.title>` — feed item title in context
* `<source.feed.text>` — description/body in context
* `<source.feed.headline>` — computed headline string

**API source (Custom) — requires that you defined these on the API item**

* Core: `<source.api.title>`, `<source.api.text>`
* Display: `<source.api.image>`, `<source.api.link>`
* Your custom keys: `<source.api.author>`, `<source.api.category>`, …

**Page source (Custom) — app defines these**

* Core: `<source.page.title>`, `<source.page.text>`
* Display: `<source.page.ogimage>`, `<source.page.finalUrl>`

**Music**

* `<source.songTitle>`, `<source.albumTitle>`, `<source.artistName>`

---

## 4) How Basic Program uses your content (no prompt editing)

Basic segments don’t expose a prompt editor. The engine composes using your attached content:

* **API Content (you must define):**

  * Keys per item: `title`, `text` (recommended), optional `image`, `link`
  * Internally available as:
    `apiContent.title` / `.title.1`…, `apiContent.text` / `.text.1`…,
    `apiContent.image` / `.image.1`…, `apiContent.link` / `.link.1`…,
    `apiContent.concatenated`
  * Convenience flag: `apiContentEnabled` (true when any `text` exists)

* **Page Content (app defines):**

  * Keys per item: `pageTitle` (fallback `title`), `content`, `ogimage`, `finalUrl`
  * Internally available as:
    `pageContent.title(.1…)`, `pageContent.text(.1…)`,
    `pageContent.ogimage(.1…)`, `pageContent.finalUrl(.1…)`,
    `pageContent.concatenated`
  * Convenience flag: `pageContentEnabled` (true when any `content` exists)

* **Feed (aggregated in Basic):**

  * Internally aggregated values:
    `newsEnabled`, `news.feedNames`, `news.headline`

* **Weather (when available):**

  * Internals such as `weatherEnabled`, `weather.condition`, `weather.temperature`, `weather.forecast`

> **Why still set API arguments in Basic?**
> The Basic engine can’t create meaningful text without `title`/`text`. Define them per API item in the **API Content Editor** so the auto‑prompt has high‑quality inputs.

---

## 5) Ready‑to‑use templates (Custom Program)

> These assume your segment’s source is API **or** Page **or** **Feed** (single source). Remove blocks for source types you aren’t using.

### A. Duo Talk — Custom Program

```
You are co‑hosts <personality.djName> and <personality.assistantName>. Keep it lively but concise.

<#weather.temperature>
Quick weather: <weather.condition|->, <weather.temperature|-->° now.
</weather.temperature>

<#source.feed.title>
Top story: <source.feed.title>
<source.feed.text>
</source.feed.title>

<#source.api.title>
API: <source.api.title> — <source.api.text>
</source.api.title>

<#source.page.title>
Page: <source.page.title> — <source.page.text>
</source.page.title>

Close with a handoff to the next segment.

Ensure the result contains only dialogue. Prefix each line with the speaker’s name.

<personality.djName>: Hello everyone!
<personality.assistantName>: Hi everyone!
```

### B. Solo Talk — Custom Program

```
Voice: <personality.djName>. Style: warm, clear, 2–3 short paragraphs.

<#source.feed.title>
Feed: <source.feed.title> — <source.feed.text>
</source.feed.title>

<#source.api.title>
API: <source.api.title> — <source.api.text>
</source.api.title>

<#source.page.title>
Page: <source.page.title> — <source.page.text>
</source.page.title>

<#weather.temperature>
Weather flash: <weather.condition|n/a>, <weather.temperature|-->°.
</weather.temperature>

Ensure the result is only dialogue text (prefix each line with the speaker’s name).

<personality.djName>: Hello everyone!
```

### C. System (Custom Program)

```
You must follow these rules:
- Respect the host persona(s) below.
- Keep output under 120 words unless told otherwise.
- Avoid repeating placeholders literally.

Persona notes:
- DJ: <personality.djDescription|professional, friendly>.
- Assistant: <personality.assistantDescription|insightful, witty>.
```

---

## 6) Tips & good practices

* **Choosing program type:**
  Use **Basic** for speed and automatic aggregation (API/Page/Feed). Use **Custom** when you need precise control; each segment picks **one** source type.
* **API Content (both types):**
  In the **API Content Editor**, **define** `title`, `text` (required) and `image`, `link` (optional).
  These are user‑provided and power the UI and (in Custom) your prompt placeholders.
* **Page Content (both types):**
  The app supplies `pageTitle`, `content`, `ogimage`, `finalUrl`. You can reference them directly in Custom prompts.
* **Feed in Custom:**
  Select **Feed** as the segment’s source to use `<source.feed.title>`, `<source.feed.text>`, `<source.feed.headline>`.
* **Conditions:**
  In Custom, use real variables as conditions (e.g., `<#source.api.title>…</source.api.title>`).
  In Basic, the engine uses convenience flags internally.

---

## 7) Troubleshooting

* **Where is the prompt for my Basic segment?**
  Basic segments don’t expose a prompt editor. Switch to a **Custom Program** if you need to edit prompts.
* **My Basic segment sounds empty.**
  Ensure your attached **API Content** items have `title` and `text`. Without them, the auto‑prompt has little to say.
* **`<source.api.image>` / `<source.page.finalUrl>` are empty in Custom.**
  For API, confirm you set `image`/`link` in the **API Content Editor**.
  For Page, `ogimage`/`finalUrl` are app‑provided—verify the page exposes OG data.

---

## 8) One‑page cheat sheet

**Custom Program (single source per segment)**

```
Program: <program.name> | <program.lang> | <program.generationTime>

Weather: <weather.condition>, <weather.temperature>°, <weather.forecast>

Feed: <#source.feed.title><source.feed.title> — <source.feed.text></source.feed.title>
      Headline: <source.feed.headline>

API:  <#source.api.title><source.api.title> — <source.api.text></source.api.title>
      (Display) <source.api.image>, <source.api.link>
      (Custom)  <source.api.author>, <source.api.category>

Page: <#source.page.title><source.page.title> — <source.page.text></source.page.title>
      (Display) <source.page.ogimage>, <source.page.finalUrl>
      (Custom)  <source.page.author>, <source.page.section>

Music: <source.songTitle> by <source.artistName>

Conditionals: <#some.key>…</some.key>    Defaults: <key|fallback>
```

**Basic Program (internal; no prompt editing)**

```
Aggregates: API Content + Page Content + Feed

API internals:  apiContent.title(.1…), apiContent.text(.1…),
                apiContent.image(.1…), apiContent.link(.1…),
                apiContent.concatenated
Page internals: pageContent.title(.1…), pageContent.text(.1…),
                pageContent.ogimage(.1…), pageContent.finalUrl(.1…),
                pageContent.concatenated
Feed internals: newsEnabled, news.feedNames, news.headline
Convenience:    apiContentEnabled, pageContentEnabled, weatherEnabled

Requirement:    Set API keys (title, text, [image], [link]) in API Content Editor.
```

---

### Release note (what changed)

* **Basic Program:** Aggregates **API/Page/Feed** and does not provide the prompt editor.
* **Custom Program:** Uses a **single source per segment**; Feed/API/Page/Music. No aggregation.
* **System‑recognized keys:** API keys are **user‑defined**; Page keys are **app‑defined**. All are usable in Custom prompts; the same keys drive Basic internally.
