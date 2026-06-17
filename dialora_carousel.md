---
name: dialora-carousel
description: >
  Automatically generate Dialora-branded social media carousel posts (slide images + PDF) and
  matching LinkedIn/Instagram captions from a blog article, written content, or topic. Use this
  skill whenever the user provides an article, blog post, or topic in a Dialora context and
  wants social media content. Also triggers when user says "make a carousel", "create slides",
  "generate posts", "turn this into a carousel", "social media post for Dialora", "carousel
  for Dialora", or simply pastes blog content after asking for a Dialora carousel earlier in
  the conversation. The skill runs end-to-end in one pass: content planning, carousel copy,
  slide design, and captions, with no mid-flow approval gates. Always use this skill for any
  Dialora content creation involving multiple slides or social post generation.
---

# Dialora Carousel & Social Post Generator

Generate polished, on-brand Dialora carousel slides (PNG per slide + PDF) and social media captions from a blog post or article.

---

## Brand Identity

**Brand**: Dialora — AI Voice/Call platform (dialora.ai)
**Tagline style**: Short, punchy, bold statements. Problem to solution narrative.
**Tone**: Confident, direct, B2B tech. Not casual. Not corporate-stiff.
**Logo**: Use the bundled `assets/dialora-logo.png` file. The logo is the wordmark "Dialora" with a stylized blue "o" mark. Place it top-left on every slide at ~180px wide.
**Website**: dialora.ai
**Primary blue**: #1976D2
**Accent blue**: #42A5F5
**Typography**: Lato (bundled in `assets/fonts/`). Use weights 400 (Regular), 700 (Bold), 900 (Black). Other weights are not available — do not use 500/600/800.

**No emojis anywhere** — not in slides, not in captions. Dialora's brand voice is clean and confident, not performative.

---

## The One Visual Style (Navy + Glow Card)

This is the locked-in Dialora carousel style. Do not invent variants without explicit user request.

- **Background**: deep navy `#050B1A` with a dotted pattern overlay (light blue dots, ~14px spacing)
- **Card**: rounded glassmorphic card (40px from edges, 32px corner radius) with a soft blue radial-gradient glow at center-top, subtle inner box-shadow, and a fine dotted texture inside
- **Headlines**: Lato Black (900), white, with one or two key words highlighted in `#42A5F5` (accent blue)
- **Body text**: Lato Regular/Bold, white at 70–85% opacity for hierarchy
- **Numbers and stats**: bold and in accent blue (`#42A5F5`)
- **Logo**: top-left, ~180px wide, using the bundled PNG (transparent background)
- **Website**: bottom-right, "dialora.ai" in small white text at 60% opacity
- **CTA buttons**: solid `#1976D2` background, white text, 50px border-radius pill shape, soft blue glow shadow
- **No emojis. No icons in circles. No gradient borders. No phone-tree clipart.** Keep it minimal.

---

## Step-by-Step Workflow

### Step 1 — Take inputs

The user will provide ONE thing: the blog post, article, or topic content. That's the only required input.

**Do not ask any clarifying questions.** Run the entire pipeline (content planning → carousel writing → design → captions) in a single pass without stopping for approval. The user's expectation is: "I gave you content, give me back a finished deck plus captions."

Defaults to use without asking:
- Style: the locked-in navy + blue glow style (see "The One Visual Style" section)
- Slide count: pick yourself based on content density (typically 7–10)
- CTA wording: "Start with Dialora"
- Last slide question: pick a relevant one based on the content's pain point

Only ask the user a question if the content itself is missing or unclear. If they give you a topic without details, you may search or expand on it as needed, but do not stop to ask preferences.

### Step 2 — Plan the carousel

Read the article and decide slide count based on the **content's importance and density**, not a fixed number. Typical range: 7–10 slides. Skip the article's intro fluff — start where the value starts.

**Slide structure pattern:**

| Slide | Purpose |
|-------|---------|
| 1 | Hook — bold problem statement or contrarian claim |
| 2 | Pain — one-liner that sharpens the problem |
| 3 to N-2 | Body — one key insight, list, stat, or breakdown per slide |
| N-1 | Payoff / ROI — the "why this matters" stat |
| N (last) | CTA — minimal, see Step 4 |

