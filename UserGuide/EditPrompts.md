# Edit Prompts for your scripting

Prompts tell the app **how to talk**. You can write your own script instructions and drop in live data (program name, weather, news, content you loaded, etc.) using simple **placeholders** like `<program.name>`.

This guide covers:

* Choosing a **Prompt Type** (System / User / Duo / Solo / Multilang)
* Writing **Prompt Content**
* Inserting **placeholders** and **conditional blocks**
* A complete **placeholder reference** split by **Basic** vs **Custom** program types
* **System‑recognized keys** (Core + Display; prompt‑visible)
* Ready‑to‑use **templates**
* Tips and troubleshooting
* **How the engine renders your prompt (for power users)**

---

## 1) Where to edit

1. Open the **segment** you want to customize.
2. Tap **Edit Prompt**.
3. You’ll see:
   * **Prompt Type** (a small menu)
   * **Prompt Content** (a text editor)
   * **Insert Placeholder** (a + menu with categories)

---

## 2) Pick the right Prompt Type

You’ll either **create** a prompt or **edit** an existing one.

* **When creating:** the *Prompt Type* menu shows only the types you haven’t used yet for this segment.
* **When editing:** the *Prompt Type* is fixed (you can edit the content, not the type).

**What the types mean**

* **System** – Invisible “rules” and style (tone, do/don’t, persona notes).
* **User** – The main instructions/content for this segment.
* **Duo Talk / Duo Talk Multilang** – Dialogue format for **two speakers**.
* **Solo Talk / Solo Talk Multilang** – Monologue format for **one speaker**.
* **Multilang** variants are for non‑English programs.

**How the app chooses at runtime**

* **Duo casting:** prefers **Duo Talk** (or **Duo Talk Multilang** for non‑English). Falls back to **User**.
* **Solo casting:** prefers **Solo Talk** (or **Solo Talk Multilang**). Otherwise **User**.

---

## 3) Write your Prompt Content

Type normal text. Wherever you want live data, insert a **placeholder** (e.g., `<program.name>`). Use **Insert Placeholder** so you don’t have to memorize names.

As you type, the editor:

* **Highlights placeholders** (colored text so you can spot them).
* Keeps your cursor where you expect it.
* Inserts **opening + closing** tags for conditional blocks for you.

**Highlight legend** (approximate colors):

* Placeholders: **blue**
* Conditional tags (like `<#weatherEnabled>…</weatherEnabled>`): **peach**
* Credentials tokens (advanced): **pink**

---

## 4) Placeholders 101

* **Basic placeholders** look like `<something.like.this>`.

* **Conditional blocks** show content **only when a key is truthy**:

  ```
  <#weatherEnabled>
  Today’s weather: <weather.temperature>°, <weather.condition>.
  </weatherEnabled>
  ```

  Use **Insert Placeholder** to add pairs—when you insert an opener (e.g., `<#weatherEnabled>`), the editor auto‑adds the matching closer (`</weatherEnabled>`).

* **Inline defaults** keep your prompt tidy when data is missing:

  ```
  Now playing: <source.songTitle|Unknown title>
  ```

  If the value is missing, the text after `|` is used.

---

## 5) Quick placeholder reference

> The Insert menu adapts to your program type and sources. Some groups appear only in **Basic Program** or only in **Custom Program**.

### Always available

**Program**

* `<program.name>` – Program title
* `<program.generationTime>` – When this segment was generated
* `<program.lang>` – Program language (e.g., `english`)

**Weather values** *(available when weather is present)*

* `<weather.temperature>`, `<weather.condition>`, `<weather.forecast>`

  > You can also type `<weather.locationName>` manually.

**Note on “Enabled” flags:**
“Enabled” flags (e.g., `weatherEnabled`) exist **only as convenience in Basic Program**. In **Custom Program**, simply use **any real variable as the condition** (examples below).

---

## 5a) System‑recognized keys (Core + Display; prompt‑visible)

These keys have **special meaning in the app** *and* are **available as prompt placeholders**. They also power the on‑screen card/UI.

> **Definition source**
>
> * **API Content (user‑defined):** You define these keys in the **API Content Editor**.
> * **Page Content (app‑defined):** The app defines these keys for you.

### API Content (user‑defined in the API Content Editor)

