# Edit Prompts for your scripting

Prompts tell the app **how to talk**. You can write your own script instructions and drop in live data (program name, weather, news, content you loaded, etc.) using simple **placeholders** like `<program.name>`.

This guide covers:

* Choosing a **Prompt Type** (System / User / Duo / Solo / Multilang)
* Writing **Prompt Content**
* Inserting **placeholders** and **conditional blocks**
* A complete **placeholder reference**
* Ready‑to‑use **templates**
* Tips and troubleshooting

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

* **System** – Invisible “rules” and style for the model (tone, do/don’t, persona notes).
* **User** – The main instructions/content for this segment.
* **Duo Talk / Duo Talk Multilang** – Dialogue format for **two speakers**.
* **Solo Talk / Solo Talk Multilang** – Monologue format for **one speaker**.
* **Multilang** variants are for non‑English programs.

**How the app chooses at runtime**

* **Duo casting:** it prefers **Duo Talk** (or **Duo Talk Multilang** if your program language isn’t English). If those aren’t present, it falls back to **User**.
* **Solo casting:** it prefers **Solo Talk** (or **Solo Talk Multilang**), otherwise **User**.

---

## 3) Write your Prompt Content

Type normal text. Wherever you want live data, insert a **placeholder** (e.g., `<program.name>`). Use the **Insert Placeholder** button so you don’t have to memorize names.

As you type, the editor:

* **Highlights placeholders** (colored text so you can spot them easily).
* Keeps your cursor where you expect it.
* Inserts **opening + closing** tags for conditional blocks for you.

**Highlight legend** (approximate colors):

* Placeholders: **blue**
* Conditional tags (like `<#weatherEnabled>…</weatherEnabled>`): **peach**
* Credentials tokens (advanced): **pink**

---

## 4) Placeholders 101

* **Basic placeholders** look like `<something.like.this>`.

* **Conditional blocks** show content **only when the data exists or is enabled**:

  ```
  <#weatherEnabled>
  Today’s weather: <weather.temperature>°, <weather.condition>.
  </weatherEnabled>
  ```

  Use the **Insert Placeholder** menu to add these—when you insert a conditional opener (like `<#weatherEnabled>`), the editor **automatically adds the matching closing tag** (`</weatherEnabled>`).

* **Inline defaults** keep your prompt tidy when data is missing:

  ```
  Now playing: <source.songTitle|Unknown title>
  ```

  If `source.songTitle` is missing, the text after the `|` is used.

---

## 5) Quick placeholder reference

> You’ll see these grouped under **Insert Placeholder**. Some appear only when your segment’s **Source** is set (Feed, API, Page, Music).

### Program

* `<program.name>` – Your program title
* `<program.generationTime>` – When the segment was generated
* `<program.lang>` – Program language (e.g., “english”)

### Weather  *(appears only if weather is available)*

* `<#weatherEnabled>` … `</weatherEnabled>` – Wrap weather‑specific lines
* `<weather.temperature>` – Number (e.g., 23.4)
* `<weather.condition>` – Short description (e.g., “Cloudy”)
* `<weather.forecast>` – Combined forecast text

  > **Advanced:** You can also type `<weather.locationName>` manually.

### News  *(appears only if headlines exist)*

Program level content available in Basic Program.

* `<#newsEnabled>` … `</newsEnabled>`
* `<news.feedNames>` – Source feed names
* `<news.headline>` – Headlines, usually one per line

### Aggregated API Content

Program level content available in Basic Program.

* `<#apiContentEnabled>` … `</apiContentEnabled>`
* `<apiContent.concatenated>` – All API text joined together

  > **Note:** `<apiContent.text>` may be available if there’s exactly one item; otherwise prefer `.concatenated`.

### Aggregated Page Content

*Program level content available in Basic Program.*

* `<#pageContentEnabled>` … `</pageContentEnabled>`
* `<pageContent.concatenated>` – All page content joined

  > **Note:** `<pageContent.text>` may exist when a single item is present.

### Personality

* `<personality.djName>`
* `<personality.assistantName>`
* `<personality.djDescription>`
* `<personality.assistantDescription>`
* `<personality.newsCasterName>`
* `<personality.newsCasterDescription>`

### Source‑specific (depends on the segment’s **Source**)

*Segment level content available in Custom Program*

* **Feed**
  * `<source.feedText>` – Joined article descriptions, or single article description if “Compose Individually“. 
  * *(Advanced extras you can type manually)*: `<source.feedTitle>`, `<source.feedHeadline>`
* **API Text**
  * `<source.apiTitle>`, `<source.apiText>`
* **API Page**
  * `<source.pageTitle>`, `<source.pageText>`
* **Music**
  * `<source.songTitle>`, `<source.albumTitle>`, `<source.artistName>`

---

## 6) Conditional blocks—when do they show?

Wrap optional sections like this:

```
<#newsEnabled>
Top stories from <news.feedNames>:
<news.headline>
</newsEnabled>
```

These show only when the matching “Enabled” flag is **truthy**:

