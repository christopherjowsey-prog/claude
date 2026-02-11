# How to Vibe Code a Presentation with the Frontend Slides Claude Skill

> Breakdown of the AI Adopters Club article on using the **Frontend Slides** Claude Code skill to create beautiful, animation-rich HTML presentations in a single conversation.

---

## What Is Frontend Slides?

Frontend Slides is a **Claude Code skill** that lets non-designers create stunning web-based presentations without knowing CSS or JavaScript. Instead of fiddling with PowerPoint templates or wrestling with design tools, you describe what you want in plain language and Claude generates a complete, self-contained HTML presentation.

**Repository:** [github.com/zarazhangrui/frontend-slides](https://github.com/zarazhangrui/frontend-slides)

### Core Philosophy

- **Zero Dependencies** — Output is a single HTML file with inline CSS and JS. No npm, no build tools, no frameworks. It will work in 10 years.
- **Show, Don't Tell** — Instead of asking you to describe your aesthetic preferences in words, it generates visual previews and lets you pick what you like.
- **Anti-AI-Slop** — Curated distinctive styles that avoid generic AI aesthetics (no purple gradients on white, no Inter font, no cookie-cutter layouts).
- **Production Quality** — Accessible, responsive, well-commented code you can customize.

---

## Step 1: Install the Skill

Create the skill directory and copy two files from the repo:

```bash
mkdir -p ~/.claude/skills/frontend-slides
```

Download these two files into that directory:
- **`SKILL.md`** — The main instruction file that tells Claude how to behave
- **`STYLE_PRESETS.md`** — Visual style reference with 12 curated themes

You can clone the repo or manually download the files:

```bash
cd ~/.claude/skills/frontend-slides
curl -O https://raw.githubusercontent.com/zarazhangrui/frontend-slides/main/SKILL.md
curl -O https://raw.githubusercontent.com/zarazhangrui/frontend-slides/main/STYLE_PRESETS.md
```

Restart Claude Code after installation.

---

## Step 2: Invoke the Skill

In Claude Code, type:

```
/frontend-slides
```

This activates the skill and Claude enters "presentation mode."

---

## Step 3: Describe Your Presentation

Tell Claude what you need. The skill handles three workflows:

| Workflow | Description |
|----------|-------------|
| **New from scratch** | Describe your topic and Claude structures + designs everything |
| **PowerPoint conversion** | Provide a `.pptx` file and Claude converts it to web |
| **Enhancement** | Improve an existing HTML presentation |

### What to provide:

- **Purpose**: Pitch deck, teaching session, conference talk, or internal review
- **Slide count**: How many slides you need
- **Content**: Bullet points, an outline, or just a topic — Claude can help structure it
- **Mood/tone**: Professional, playful, technical, bold, minimal, etc.

**Example prompt:**
> "Create a 12-slide presentation about our Q1 product roadmap. It's for an internal all-hands meeting. Tone should be confident and clean."

---

## Step 4: Pick a Visual Style (the "Vibe" Part)

This is where vibe coding shines. Claude generates **3 distinct style preview files** based on your mood preferences. You open them in your browser and **pick the one that resonates**.

No need to articulate "I want a serif font with muted earth tones" — just look and choose.

### Available Style Presets (12 themes):

**Dark Themes:**
| Theme | Character |
|-------|-----------|
| Bold Signal | Confident orange accent cards |
| Electric Studio | Split panel design |
| Creative Voltage | Neon yellow + electric blue |
| Dark Botanical | Elegant serif with soft abstract shapes |

**Light Themes:**
| Theme | Character |
|-------|-----------|
| Notebook Tabs | Editorial with colorful edge tabs |
| Pastel Geometry | Friendly rounded cards with vertical pills |
| Split Pastel | Two-color vertical divisions |
| Vintage Editorial | Witty geometric accents |

**Specialty Themes:**
| Theme | Character |
|-------|-----------|
| Neon Cyber | Futuristic glow effects |
| Terminal Green | Developer / hacker aesthetic |
| Swiss Modern | Bauhaus precision and grids |
| Paper & Ink | Literary elegance |

---

## Step 5: Claude Builds the Full Presentation

Once you select a style, Claude generates the complete presentation as a **single self-contained HTML file**. This file includes:

- **Inline CSS** with custom properties and responsive typography via `clamp()` functions
- **Inline JavaScript** for navigation and animations
- **Keyboard navigation** (arrow keys, spacebar)
- **Touch/swipe support** for mobile
- **Mouse wheel scrolling**
- **Progress indicator and navigation dots**
- **Scroll-triggered animations**
- **Reduced motion accessibility support**

### Strict Design Rules the Skill Enforces:

- Every slide occupies exactly **one viewport height** (`100vh`) — no scrolling within slides, ever
- **Title slides**: 1 heading + 1 subtitle max
- **Content slides**: 1 heading + 4-6 bullets (2 lines each) max, or 2 short paragraphs
- **Feature grids**: Capped at 6 cards
- Responsive breakpoints at 700px, 600px, and 500px heights
- No generic fonts (Inter, Roboto), no cliché colors (#6366f1 indigo), no realistic illustrations — abstract shapes only

---

## Step 6: Preview in Your Browser

Claude opens the HTML file in your default browser. Review the presentation:

- Click through slides using arrow keys or dots
- Test on different screen sizes
- Check animations and transitions

---

## Step 7: Iterate Conversationally

Refine the presentation through follow-up prompts:

- "Make the title slide bolder"
- "Add a slide after slide 4 about competitive landscape"
- "Swap to a darker color scheme"
- "Add speaker notes"
- "Reduce the text on slide 7 — it feels too dense"

Each iteration regenerates the HTML file. The skill maintains the constraint that every slide must fit in one viewport — if you add too much content, it will restructure rather than allow scrolling.

---

## Step 8: Convert PowerPoint (Optional Workflow)

If you have an existing `.pptx` file:

1. Tell Claude: "Convert this PowerPoint to a web presentation" and provide the file
2. Claude uses **python-pptx** to extract text, images, and speaker notes
3. You confirm the extracted content is accurate
4. Pick a visual style from the 3 previews
5. Receive a single HTML file preserving all original assets

**Requirement:** Python with the `python-pptx` library installed:
```bash
pip install python-pptx
```

---

## Step 9: Deploy or Share

Since the output is a single HTML file:

- **Email it** as an attachment (anyone with a browser can view it)
- **Host it** on any static hosting (GitHub Pages, Netlify, Vercel)
- **Present directly** from the file — no internet required
- **Edit the source** — the code is well-commented for manual tweaks

---

## Summary: The Complete Workflow

```
1. Install     →  Copy SKILL.md + STYLE_PRESETS.md to ~/.claude/skills/frontend-slides/
2. Invoke      →  Type /frontend-slides in Claude Code
3. Describe    →  Tell Claude your topic, purpose, slide count, and mood
4. Pick Style  →  Browse 3 visual previews, choose your favorite
5. Generate    →  Claude builds the full HTML presentation
6. Preview     →  Open in browser, test navigation and responsiveness
7. Iterate     →  Refine through conversation ("make slide 3 bolder")
8. Ship        →  Share the single HTML file or host it anywhere
```

---

## Why This Matters

- **No design skills required** — The visual preview approach means you pick what looks good rather than trying to describe it
- **No framework lock-in** — A single HTML file has zero dependencies and will outlast any SaaS tool
- **No AI slop** — The curated style presets avoid the generic look that plagues AI-generated content
- **Speed** — Go from idea to polished presentation in one conversation

---

## Sources

- [Frontend Slides GitHub Repository](https://github.com/zarazhangrui/frontend-slides)
- [AI Adopters Club — How to Vibe Code a Presentation](https://open.substack.com/pub/aiadopters/p/how-to-vibe-code-a-presentation)
- [36 Claude Skills Examples (Substack)](https://aiblewmymind.substack.com/p/claude-skills-36-examples)
- [Awesome Claude Skills (GitHub)](https://github.com/travisvn/awesome-claude-skills)