### Step 3 — Content density rule (CRITICAL)

This is the rule most often violated. Each slide is a billboard, not a paragraph.

- **Hook slide**: 1 short statement. One line of supporting text max.
- **Body slides**: 3–5 short bullets, each 3–7 words. NO sub-explanations under bullets. NO long sentences.
- **Stat slides**: one big number + short context line + 1–2 small footer lines.
- **CTA slide**: ONE question + ONE supporting line + ONE CTA button. Nothing else. Do not list features. Do not describe what they get. Do not add a tagline.
- **Total content per slide should be 30–50% of what feels "complete"** as a writer. Cut. Then cut again.
- **Headlines**: 4–10 words max
- **Supporting lines**: 1–2 short lines, never 3+

If you find yourself writing "and" or commas in bullet points, you've gone too long.

**Internally plan the carousel content before writing HTML, but do not stop to present it to the user as a separate approval step.** Run end-to-end in one pass.

### Step 4 — Generate slides as HTML at 980×1200px

Each slide is a self-contained HTML file. Use the unified template below — do not invent new layouts per slide. Vary content blocks (price-row, num-list, bullets, big-stat) within the same shell.

**Required structural elements on every slide:**
- The dotted background pattern
- The glow card with its `::before` and `::after` pseudo-elements
- Logo image top-left (`assets/dialora-logo.png`)
- "dialora.ai" text bottom-right
- Content vertically centered (`justify-content: center`) inside `.content`

#### Master HTML Template

