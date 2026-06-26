---
name: dialora-linkedin-full
description: >
  Turn any Dialora blog post or source content into a complete LinkedIn deliverable: a rephrased
  article, a caption, and a branded 2400x1348 PNG banner. Trigger on any combination of: "linkedin
  article for dialora", "turn this blog into a linkedin article", "linkedin banner for dialora",
  "give the banner", "caption for dialora", "rephrase this for linkedin", or simply "give me the article".
  Run all three by default unless the user asks for a subset. Never stop mid-flow for approval.
---

# Dialora LinkedIn: Article + Caption + Banner

Three deliverables from one source. Produce only what is asked (default: all three). Run end to end. Never ask for confirmation between steps.

---

## Writing rules (article + caption)

- **Source-only.** Use only facts from the content provided. No external stats, no added claims. Trust the source's numbers; just reword them. Add only framing, transitions, the hook, and CTAs.
- American spelling. No em dashes. No emojis in the article body (caption gets exactly one: 👇).
- Short sentences. Direct B2B voice. Not stiff, not hypey.
- **Format**: mix of short paragraphs and bullets. Never wall-to-wall paragraphs. Use bullets where the source has a list, steps, or enumerable benefits. Use short prose for narrative context and story moments. Every section ends with a punchy single line to carry the reader forward.

---

## 1. Article

Save to `/mnt/user-data/outputs/dialora_linkedin_article_<topic>.md`.

- **Title**: rephrased, not the blog's exact headline. Keyword up front. Simple and straight.
- **First line**: Unicode bold hook — always (see Unicode bold below).
- **Body**: re-angle the source; do not reorder it 1:1. Short paras + bullets mixed. Bold subheads for each section. Bullets only where the source is genuinely a list, steps, or multiple items.
- **Length**: 700–800 words. Verify with `wc -w`.
- **CTAs** (end of article):
  ```
  👉 Try Dialora for free: https://www.dialora.ai/signup

  📖 Read the full blog to know more: {{BLOG_URL}}
  ```
  Leave `{{BLOG_URL}}` as placeholder unless user provides the live URL.

---

## 2. Caption

Present inline. Format:

```
𝗨𝗻𝗶𝗰𝗼𝗱𝗲 𝗯𝗼𝗹𝗱 𝗵𝗼𝗼𝗸 𝗹𝗶𝗻𝗲.

Short line. Short line.

One compelling line, read the full article 👇

#AIVoiceAgent #VoiceAI #DialoraAI #Tag4 #Tag5
```

- Hook: one curiosity-gap line tied to the article's most intriguing point. Always Unicode bold.
- CTA must end with "read the full article" + 👇.
- Hashtags: must include `#AIVoiceAgent`, `#VoiceAI`, `#DialoraAI` plus 2–3 topic-relevant tags.

### Unicode bold

```python
def ubold(s):
    out = []
    for c in s:
        o = ord(c)
        if 65 <= o <= 90:    out.append(chr(0x1D5D4 + (o - 65)))  # A-Z
        elif 97 <= o <= 122: out.append(chr(0x1D5EE + (o - 97)))  # a-z
        elif 48 <= o <= 57:  out.append(chr(0x1D7EC + (o - 48)))  # 0-9
        else:                out.append(c)
    return "".join(out)
```

---

## 3. Banner

**Specs (locked): 2400 × 1348 px, PNG only. Never JPG.**

### Visual system (locked — never deviate)

| Token | Value |
|---|---|
| Background | `#060d1e` |
| Dot grid | `#3a6ea8` 1.2px dots, 44px grid, opacity 0.10 — left zone only |
| Brand blue | `#1976D2` |
| Accent blue | `#42A5F5` |
| Font | Lato Black 900 (headline), Bold 700 (subline) |
| Logo | `assets/dialora-logo.png` — top-left, 220px wide, single instance only |
| Website | `dialora.ai` — bottom-right only, 36px, ~42% white opacity |

### Layout — text LEFT, photo RIGHT (default for all banners)

