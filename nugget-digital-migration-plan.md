# Nugget Digital — Migration Architecture Plan

## Goal

Rebuild nuggetdigital.com.au as a **Next.js + Vercel** site that:
1. Retains the current Squarespace design aesthetics (clean, professional, minimal)
2. Enables **fully automated content publishing** via API (like the broadband deals site)
3. Positions Nugget Digital as an **AI-first consultancy**
4. Supports high-frequency content marketing at scale

## Why Migrate Off Squarespace

Squarespace has **no content creation API**. Full stop. You can't programmatically publish blog posts, update pages, or manage content. For an AI-first consultancy that wants automated content pipelines, this is a fundamental blocker — not a workaround-able limitation.

## Proposed Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    VERCEL (Hosting)                       │
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │           Next.js Application                     │   │
│  │                                                    │   │
│  │  ┌─────────┐  ┌──────────┐  ┌─────────────────┐ │   │
│  │  │  Pages   │  │   Blog   │  │  Case Studies   │ │   │
│  │  │ (Static) │  │  (Dynamic)│  │   (Dynamic)     │ │   │
│  │  └─────────┘  └──────────┘  └─────────────────┘ │   │
│  │                                                    │   │
│  │  ┌─────────────────┐  ┌──────────────────────┐   │   │
│  │  │  API Routes      │  │  Content Preview     │   │   │
│  │  │  /api/publish    │  │  /api/draft-preview  │   │   │
│  │  │  /api/generate   │  │                      │   │   │
│  │  └─────────────────┘  └──────────────────────┘   │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
           │                    │
           ▼                    ▼
┌──────────────────┐  ┌──────────────────────────────┐
│    Supabase       │  │      Claude API               │
│                    │  │                                │
│  - Blog posts     │  │  - Content generation          │
│  - Case studies   │  │  - SEO optimisation            │
│  - Page content   │  │  - Social copy creation        │
│  - Contact forms  │  │  - Content repurposing         │
│  - Analytics      │  │  - Editorial suggestions       │
│  - Media assets   │  │                                │
└──────────────────┘  └──────────────────────────────┘
```

## Stack Comparison

| Concern | Current (Squarespace) | Proposed (Next.js + Vercel) |
|---------|----------------------|----------------------------|
| Content publishing | Manual only | Fully automated via API |
| Blog posts | Manual editor | Claude generates → API publishes |
| Design control | Template-locked | Full custom (Tailwind CSS) |
| Performance | Decent | Excellent (SSG/ISR, edge CDN) |
| SEO | Basic built-in | Full programmatic control |
| Cost | ~$33/mo (Business) | ~$20/mo (Vercel Pro) + Supabase free tier |
| Content API | None | Full CRUD via Supabase |
| AI integration | None native | Native Claude API integration |
| Analytics | Squarespace Analytics | Vercel Analytics + custom |

## Design Migration Strategy

### Preserving the Squarespace Aesthetic

The current Nugget Digital site has a clean, professional consultancy look. To retain this:

1. **Typography** — Identify the current Adobe Typekit fonts and source equivalents (Google Fonts or self-hosted)
2. **Colour palette** — Extract exact hex values from the current site CSS
3. **Layout patterns** — Recreate the section-based layout using Tailwind CSS
4. **Imagery style** — Maintain the same image treatment and spacing
5. **Navigation** — Replicate the minimal top nav pattern

### Implementation Approach

Use **Tailwind CSS** with a custom theme config that mirrors the current Squarespace design tokens:

```js
// tailwind.config.js (example - actual values extracted from current site)
module.exports = {
  theme: {
    extend: {
      colors: {
        nugget: {
          primary: '#...', // Extract from current site
          secondary: '#...',
          accent: '#...',
          bg: '#...',
          text: '#...',
        }
      },
      fontFamily: {
        heading: ['...', 'sans-serif'], // Match current Typekit fonts
        body: ['...', 'sans-serif'],
      }
    }
  }
}
```

## Content Automation Pipeline

### How It Works End-to-End

```
1. TRIGGER
   ├── Scheduled (cron via Vercel)
   ├── Manual (admin dashboard)
   └── Event-driven (industry news, trending topics)
           │
           ▼
2. GENERATE (Claude API)
   ├── Blog post draft
   ├── SEO metadata (title, description, keywords)
   ├── Social media copy (LinkedIn, Twitter)
   ├── Email newsletter snippet
   └── Internal linking suggestions
           │
           ▼
3. REVIEW (Optional)
   ├── Auto-publish (for routine content)
   └── Queue for human review (for thought leadership)
           │
           ▼
4. PUBLISH (Supabase + Vercel)
   ├── Insert into Supabase (blog_posts table)
   ├── Trigger Vercel ISR revalidation
   ├── Page goes live in seconds
   └── Sitemap auto-updates
           │
           ▼
5. DISTRIBUTE
   ├── LinkedIn post via API
   ├── Email newsletter via SendGrid/Resend
   ├── Google Search Console ping
   └── Analytics tracking begins