* **Core content keys** → feed your prompt

  * `title` → `<apiContent.title>` (Basic), `<source.api.title>` (Custom)
  * `text`  → `<apiContent.text>`  (Basic), `<source.api.text>`  (Custom)

* **Display keys** → also prompt‑visible

  * `image` → `<apiContent.image>` (Basic), `<source.api.image>` (Custom)
  * `link`  → `<apiContent.link>`  (Basic), `<source.api.link>`  (Custom)

### Page Content (app‑defined)

* **Core content keys** → feed your prompt

  * `pageTitle` (fallback: `title`) → `<pageContent.title>` (Basic), `<source.page.title>` (Custom)
  * `content`                      → `<pageContent.text>`  (Basic), `<source.page.text>`  (Custom)

* **Display keys** → also prompt‑visible

  * `ogimage`  → `<pageContent.ogimage>`  (Basic), `<source.page.ogimage>`  (Custom)
  * `finalUrl` → `<pageContent.finalUrl>` (Basic), `<source.page.finalUrl>` (Custom)

**Indexing and concatenation (Basic Program):**

* **Indexing:** append `.1`, `.2`, … to target specific items (e.g., `<apiContent.image.2>`, `<pageContent.finalUrl.3>`).
* **Concatenated:** `<apiContent.concatenated>` and `<pageContent.concatenated>` join items as **`title\ntext`** blocks.

---

## 6) Basic Program ⇨ **Aggregated content variables** + **Convenience flags**

When you attach **API** and/or **Page** content to program, the app exposes **aggregated** variables and **convenience “enabled” flags**.

> **Indexing:** `.1`, `.2`, … targets a specific item.
> **Defaults:** Unindexed keys (e.g., `<apiContent.title>`) come from the **first** item.
> **Concatenated:** `<…concatenated>` joins items as `title\ntext` blocks.
> **Convenience flags (Basic‑only):** `apiContentEnabled`, `pageContentEnabled`, `newsEnabled`, `weatherEnabled`.

### Aggregated API Content (Basic Program)

* Conditional (Basic‑only convenience): `<#apiContentEnabled>` … `</apiContentEnabled>`

* Canonical (prompt‑visible, user‑defined in API Content Editor):
  * `<apiContent.title>`, `<apiContent.text>`
  * `<apiContent.image>`, `<apiContent.link>`
  * Indexed: `<apiContent.title.1>`, `<apiContent.text.1>`, `<apiContent.image.1>`, `<apiContent.link.1>` (then `.2`, …)
  * Concatenated: `<apiContent.concatenated>`

* **Your custom API arguments** (keys you add beyond the four above):
  * `<apiContent.author>`, `<apiContent.category>`, …
  * Indexed: `<apiContent.author.1>`, `<apiContent.author.2>`, …

### Aggregated Page Content (Basic Program)

* Conditional (Basic‑only convenience): `<#pageContentEnabled>` … `</pageContentEnabled>`

* Canonical (prompt‑visible, app‑defined):
  * `<pageContent.title>` (from `pageTitle`, fallback `title`)
  * `<pageContent.text>`  (from `content`)
  * `<pageContent.ogimage>`, `<pageContent.finalUrl>`
  * Indexed: `<pageContent.title.1>`, `<pageContent.text.1>`, `<pageContent.ogimage.1>`, `<pageContent.finalUrl.1>` (then `.2`, …)
  * Concatenated: `<pageContent.concatenated>`

* **Your custom Page arguments** (keys beyond the four above):
  * `<pageContent.author>`, `<pageContent.section>`, …
  * Indexed: `<pageContent.author.1>`, `<pageContent.section.2>`, …

### Program‑level News (Basic Program)

* Convenience conditional: `<#newsEnabled>` … `</newsEnabled>`
* Values: `<news.feedNames>`, `<news.headline>`

---

## 7) Custom Program ⇨ **Source‑specific variables** (no convenience flags)

In **Custom Program**, each segment composes from a single **source item**. Use `source.*` variables; there are **no “enabled” flags** here—use a **real variable as the condition**.

### Feed (Custom Program)

* `<source.feed.title>` — title in context
* `<source.feed.text>` — article description/body in context
* `<source.feed.headline>` — computed headline string

### API source (Custom Program — user‑defined values)

* **Core:** `<source.api.title>`, `<source.api.text>`
* **Display:** `<source.api.image>`, `<source.api.link>`
* **Your custom API arguments:** `<source.api.author>`, `<source.api.category>`, …

