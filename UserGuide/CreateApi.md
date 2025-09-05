# Create and Edit API Templates with Placeholders

*A guide to prompts, credentials, date/location context, for your Generativ AI, API and Page Content and Catalog setting*

This feature lets you design simple **API templates** that include placeholders. When you run a request, the app fills those placeholders with your runtime data, your saved credentials (API keys), today’s date, and—if you allow it—your current location.

---

## What you can do

* **Write once, reuse forever:** Save an API request template with placeholders.
* **Drop in your prompts:** Fill `<userPrompt>`, `<systemPrompt>`, `<positivePrompt>`, and `<negativePrompt>` just before sending.
* **Auto‑insert credentials:** Use `<credentials:Name>` to inject the API key you’ve saved under “Name”.
* **Use today’s date & your location:** Place `<yyyy>`, `<mm>`, `<dd>`, `<lat>`, `<lon>` anywhere in the endpoint or JSON body.

---

## Quick start

1. **Create a template**

   * Add placeholders where you want dynamic values:

     * Prompts: `<userPrompt>`, `<systemPrompt>`, `<positivePrompt>`, `<negativePrompt>`
     * Credentials: `<credentials:MyServiceKey>`
     * Date: `<yyyy>`, `<mm>`, `<dd>`
     * Location: `<lat>`, `<lon>`
   * Example endpoint and body:

     ```
     Endpoint:
     https://api.example.com/v1/search?on=<yyyy>-<mm>-<dd>&lat=<lat>&lon=<lon>

     Headers:
     Authorization: Bearer <credentials:ExampleKey>

     Body:
     {
       "system": "<systemPrompt>",
       "user": "<userPrompt>",
       "filters": { "near": "<lat>,<lon>" }
     }
     ```

2. **Save your credential**

   * In Settings → Credentials, save your API key with the exact **Name** used in your placeholder (e.g., `ExampleKey`).
   * You’ll reference it as `<credentials:ExampleKey>`.

3. **Run the template**

   * Enter your prompts (you can provide any subset—user, system, positive, negative).
   * If grant **Location** permission, `<lat>` and `<lon>` can be filled.
   * Send the request.

---

## Placeholders you can use

### Prompts (request body only)

* `<userPrompt>`
* `<systemPrompt>`
* `<positivePrompt>`
* `<negativePrompt>`

> These are replaced everywhere they appear inside the JSON body strings.

### Credentials (headers and body)

* `<credentials:Name>`

  * **Name** can contain letters and digits only (A–Z, a–z, 0–9).
  * Example: `Authorization: Bearer <credentials:ExampleKey>`
  * Make sure you’ve saved a credential with the exact name `ExampleKey`.

> Credentials are replaced inside header values and anywhere in the body strings. (They are **not** used in the endpoint URL.)

### Date (endpoint and body)

* `<yyyy>` – 4‑digit year (e.g., `2025`)
* `<mm>` – 2‑digit month (e.g., `08`)
* `<dd>` – 2‑digit day (e.g., `30`)

### Location (endpoint and body)

* `<lat>` – latitude from your device (if permission granted)
* `<lon>` – longitude from your device (if permission granted)

> If location isn’t available or permission is denied, `<lat>`/`<lon>` are n/a. 

---

## Example: before → after

**Before (your template):**

```
Endpoint:
https://api.example.com/search?on=<yyyy>-<mm>-<dd>&lat=<lat>&lon=<lon>

Headers:
Authorization: Bearer <credentials:WeatherKey>

Body:
{
  "system": "<systemPrompt>",
  "user": "<userPrompt>",
  "radiusKm": 25.0,
  "location": { "lat": "<lat>", "lon": "<lon>" }
}
```

**After (what the app sends on Aug 30, 2025 in Paris with saved key):**

```
Endpoint:
https://api.example.com/search?on=2025-08-30&lat=48.8566&lon=2.3522

Headers:
Authorization: Bearer sk-live-••••••••

Body (pretty-printed):
{
  "system" : "You are a nice script writer.",
  "user" : "Find outdoor events today, and create talk between DJ and sidekick.",
  "radiusKm" : 25,
  "location" : {
    "lat" : "48.8566",
    "lon" : "2.3522"
  }
}
```

---

## How replacement works (so you don’t get surprised)

* **Recursive & thorough:** Placeholders are searched and replaced everywhere inside strings in your JSON body **and** in the endpoint URL (for date and location).
* **Case‑sensitive:** `<userPrompt>` is not the same as `<UserPrompt>`.
* **Exact tokens:** Include the angle brackets exactly as shown, e.g., `<yyyy>`, not `yyyy`.
* **Where each type works:**
  * **Prompts:** body only
  * **Credentials:** headers and body
  * **Date & Location:** body and endpoint
* **Location permission:** If denied or unavailable, `<lat>`/`<lon>` remain unreplaced.
* **Endpoint credentials:** Credentials are **not** read or replaced in the endpoint; put them in headers or the body.

---

## Best practices

* **Name your credentials simply:** Use letters/numbers only (e.g., `ServiceAIKey`, `MyMaps2`).
* **Keep secrets in headers:** Prefer `Authorization: Bearer <credentials:Name>` rather than placing secrets in URLs.
* **Avoid partial tokens:** Don’t wrap placeholders inside other characters (e.g., `id-<yyyy>` is fine, but the placeholder must appear exactly as shown).
* **Validate your JSON:** If your body isn’t valid JSON, replacements can’t run.
* **Grant location only if needed:** If your template doesn’t use `<lat>/<lon>`, you can deny the permission.

---

## FAQ

**Do I have to use all four prompt placeholders?**
No. Provide any subset. The app will only replace what you use.

**Can I use placeholders inside arrays or deeply nested objects?**
Yes. Replacement is recursive: it works at any depth in your JSON.

**Can I put credentials in the endpoint URL?**
Not supported. Credentials are replaced in headers and body only.

**What date/time is used?**
The device’s current date (year, month, day) at the moment you run the template.

---

## Example templates you can copy

**1) Text generation**

```
Endpoint:
https://api.example.ai/v1/chat

Headers:
Authorization: Bearer <credentials:GenAIKey>
Content-Type: application/json

Body:
{
  "system": "<systemPrompt>",
  "messages": [
    { "role": "user", "content": "<userPrompt>" }
  ]
}
```

**2) Weather near me today**

```
Endpoint:
https://api.weather.example/v2/forecast?date=<yyyy>-<mm>-<dd>&lat=<lat>&lon=<lon>

Headers:
Authorization: Bearer <credentials:WeatherKey>

Body:
{
  "units": "metric",
  "notes": "User asked: <userPrompt>"
}
```

**3) Search with date filter**

```
Endpoint:
https://api.search.example/v1/query?on=<yyyy>-<mm>-<dd>

Headers:
X-API-Key: <credentials:SearchKey>

Body:
{
  "q": "<userPrompt>",
  "boost": "<positivePrompt>"
}
```

---

## Safety & privacy

* **Credentials** are read from your device’s Keychain just before a request is built and are never shown in logs.
* **Location** is optional and only read if you’ve granted permission. You can revoke it at any time in system settings.

---

*That’s it! Save a template for your Generative AI, API/Page Content or Catalog. Keep your API keys in Credentials, and let the app do the rest.*