```

### Content Types to Automate

| Content Type | Frequency | Automation Level |
|-------------|-----------|-----------------|
| Blog posts (SEO) | 2-4x per week | Full auto or review queue |
| Case studies | Monthly | AI draft → human review |
| Industry insights | Weekly | Full auto from news feeds |
| LinkedIn posts | Daily | Full auto from blog content |
| Newsletter | Weekly | Auto-curated from week's content |
| Service page updates | Quarterly | AI draft → human review |

## Database Schema (Supabase)

```sql
-- Core content tables
CREATE TABLE pages (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug TEXT UNIQUE NOT NULL,
  title TEXT NOT NULL,
  content JSONB NOT NULL,
  meta_title TEXT,
  meta_description TEXT,
  status TEXT DEFAULT 'draft', -- draft, published, archived
  published_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE blog_posts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug TEXT UNIQUE NOT NULL,
  title TEXT NOT NULL,
  excerpt TEXT,
  content TEXT NOT NULL, -- Markdown
  featured_image TEXT,
  author TEXT DEFAULT 'Nugget Digital',
  tags TEXT[],
  categories TEXT[],
  meta_title TEXT,
  meta_description TEXT,
  status TEXT DEFAULT 'draft',
  ai_generated BOOLEAN DEFAULT false,
  reviewed BOOLEAN DEFAULT false,
  published_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE contact_submissions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  company TEXT,
  message TEXT,
  source TEXT, -- which page/form
  created_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE content_queue (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  content_type TEXT NOT NULL, -- blog, social, newsletter
  title TEXT,
  content TEXT,
  metadata JSONB,
  status TEXT DEFAULT 'pending', -- pending, approved, published, rejected
  scheduled_for TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now()
);
```

## Site Pages to Build

### Static Pages (migrated from Squarespace)
1. **Home** — Hero, services overview, social proof, CTA
2. **About Us** — Team, story, values
3. **Digital Marketing Strategy** — Service details, methodology
4. **Join Us** — Careers/contractor opportunities
5. **Get in Touch** — Contact form (→ Supabase + email notification)

### New Dynamic Pages (AI-first additions)
6. **Blog / Insights** — AI-generated + curated content hub
7. **Case Studies** — Dynamic, filterable portfolio
8. **Resources** — Guides, templates, tools (lead magnets)
9. **AI Lab** — Showcase AI capabilities, live demos (differentiator)

## "AI-First" Brand Positioning

The rebuilt site should **demonstrate** AI-first, not just claim it:

- **Live AI content generation demo** on the site (showcase capability)
- **Blog cadence** that no human-only team could match (2-4 posts/week)
- **Personalised content** based on visitor industry/interest
- **Automated insights dashboard** showing real-time marketing metrics
- **AI-powered contact form** that provides instant, intelligent responses

## Migration Timeline (Suggested Phases)

### Phase 1: Foundation (Week 1-2)
- [ ] Set up Next.js project with Tailwind CSS
- [ ] Extract design tokens from current Squarespace site
- [ ] Set up Supabase database with schema
- [ ] Deploy to Vercel with custom domain config
- [ ] Build static pages (Home, About, Services, Contact, Join)

### Phase 2: Content Engine (Week 3-4)
- [ ] Build blog system with Supabase backend
- [ ] Create Claude API content generation pipeline
- [ ] Build admin dashboard for content review queue
- [ ] Implement ISR for blog post pages
- [ ] Set up automated publishing API routes

### Phase 3: Automation (Week 5-6)
- [ ] Set up cron jobs for scheduled content generation
- [ ] Build social media distribution pipeline
- [ ] Implement contact form with AI-powered responses
- [ ] Add analytics and tracking
- [ ] SEO optimisation (sitemap, structured data, meta tags)

### Phase 4: AI Showcase (Week 7-8)
- [ ] Build AI Lab / demo page
- [ ] Add personalisation features
- [ ] Launch content marketing at scale
- [ ] Monitor, iterate, optimise

## DNS & Domain Transition

1. Build and test on a Vercel preview URL
2. When ready, update DNS for nuggetdigital.com.au to point to Vercel
3. Zero-downtime cutover (Vercel handles SSL automatically)
4. Keep Squarespace subscription active for 30 days as fallback

---

## Key Decision Points

1. **Supabase vs. simpler option** — For a consultancy site (not e-commerce), you may not need Supabase. A file-based MDX blog + Vercel could be simpler. But if you want the full automation pipeline and admin dashboard, Supabase makes sense.

2. **Design fidelity** — Do you want a pixel-perfect recreation of the current site, or is this an opportunity to refresh the brand while keeping the same feel?

3. **Content review workflow** — Full auto-publish, or human-in-the-loop approval for some/all content?

4. **Scope of AI features** — How prominent should the AI showcase be? Subtle (just fast content) or explicit (live demos, AI Lab page)?

---

*Architecture plan created: 2026-03-19*