> **Reminder:** For API sources, you must define `title`, `text`, and (optionally) `image`, `link` in the **API Content Editor**.

### Page source (Custom Program — app‑defined values)

* **Core:** `<source.page.title>`, `<source.page.text>`
* **Display:** `<source.page.ogimage>`, `<source.page.finalUrl>`
* **Your custom Page arguments:** `<source.page.author>`, `<source.page.section>`, …

---

## 8) Custom prompt arguments (works in both program types)

Add **custom key–value pairs** (“arguments”) to your content items. They become variables automatically.

* **Basic Program (aggregated):**
  `<apiContent.author>`, `<apiContent.author.2>`, `<pageContent.section>`, `<pageContent.section.2>`, …
* **Custom Program (source‑specific):**
  `<source.api.author>`, `<source.api.category>`, `<source.page.author>`, `<source.page.section>`, …

**Tips**

* Keep keys simple: `author`, `category`, `publishedAt`, `toneHint`.
* Don’t redefine the **System‑recognized** keys for unrelated meanings:
  * API: `title`, `text`, `image`, `link` (user‑defined)
  * Page: `pageTitle`, `content`, `ogimage`, `finalUrl` (app‑defined)
* If you need different semantics, create your own keys (e.g., `imageForPrompt`, `canonicalLinkForPrompt`).

---

## 9) Conditional blocks — **how they really work**

Conditionals use the form:

```
<#SOME.KEY> … </SOME.KEY>
```

They **render the inside** only if the key’s value is **truthy**. Otherwise, the whole block is removed (including the tags).

**Use any variable as the condition.**
You’re not limited to “enabled” flags.

**Examples**

*Basic Program (either approach works):*

```text
<#apiContentEnabled>
API roundup:
<apiContent.concatenated>
</apiContentEnabled>
```

*or, using a real value as the switch:*

```text
<#apiContent.title>
Top API title: <apiContent.title>
</apiContent.title>
```

*Custom Program (no convenience flags—use a real value):*

```text
<#source.api.title>
API item: <source.api.title> — <source.api.text>
</source.api.title>
```

**Allowed characters in conditional names:** letters, digits, `.`, `_`, `-`
(Examples: `<#source.page.title>…</source.page.title>`, `<#pageContent.section>…</pageContent.section>`)

**Truthiness rules (case‑insensitive):**

* Treated as **false**: `""` (empty), `false`, `no`, `0`, `off`, `disabled`
* Any other **non‑empty** string is **true**

**Notes**

* Conditionals **don’t** support inline defaults inside the `<#…>` tag. Use defaults inside the content placeholders instead, e.g., `<source.api.title|n/a>`.
* You **can nest** conditionals. Just make sure tags are properly paired.

---

## 10) Inline defaults—avoid blank spots

If some data might be missing, add a default **inside** the placeholder:

```
Weather: <weather.condition|n/a>, <weather.temperature|-->°C
```

* The text after `|` is used when the value is missing.
* The editor also collapses extra blank lines.

---

## 11) Ready‑to‑use templates (pick **Basic** or **Custom**)

### A. Duo Talk — **Basic Program**

```
You are co‑hosts <personality.djName> and <personality.assistantName>. Keep it lively but concise.

<#newsEnabled>
Open with one upbeat line about today’s headlines from <news.feedNames>.
Then pick 1–2 headlines to react to (30–40 words total).
</newsEnabled>

<#weatherEnabled>
Add a quick weather hit: <weather.condition|->, <weather.temperature|-->° now.
</weatherEnabled>

<#apiContentEnabled>
Key takeaways from API sources:
<apiContent.concatenated>
</apiContentEnabled>

<#pageContentEnabled>
From today’s pages:
<pageContent.concatenated>
</pageContentEnabled>

<#apiContent.image>
If useful, mention our visual: <apiContent.image>
</apiContent.image>

<#pageContent.finalUrl>
Point listeners to the canonical link: <pageContent.finalUrl>
</pageContent.finalUrl>

Close with a handoff to the next segment.

Ensure the result contains only dialogue. Prefix each line with the speaker’s name.

<personality.djName>: Hello everyone!
<personality.assistantName>: Hi everyone!
```

### B. Duo Talk — **Custom Program**

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

<#source.api.link>
If relevant, mention the link: <source.api.link>
</source.api.link>