Use this as the shell for every slide. Replace only the inner `<div class="content">` block per slide.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
  @font-face {
    font-family: 'Lato';
    src: url('file://ABSOLUTE_PATH/assets/fonts/Lato-Regular.ttf') format('truetype');
    font-weight: 400; font-style: normal;
  }
  @font-face {
    font-family: 'Lato';
    src: url('file://ABSOLUTE_PATH/assets/fonts/Lato-Bold.ttf') format('truetype');
    font-weight: 700; font-style: normal;
  }
  @font-face {
    font-family: 'Lato';
    src: url('file://ABSOLUTE_PATH/assets/fonts/Lato-Black.ttf') format('truetype');
    font-weight: 900; font-style: normal;
  }
  @font-face {
    font-family: 'Lato';
    src: url('file://ABSOLUTE_PATH/assets/fonts/Lato-Italic.ttf') format('truetype');
    font-weight: 400; font-style: italic;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    width: 980px; height: 1200px; overflow: hidden;
    background: #050B1A;
    font-family: 'Lato', sans-serif;
    position: relative;
  }
  .dot-pattern {
    position: absolute; inset: 0;
    background-image: radial-gradient(circle, rgba(74,144,217,0.35) 1.2px, transparent 1.2px);
    background-size: 14px 14px;
    z-index: 1;
  }
  .card {
    position: absolute;
    top: 40px; left: 40px; right: 40px; bottom: 40px;
    border-radius: 32px;
    background: radial-gradient(ellipse at 50% 35%,
      rgba(33,109,200,0.55) 0%,
      rgba(15,40,80,0.7) 35%,
      rgba(8,20,40,0.85) 70%,
      rgba(5,11,26,0.95) 100%);
    border: 1px solid rgba(255,255,255,0.08);
    box-shadow: 0 0 80px rgba(25,118,210,0.2) inset;
    overflow: hidden;
    z-index: 2;
  }
  .card::before {
    content: '';
    position: absolute; top: 0; left: 50%; transform: translateX(-50%);
    width: 80%; height: 60%;
    background: radial-gradient(ellipse at center top, rgba(66,165,245,0.3) 0%, transparent 70%);
    pointer-events: none;
  }
  .card::after {
    content: '';
    position: absolute; inset: 0;
    background-image: radial-gradient(circle, rgba(150,200,255,0.06) 1px, transparent 1px);
    background-size: 12px 12px;
    pointer-events: none;
  }

  .logo-top {
    position: absolute; top: 60px; left: 70px; z-index: 11;
    width: 180px; height: auto;
  }
  .content {
    position: relative; z-index: 10;
    padding: 60px 70px;
    height: 100%;
    display: flex; flex-direction: column;
    justify-content: center;
    align-items: flex-start;
  }
  h1 {
    font-size: 76px; font-weight: 900; line-height: 1.05;
    color: #ffffff; letter-spacing: -1.5px; margin-bottom: 32px;
  }
  h1 .blue { color: #42A5F5; }
  .sub {
    font-size: 30px; color: rgba(255,255,255,0.75);
    line-height: 1.4; font-weight: 400;
  }
  .website {
    position: absolute; bottom: 50px; right: 70px; z-index: 11;
    font-size: 22px; color: rgba(255,255,255,0.6);
    font-weight: 400; letter-spacing: 1px;
  }
</style>
</head>
<body>
<div class="dot-pattern"></div>
<div class="card">
  <img src="file://ABSOLUTE_PATH/assets/dialora-logo.png" class="logo-top" alt="Dialora">
  <div class="content">
    <!-- SLIDE CONTENT BLOCK GOES HERE -->
  </div>
  <div class="website">dialora.ai</div>
</div>
</body>
</html>
```

Replace `ABSOLUTE_PATH` with the actual absolute path to the skill assets when rendering. Do not use relative paths — Playwright requires absolute file:// URIs.

### Step 5 — Content blocks for inner `.content`

Pick one block per slide based on what the slide needs to communicate.

**Hook / pain / payoff slide** (just headline + sub):
```html
<h1>You're probably<br><span class="blue">overpaying</span><br>for Voice AI.</h1>
<p class="sub">And you don't even know it.</p>
```

**Numbered list slide** (the "X ways / things / steps" pattern):
```html
<h1>Voice AI vendors<br>charge in <span class="blue">4 ways.</span></h1>
<ul class="num-list">
  <li><span class="num">01</span> Per-minute</li>
  <li><span class="num">02</span> Per-conversation</li>
  <li><span class="num">03</span> Monthly license tiers</li>
  <li><span class="num">04</span> Enterprise usage bands</li>
</ul>
<div class="footer-line">Most blend two. Always model both.</div>
```
Add CSS:
```css
.num-list { list-style: none; }
.num-list li { display: flex; align-items: baseline; gap: 24px;
  margin-bottom: 24px; color: #ffffff; font-size: 32px; font-weight: 700; }
.num-list li .num { font-size: 28px; color: #42A5F5; font-weight: 900; min-width: 40px; }
.footer-line { margin-top: 36px; font-size: 26px;
  color: rgba(255,255,255,0.6); font-weight: 400; font-style: italic; }
```

**Price/comparison row slide** (label on left, value on right):
```html
<h1>What it actually<br>costs <span class="blue">today.</span></h1>
<div class="price-row">
  <div class="label">Small Business</div>
  <div class="amount">$0.10–$0.15 / min</div>
</div>
<!-- repeat .price-row -->
```
Add CSS:
```css
.price-row { display: flex; justify-content: space-between; align-items: baseline;
  padding: 22px 0; border-bottom: 1px solid rgba(66,165,245,0.18); }
.price-row .label { font-size: 30px; color: #ffffff; font-weight: 700; }
.price-row .amount { font-size: 34px; color: #42A5F5; font-weight: 900; letter-spacing: -0.5px; }
```

**Bullet list slide** (3–5 short items):
```html
<h1>It's not the<br><span class="blue">per-minute rate.</span></h1>
<div class="pre-line">It's:</div>
<ul class="bullets">
  <li>Voice quality &amp; latency</li>
  <li>The LLM behind your agent</li>
  <li>Telephony &amp; phone numbers</li>
  <li>Compliance (HIPAA / SOC 2)</li>
</ul>
```
Add CSS:
```css
.pre-line { font-size: 28px; color: rgba(255,255,255,0.65);
  font-weight: 400; margin-bottom: 38px; }
ul.bullets { list-style: none; }
ul.bullets li { font-size: 32px; color: #ffffff; margin-bottom: 22px;
  font-weight: 700; display: flex; align-items: baseline; gap: 18px; }
ul.bullets li::before { content: ''; display: inline-block;
  width: 12px; height: 12px; border-radius: 50%; background: #42A5F5;
  flex-shrink: 0; transform: translateY(-6px); }
```

**Big stat slide** (giant percentage or number, with context):
```html
<div class="big-stat">40–70<span class="pct">%</span></div>
<div class="stat-line">lower cost per call.</div>
<div class="footer-stats">
  <div><strong>$80B</strong> in projected labor savings (Gartner).</div>
  <div>The question isn't <em>if.</em> It's <em>which platform.</em></div>
</div>
```
Add CSS:
```css
.big-stat { font-size: 220px; font-weight: 900; color: #42A5F5;
  line-height: 1; letter-spacing: -6px; margin-bottom: 12px; }
.big-stat .pct { font-size: 110px; vertical-align: top; color: #ffffff; }
.stat-line { font-size: 40px; font-weight: 700; color: #ffffff;
  line-height: 1.2; margin-bottom: 50px; }
.footer-stats { display: flex; flex-direction: column; gap: 14px;
  font-size: 26px; color: rgba(255,255,255,0.75); line-height: 1.5; font-weight: 400; }
.footer-stats strong { color: #42A5F5; font-weight: 900; }
```

**CTA slide (minimal — this is the locked CTA pattern)**:

The CTA slide uses a brighter card variant — replace the standard card gradient with this brighter one to make the last slide pop:
```css
.card {
  background: radial-gradient(ellipse at 50% 50%,
    rgba(40,130,230,0.7) 0%,
    rgba(20,60,130,0.75) 35%,
    rgba(8,25,55,0.9) 75%,
    rgba(5,11,26,0.95) 100%);
  /* keep border + box-shadow + overflow same as standard */
}
```
Content block:
```html
<h1>Still guessing<br>your Voice AI <span class="blue">bill?</span></h1>
<p class="sub">Get a real number based on<br>your actual call volume.</p>
<div class="cta-btn">Start with Dialora</div>
```
Add CSS:
```css
.cta-btn {
  display: inline-block;
  background: #1976D2; color: #ffffff;
  font-size: 30px; font-weight: 700;
  padding: 24px 56px; border-radius: 50px;
  width: fit-content; letter-spacing: -0.3px;
  box-shadow: 0 0 30px rgba(66,165,245,0.4);
  margin-top: 32px;
}
```

**RULE for the CTA slide**: one question, one supporting line, one button. That is it. Do not add bullet lists, feature highlights, or extra taglines.

### Step 6 — Convert HTML slides to PNG + PDF

Install dependencies once:
```bash
pip install playwright img2pdf --break-system-packages
playwright install chromium
```

Render script:
```python
import asyncio
from pathlib import Path
from playwright.async_api import async_playwright
import img2pdf

HTML_DIR = Path("/path/to/html")
OUTPUT_DIR = Path("/mnt/user-data/outputs")
OUTPUT_DIR.mkdir(parents=True, exist_ok=True)

async def html_to_png(html_path: Path, png_path: Path):
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page(viewport={"width": 980, "height": 1200})
        await page.goto(f"file://{html_path}")
        await page.wait_for_timeout(400)  # let fonts/images load
        await page.screenshot(path=str(png_path),
                              clip={"x":0,"y":0,"width":980,"height":1200})
        await browser.close()

async def main():
    html_files = sorted(HTML_DIR.glob("slide_*.html"))
    png_files = []
    for hf in html_files:
        png_path = OUTPUT_DIR / hf.with_suffix(".png").name
        await html_to_png(hf, png_path)
        png_files.append(str(png_path))
    pdf_path = OUTPUT_DIR / "dialora_carousel.pdf"
    with open(pdf_path, "wb") as f:
        f.write(img2pdf.convert(png_files))

asyncio.run(main())
```

**Important**: If the bundled logo PNG has a black background (not transparent), strip it once before use:
```python
from PIL import Image
import numpy as np
img = Image.open('assets/dialora-logo.png').convert('RGBA')
data = np.array(img)
r, g, b, _ = data[:,:,0], data[:,:,1], data[:,:,2], data[:,:,3]
mask = (r < 30) & (g < 30) & (b < 30)
data[mask] = [0, 0, 0, 0]
Image.fromarray(data).save('assets/dialora-logo.png')
```

### Step 7 — Generate captions (LinkedIn + Instagram)

Always produce both. Both follow the **Hook → Body → CTA** structure with clearly labeled sections so the user can scan them.

**Caption rules:**
- **No em dashes** (`—`). Use periods, commas, or new sentences instead.
- **No "2026"** or year references. Dates anchor content and feel stale fast.
- **No emojis**.
- **Body should be tight**: 2–3 short lines max. Resist the urge to recap the whole carousel.
- Hashtags: LinkedIn 4–6 niche tags, Instagram 8–12 mixed broad + niche.

**LinkedIn format:**
```
Hook
[1 short, contrarian or curiosity-gap statement]

Body
[1–2 short paragraphs, 2–3 sentences total. State the problem and one consequence. Don't list everything from the carousel.]

CTA
[One clear question + offer. Link to dialora.ai]

#VoiceAI #SaaS #ContactCenter #ConversationalAI #AIAgents
```

**Instagram format:**
```
Hook
[1 punchy sentence]

Body
[2–3 short stacked lines, broken with whitespace. One concrete number if possible. End with "Swipe for the breakdown" or similar.]

CTA
[Save prompt + "link in bio"]

.
.
.

#voiceai #aiagents #saas #b2bsaas #aitools #conversationalai #contactcenter #callcenter
```

When presenting captions to the user, use **bold section labels** (`**Hook**`, `**Body**`, `**CTA**`) in the chat output so the structure is visible at a glance.

---

## Output Checklist

Before presenting:
- [ ] All slides rendered as PNG (slide_01.png … slide_NN.png) at 980×1200
- [ ] Single combined PDF (dialora_carousel.pdf)
- [ ] Logo image present top-left on every slide (not text wordmark)
- [ ] Content vertically centered on every slide
- [ ] Brand blue accents on key words/numbers, not just plain white
- [ ] Last slide is minimal: question + supporting line + CTA only
- [ ] LinkedIn caption written with Hook/Body/CTA labels
- [ ] Instagram caption written with Hook/Body/CTA labels
- [ ] No emojis anywhere (slides or captions)
- [ ] No em dashes in captions
- [ ] No year references in captions
- [ ] Files presented via `present_files` tool

## File naming convention
```
/mnt/user-data/outputs/
  slide_01.png
  slide_02.png
  …
  slide_NN.png
  dialora_carousel.pdf
```

---

## Workflow Order (IMPORTANT)

The user gives you content. You give them a finished deck + captions. No questions, no approval gates in the middle.

1. Take the content the user provides. Do not ask for style, slide count, or CTA preferences.
2. Skip article intro fluff. Start where the value starts.
3. Internally plan the carousel structure (hook, body slides, payoff, CTA).
4. Write the carousel content following the density rule (30–50% of "feels complete").
5. Design the slides using the master template + content blocks.
6. Render to PNG + PDF.
7. Write captions (Hook / Body / CTA, both LinkedIn and Instagram).
8. Present everything via `present_files` in a single response.

The user can always request revisions after seeing the result. Do not preempt that with mid-flow approval gates.

---

## Common Mistakes to Avoid

- **Too much copy per slide.** Each slide is a billboard, not a paragraph. If a bullet has "and" or a comma, it's too long.
- **Using the text wordmark "Dialora" instead of the logo image.** Always use `assets/dialora-logo.png`.
- **Plain white headlines with no blue accent.** Pick 1–2 key words per headline and color them `#42A5F5`.
- **Top-aligned content instead of centered.** Always vertically center the `.content` block.
- **Bloated CTA slide with features and taglines.** One question, one line, one button.
- **Em dashes, emojis, or year references in captions.** Strip all three.
- **Stopping mid-flow to ask the user questions.** Run end-to-end. The user gives content, you deliver a finished deck plus captions in one pass.
- **Inventing new font weights.** Lato has 100/300/400/700/900 only. Use 400/700/900 for safety.
- **Adding icons in colored circles or emoji-like decorations.** Keep it typographic and minimal.

---

## Bundled Assets

- `assets/dialora-logo.png` — Dialora wordmark, transparent background, light variant (use on dark slides)
- `assets/fonts/Lato-*.ttf` — Lato font family (Regular, Bold, Black, Italic, BoldItalic, Light, Thin)

When using these in HTML, reference them with absolute `file://` paths so Playwright can load them at render time.