* `weatherEnabled` – Set automatically when current weather or a forecast is available.
* `newsEnabled` – Set when headlines are available.
* `apiContentEnabled` – Set when the app has API content for this segment.
* `pageContentEnabled` – Set when page content is available.

> **Tip:** The Insert menu adds the pair of tags for you. If you type by hand, make sure your closing tag **exactly** matches the opener (e.g., `</weatherEnabled>`).

---

## 7) Inline defaults—avoid blank spots

If some data might be missing, add a default **inside** the placeholder:

```
Weather: <weather.condition|n/a>, <weather.temperature|-->°C
```

* The text after `|` is used when the value is missing.
* The editor also reduces extra blank lines automatically, so your prompt stays neat.

---

## 8) Ready‑to‑use templates

### A. Duo Talk (English)

Use with **Duo Talk** prompt type.

```
You are co‑hosts <personality.djName> and <personality.assistantName>. Keep it lively but concise.

<#newsEnabled>
Open with one upbeat line about today’s headlines from <news.feedNames>.
Then pick 1–2 headlines to react to (30–40 words total).
</newsEnabled>

<#weatherEnabled>
Add a quick weather hit: <weather.condition|->, <weather.temperature|-->° now.
</weatherEnabled>

If there’s content for this segment, weave it in naturally:
<#apiContentEnabled>
Key takeaways from today’s source:
<apiContent.concatenated>
</apiContentEnabled>

Close with a handoff to the next segment.

Ensure that the result contains only dialogue text. No extra briefing, and no non-dialogue text such as "Here is the closing dialogue:" or "(The program ends)" must be included.
And also, ensure that each line of dialogue must be prefixed with the speaker's name, as shown below. (Note that the script is an example; feel free to start with any catchy words.)

<personality.djName>: Hello everyone!
<personality.assistantName>: Hi everyone!
```

**NOTE:** Ensure prompting match to the *Parsing Style* of the segment.

### B. Solo Talk (Multilang‑friendly)

Use with **Solo Talk** or **Solo Talk Multilang**.

```
Voice: <personality.djName>.
Style: warm, clear, 2–3 short paragraphs.

<#pageContentEnabled>
Summarize the page content in plain language:
<pageContent.concatenated>
</pageContentEnabled>

<#apiContentEnabled>
If API content exists, give 2 bullet points:
- <apiContent.concatenated>
</apiContentEnabled>

<#weatherEnabled>
Weather flash: <weather.condition|n/a>, <weather.temperature|-->°.
</weatherEnabled>

Ensure that the result contains only dialogue text. No extra briefing, and no non-dialogue text such as "Here is the closing dialogue:" or "(The program ends)" must be included.
And also, ensure that each line of talk must be prefixed with the speaker's name, as shown below. (Note that the script is an example; feel free to start with any catchy words.)

<personality.djName>: Hello everyone!
```

**NOTE:** Also ensure prompting match to the *Parsing Style* of the segment.

### C. System (applies to Duo or Solo)

Use with **System**.

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

## 9) Tips & good practices

* **Start broad, then specialize.** Put tone and safety rules in **System**, content flow in **User/Duo/Solo**.
* **Prefer `.concatenated`** when handling multiple items (API/Page content).
* **Keep conditionals small.** Wrap only the lines that need to be optional.
* **Use inline defaults** for anything that might be missing.
* **Music segments:** Reference `<source.songTitle>`, `<source.artistName>` for natural intros.

---

## 10) Troubleshooting

* **My placeholder shows literally (e.g., `<weather.temperature>`).**
  That value might not exist for this segment. Add a default: `<weather.temperature|-->` or wrap the whole bit in `<#weatherEnabled>…</weatherEnabled>`.

* **I don’t see certain placeholders in the menu.**
  Some are **source‑specific** (e.g., Feed vs Page). Also, a few advanced keys (like `<weather.locationName>`) exist even if they’re not listed—typing them manually is OK.

* **I can’t change the Prompt Type.**
  You’re editing an existing prompt; type changes are disabled for safety. Create a new prompt of the desired type if it’s still unused.

---

## 11) One‑page cheat sheet

```
Text with data:  Hello, this is <program.name>.
Conditional:     <#newsEnabled>…</newsEnabled>
Inline default:  <weather.temperature|-->
Feed (source):   <source.feedText>
API (source):    <source.apiTitle> — <source.apiText>
Page (source):   <source.pageTitle> — <source.pageText>
Music (source):  <source.songTitle> by <source.artistName>
Aggregated API:  <#apiContentEnabled><apiContent.concatenated></apiContentEnabled>
Aggregated Page: <#pageContentEnabled><pageContent.concatenated></pageContentEnabled>
Personality:     <personality.djName>, <personality.assistantName>
Weather:         <#weatherEnabled><weather.condition>, <weather.temperature>°</weatherEnabled>
News:            <#newsEnabled><news.headline></newsEnabled>
```

---

### Final notes

* You can **nest** different conditional blocks (e.g., a weather line inside an API section), but don’t overlap mismatched tags.
* Most segments will “just work” if you:

  1. Pick the correct **Prompt Type**,
  2. Insert placeholders via the **Insert Placeholder** menu, and
  3. Use **inline defaults** for optional values.