<#source.page.ogimage>
Optional visual reference: <source.page.ogimage>
</source.page.ogimage>

Close with a handoff to the next segment.

Ensure the result contains only dialogue. Prefix each line with the speaker’s name.

<personality.djName>: Hello everyone!
<personality.assistantName>: Hi everyone!
```

### C. Solo Talk — **Basic Program**

```
Voice: <personality.djName>. Style: warm, clear, 2–3 short paragraphs.

<#pageContentEnabled>
Summarize the page content in plain language:
<pageContent.concatenated>
</pageContentEnabled>

<#apiContentEnabled>
If API content exists, give 2 bullet points:
- <apiContent.concatenated>
</apiContentEnabled>

<#apiContent.link>
If a link exists, include it at the end: <apiContent.link>
</apiContent.link>

<#weatherEnabled>
Weather flash: <weather.condition|n/a>, <weather.temperature|-->°.
</weatherEnabled>

Ensure the result is only dialogue text (prefix each line with the speaker’s name).

<personality.djName>: Hello everyone!
```

### D. Solo Talk — **Custom Program**

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

<#source.page.finalUrl>
For details, see: <source.page.finalUrl>
</source.page.finalUrl>

<#weather.temperature>
Weather flash: <weather.condition|n/a>, <weather.temperature|-->°.
</weather.temperature>

Ensure the result is only dialogue text (prefix each line with the speaker’s name).

<personality.djName>: Hello everyone!
```

### E. System (works for both)

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

## 12) Tips & good practices

* **Pick placeholders for your program type.**
  **Basic:** aggregated (`apiContent.*`, `pageContent.*`, `news.*`) + convenience flags (`*Enabled`).
  **Custom:** source‑specific (`source.api.*`, `source.page.*`, `source.feed.*`, music `source.*`). Use variables as conditions.

* **System‑recognized keys are prompt‑visible.**
  * **API (user‑defined in the API Content Editor):** `title`, `text`, `image`, `link`
  * **Page (app‑defined):** `pageTitle`, `content`, `ogimage`, `finalUrl`

* **Prefer `.concatenated` (Basic)** for multiple items. Use indexing (`.1`, `.2`, …) to target a specific item.

* **Use real variables as conditions** (especially in Custom). Examples: `<#source.api.title>…</source.api.title>`, `<#pageContent.title>…</pageContent.title>`.

* **Use inline defaults** for anything that might be missing (`<x|n/a>`).

* **Add custom prompt arguments** for your own metadata (`author`, `category`, `toneHint`):
  * **Basic:** `<apiContent.author>`, `<pageContent.section.2>`
  * **Custom:** `<source.api.author>`, `<source.page.section>`

* **Avoid key collisions.** Don’t repurpose `title`, `text`, `image`, `link`, `pageTitle`, `content`, `ogimage`, `finalUrl` for unrelated meanings. Use your own keys if you need different semantics (e.g., `heroImage`, `canonicalLinkForPrompt`).

---

## 13) Troubleshooting

* **My placeholder prints literally.**
  Check your program type:

  * **Basic:** use `<apiContent.*>` / `<pageContent.*>` / `<news.*>`, not `source.*`.
  * **Custom:** use `source.*`, not the aggregated forms.
    Add a default: `<…|n/a>`.

* **My conditional doesn’t show.**
  Remember: conditionals use **any key’s truthiness**, not only “enabled” flags.
  Try: `<#source.api.title>…</source.api.title>` (Custom) or `<#apiContent.title>…</apiContent.title>` (Basic).

* **I don’t see some placeholders in the Insert menu.**
  The menu adapts to your program type and sources. You can always type advanced keys (e.g., `<weather.locationName>`, `<apiContent.author>`) manually.

* **Image/link/OG placeholders are empty.**
  * **API items:** Ensure you added `image` and/or `link` in the **API Content Editor** (Prompt Arguments).
  * **Page items:** `ogimage` and `finalUrl` are app‑defined; make sure the Page Content actually provides them.

* **Which placeholders should I use, Basic vs Custom?**
  * **Basic (aggregated):** `<apiContent.*>`, `<pageContent.*>`
  * **Custom (single source item):** `<source.api.*>`, `<source.page.*>`

---

## 14) How the engine renders your prompt (for power users)

This mirrors actual behavior—useful if you’re crafting complex templates.