```css
/* Logo: top-left */
.logo { position: absolute; top: 80px; left: 90px; width: 220px; z-index: 5; }

/* Dot grid: left zone */
.dot-grid {
  position: absolute; left: 0; top: 0; width: 1100px; height: 1348px;
  background-image: radial-gradient(circle, rgba(58,110,168,0.10) 1.2px, transparent 1.2px);
  background-size: 44px 44px; z-index: 0;
}

/* Photo: right panel — use raw image as-is, NO mirror */
.photo {
  position: absolute; right: 0; top: 0;
  width: 1340px; height: 1348px;
  background-size: cover; background-position: center top;
  mask-image: linear-gradient(to right,
    transparent 0%, rgba(0,0,0,0.0) 4%,
    rgba(0,0,0,0.20) 14%, rgba(0,0,0,0.65) 26%, black 40%);
  /* NO transform: scaleX(-1) — face points RIGHT, away from text. This is correct. */
}

/* Veil: keeps left text zone dark */
.veil {
  position: absolute; inset: 0;
  background: linear-gradient(to right,
    #060d1e 0%, #060d1e 32%,
    rgba(6,13,30,0.70) 46%, rgba(6,13,30,0.0) 64%);
  z-index: 1;
}

/* Text: vertically centered, left-aligned */
.text { position: absolute; left: 90px; top: 50%; transform: translateY(-50%); max-width: 980px; z-index: 3; }

/* Headline */
.headline { font-weight: 900; font-size: 152px; line-height: 1.03; letter-spacing: -2px; color: #FFFFFF; }
.headline .blue { color: #42A5F5; }  /* ONE blue span only */

/* Blue rule */
.rule { width: 86px; height: 5px; background: #1976D2; margin: 44px 0 38px 0; }

/* Subline */
.subline { font-weight: 700; font-size: 50px; line-height: 1.42; color: #FFFFFF; }

/* Watermark: bottom-right */
.watermark { position: absolute; bottom: 54px; right: 72px; font-size: 36px; color: rgba(255,255,255,0.42); z-index: 5; }
```

**No card box. No rounded container. Text sits directly on dark veil zone.**

---

### Subject direction — THE CORRECT WORKFLOW (CRITICAL)

**The goal:** Subject on RIGHT side of canvas, face pointing RIGHT (away from text). This is the preferred aesthetic.

**Freepik Mystic generates subjects facing RIGHT by default — use the image as-is. Do NOT apply any mirror or flip.**

**Do NOT:**
- Apply `transform: scaleX(-1)` on the `.photo` div — this flips the face to point left, which is wrong
- Use Pillow to flip the image
- Prompt Mystic for "facing left"

**Always:** Generate with Mystic → download as-is → composite without any transform → face points right. ✓

---

### Hero image — content analysis first (CRITICAL)

Read the article before choosing a subject. The image must map to the specific idea in the article.

**Do NOT default to:** headset agent, person staring at screen, generic call center imagery.

**DO this instead:**
1. Identify the core tension or transformation in the article.
2. Map it to a concrete real-world moment.
3. Choose a subject the target audience would associate with that specific moment.

**Examples:**

| Article core idea | Wrong image | Right image |
|---|---|---|
| Law firm missing after-hours intake calls | Generic businessman with tablet | Attorney at desk late at night reviewing case intake documents under desk lamp |
| Voice AI + CRM gap | Headset person | Professional mid-call, data disappearing |
| Automated calls for SMBs | Generic call center | Confident ops person running the operation |
| Tech comparison (Vapi vs ElevenLabs) | Dashboard screenshot | Developer reviewing code architecture |

**Variety rule:** Not every banner gets a headset person. Vary across banners: attorney, developer, ops manager, business owner, abstract concept.

---

### Hero image — Freepik Mystic generation

```
freepik:create_image_mystic → poll freepik:get_mystic_task_status until COMPLETED
```

- `model`: `realism`
- `aspect_ratio`: `social_post_4_5`
- `resolution`: `2k`
- `creative_detailing`: `22`
- `styling.colors`: `[{"color":"#0B1A33","weight":0.4},{"color":"#1976D2","weight":0.15}]`
- `negative_prompt`: `floating particles, dots, debris, dripping streaks, sparks, glitch, text, words, letters, logo, watermark, deformed face, extra fingers, distorted hands, clutter, multiple subjects, off-brand colors, white background`
- Prompt shape: `"ONE subject, editorial photograph for a premium technology magazine, [SPECIFIC SUBJECT TIED TO ARTICLE], upper body shot, subject positioned on the right side of the frame, soft cinematic cool blue lighting, dark deep navy background, generous dark negative space on the left third of the image, no text, no logos, no brand marks"`
- **Do NOT include "facing left" in the prompt** — Mystic ignores it. Mirror via CSS instead.
- Download: `curl -sS -L -o hero.png "<url>"`