1. **Conditionals first**
   Blocks like `<#some.key>…</some.key>` are evaluated before anything else.

   * If `some.key` resolves to a **truthy** value, the block **keeps** its inner content and **removes** the tags.
   * If it’s missing or **falsey**, the **entire block is removed**.
   * **Truthiness:** empty/`false`/`no`/`0`/`off`/`disabled` → false; any other non‑empty string → true.
   * You can **nest** conditionals. Tag names allow letters, digits, `.`, `_`, `-`.

2. **Placeholders next**
   Every `<key>` is replaced with its value. If missing:

   * If you wrote a default (`<key|Default>`), the default is used.
   * Otherwise, it becomes **empty** (the default replacement is an empty string).

3. **Cleanup**

   * **Blank lines collapse** to a single blank line.
   * (Advanced) If a special “n/a” replacement is configured, lines that reduce to “n/a” plus punctuation may be removed. By default, missing values just become empty strings.

**What to remember:**

* In **Basic Program**, “enabled” flags are just a **convenience**.
* In **Custom Program**, simply **use variables themselves** as conditions.
* **All System‑recognized keys (Core + Display)** are prompt‑visible:

  * API (user‑defined): `title`, `text`, `image`, `link`
  * Page (app‑defined): `pageTitle`, `content`, `ogimage`, `finalUrl`

---

## 15) One‑page cheat sheet

**Basic Program (Aggregated + convenience flags)**

```
Conditional:     <#apiContentEnabled>…</apiContentEnabled>
API (agg):       <apiContent.title>, <apiContent.text>, <apiContent.image>, <apiContent.link>
API index:       <apiContent.title.2>, <apiContent.text.2>, <apiContent.image.2>, <apiContent.link.2>
API concat:      <apiContent.concatenated>
API custom:      <apiContent.author>, <apiContent.author.2>

Page (agg):      <pageContent.title>, <pageContent.text>, <pageContent.ogimage>, <pageContent.finalUrl>
Page index:      <pageContent.title.2>, <pageContent.text.2>, <pageContent.ogimage.2>, <pageContent.finalUrl.2>
Page concat:     <pageContent.concatenated>
Page custom:     <pageContent.section>, <pageContent.section.2>

News:            <#newsEnabled><news.headline></newsEnabled>
Weather:         <#weatherEnabled><weather.condition>, <weather.temperature>°</weatherEnabled>
```

**Custom Program (Source‑specific, use variables as conditions)**

```
Feed:            <#source.feed.title><source.feed.title> — <source.feed.text></source.feed.title>
API:             <#source.api.title><source.api.title> — <source.api.text></source.api.title>
API display:     <#source.api.image><source.api.image></source.api.image>  <#source.api.link><source.api.link></source.api.link>
API custom:      <source.api.author>, <source.api.category>

Page:            <#source.page.title><source.page.title> — <source.page.text></source.page.title>
Page display:    <#source.page.ogimage><source.page.ogimage></source.page.ogimage>  <#source.page.finalUrl><source.page.finalUrl></source.page.finalUrl>
Page custom:     <source.page.author>, <source.page.section>

Music:           <source.songTitle> by <source.artistName>
Weather:         <#weather.temperature><weather.condition>, <weather.temperature>°</weather.temperature>
```

---

### Appendix — API Content Editor quick guide (for the four system‑recognized API keys)

* Open **API Content → Prompt Arguments**.
* Add the following **keys** (your values may come from JSON paths or constants):

  * `title` – short headline/title (required for canonical placeholders).
  * `text`  – body/summary (required for canonical placeholders).
  * `image` – URL for artwork/thumbnail (optional).
  * `link`  – URL for “open” action (optional).
* These appear in prompts as:

  * **Basic:** `<apiContent.title>`, `<apiContent.text>`, `<apiContent.image>`, `<apiContent.link>` (plus indexed forms).
  * **Custom:** `<source.api.title>`, `<source.api.text>`, `<source.api.image>`, `<source.api.link>`.

For **Page Content**, the app automatically provides:

* `pageTitle`, `content`, `ogimage`, `finalUrl` → usable as

  * **Basic:** `<pageContent.title>`, `<pageContent.text>`, `<pageContent.ogimage>`, `<pageContent.finalUrl>`
  * **Custom:** `<source.page.title>`, `<source.page.text>`, `<source.page.ogimage>`, `<source.page.finalUrl>`