---

### Hero image — checklist (run before compositing)

- [ ] Subject maps to the article's specific core moment (not generic voice AI imagery)
- [ ] Image downloaded
- [ ] `.photo` div has NO transform applied — raw image used as-is
- [ ] One hero subject, no collage
- [ ] Editorial, magazine-grade feel
- [ ] No spillage (no particles, streaks, sparks, debris)
- [ ] Dark negative space on left third of source image
- [ ] No readable text or third-party logos in the art
- [ ] Final PNG viewed — face points RIGHT, text not obscured, logo top-left (single), watermark bottom-right

---

### Composite + render (all assets embedded as base64)

```python
import base64
from playwright.sync_api import sync_playwright

def b64(p):
    with open(p,'rb') as f: return base64.b64encode(f.read()).decode()

lato_black = b64('/home/claude/Lato-Black.ttf')
lato_bold  = b64('/home/claude/Lato-Bold.ttf')
logo       = b64('/home/claude/dialora-logo.png')
hero       = b64('/home/claude/hero.png')  # raw download — mirror applied via CSS scaleX(-1)

# Build HTML with @font-face data URIs, all assets base64
# .photo div MUST have transform: scaleX(-1)
# Render at 2400x1348 with Playwright chromium

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page(viewport={'width': 2400, 'height': 1348})
    page.goto('file:///home/claude/banner.html')
    page.wait_for_timeout(2000)
    page.screenshot(path='/mnt/user-data/outputs/dialora_banner_<topic>.png',
                    clip={'x':0,'y':0,'width':2400,'height':1348})
    browser.close()
```

Font + asset paths (copy to `/home/claude/` before use):
- `/mnt/skills/user/dialora-carousel/assets/fonts/Lato-Black.ttf`
- `/mnt/skills/user/dialora-carousel/assets/fonts/Lato-Bold.ttf`
- `/mnt/skills/user/dialora-carousel/assets/dialora-logo.png`

**Dependencies (install once):**
```bash
pip install playwright pillow --break-system-packages
playwright install chromium
```

---

## Workflow order

1. Identify deliverables (article / caption / banner). Default: all three. Never ask.
2. Read source. Extract only its facts.
3. **Article**: title → Unicode bold hook → rephrased body (mixed paras + bullets) → CTAs → `wc -w` check.
4. **Caption**: Unicode bold hook → 2 short lines → CTA with 👇 → hashtags.
5. **Banner**:
   - Read article → identify core tension → choose subject tied to that tension (not generic)
   - Generate image with Mystic (do NOT prompt for facing direction)
   - Download hero.png as-is
   - Composite (base64 embed, NO transform on .photo div) → render → view PNG → confirm face points right → `present_files`

---

## Hard rules (never break)

- No facts outside the source.
- Unicode bold on every hook — article and caption.
- Banner is PNG only, 2400×1348.
- Logo appears ONCE — top-left, 220px. No bottom lockup, no second instance.
- `dialora.ai` watermark — bottom-right only.
- One `<span class="blue">` per banner headline. Not two.
- No em dashes anywhere. No emojis in the article body.
- Caption hashtags must include `#AIVoiceAgent #VoiceAI #DialoraAI`.
- Never stop mid-flow to ask preferences when the source is clear.
- `.photo` div NEVER has `transform: scaleX(-1)` — face points RIGHT naturally, use raw image as-is.
- Never apply any flip or mirror to the hero image, neither in CSS nor via Pillow.
- Never prompt Mystic for "facing left" — use the image as generated.
- No card box or rounded container on the banner — text sits directly on dark zone.
- Article body must mix paragraphs and bullets — never wall-to-wall prose.
- Subject choice must be tied to the article's specific core moment, not the generic topic category.
- Never use same subject type two banners in a row — vary.

---

## Bundled assets

- `assets/dialora-logo.png` — transparent light wordmark for dark banners
- `assets/fonts/Lato-Regular.ttf`, `Lato-Bold.ttf`, `Lato-Black.ttf`
