# Squarespace API Exploration — Nugget Digital

## Site Overview: nuggetdigital.com.au

**Platform:** Squarespace
**Tech Stack:** Squarespace Online Stores, jQuery 2.1.1, Adobe Typekit, Google Tag Manager

### Current Site Pages
- **Home** — nuggetdigital.com.au
- **About Us** — /about-us
- **Digital Marketing Strategy** — /digital-marketing-strategy
- **Join Us** — /join-us
- **Get in Touch** — /get-in-touch

### Services Offered
- Fractional CMO services
- 90-day digital marketing action plans
- Demand generation & content marketing programs
- Data + content + tech strategies for Australian entrepreneurs

---

## Squarespace API Capabilities

### Official REST APIs (api.squarespace.com)

The formal Squarespace API is **commerce-focused**. Available endpoints:

| API | Description | Plan Required |
|-----|-------------|---------------|
| **Products** | Manage physical, service, gift card, and download products | Commerce |
| **Inventory** | Update stock levels programmatically | Commerce |
| **Orders / Transactions** | Access order and financial transaction data | Commerce Advanced |
| **Profiles** | Retrieve customers, mailing list subscribers, donors | Commerce |
| **Forms** | Receive form submission data | Business+ |
| **Webhook Subscriptions** | Subscribe to site events (e.g. order created) | Commerce |
| **Store Pages** | Retrieve store page listings | Commerce |

**Authentication:** OAuth 2.0 or API Key (generated in Squarespace settings)

### Content Access via JSON (Unofficial but Supported)

Squarespace exposes **all page content as JSON** by appending `?format=json-pretty` to any URL:

```
https://www.nuggetdigital.com.au/?format=json-pretty
https://www.nuggetdigital.com.au/about-us?format=json-pretty
https://www.nuggetdigital.com.au/digital-marketing-strategy?format=json-pretty
```

This returns structured data including:
- Page/collection metadata (id, title, urlId, tags, categories)
- Blog post content and custom fields
- Navigation structure
- Site-wide settings

**Limitation:** This is **read-only** — you cannot create/update content via this method.

### What's NOT Available via API

- **No content creation/update API** — Blog posts, pages, and general content cannot be created or modified via REST API
- **No page management API** — Cannot programmatically add/remove/reorder pages
- **No design/template API** — Cannot modify styling or layout programmatically
- **No SEO settings API** — Cannot update meta descriptions, titles, etc. via API

---

## Integration Options for Content Marketing

### 1. Third-Party MCP/Automation Bridges

| Platform | Capability |
|----------|-----------|
| **viaSocket MCP** | Connects Squarespace actions with Claude/AI tools via Model Context Protocol |
| **Zapier MCP** | Squarespace Forms integration with Claude for real-time automation |
| **Make (Integromat)** | Visual workflows connecting Claude + Squarespace |
| **Appy Pie Automate** | AI-powered content creation pipeline to Squarespace |

### 2. Squarespace Developer Mode

- Full access to site templates (HTML, CSS, JS)
- Can inject custom scripts and tracking
- Enables custom code blocks for dynamic content
- Available on Business plan and above

### 3. Content Marketing Opportunities

Given the API constraints, here are viable approaches:

#### A. Automated Content Pipeline (Indirect)
1. Use Claude API to **generate** blog posts, social copy, newsletters
2. Store content in a staging system (Google Sheets, Notion, Airtable)
3. Use Zapier/Make to **notify** when content is ready for publishing
4. Manual publish step in Squarespace (unavoidable without content API)

#### B. Form & Lead Management
- **Forms API** can capture lead data from the site
- Pipe form submissions to CRM/email tools via webhook
- Trigger automated follow-up sequences

#### C. Analytics & Monitoring
- Scrape site JSON endpoints to monitor content freshness
- Track page structure changes over time
- Monitor blog post performance via Google Tag Manager data

#### D. E-commerce Integration (if applicable)
- Full product catalog management via Products API
- Inventory sync across platforms
- Order management and fulfillment automation

#### E. Content Scheduling Dashboard
- Build a custom dashboard that:
  - Drafts content using Claude API
  - Manages an editorial calendar
  - Generates SEO-optimized copy
  - Exports formatted content ready for Squarespace paste-in

---

## Recommendation: Migration vs. Integration

### Option 1: Stay on Squarespace + Automation Layer
- **Pros:** No migration cost, familiar platform, good design tools
- **Cons:** No content API = manual publishing bottleneck, limited automation
- **Best for:** Low-frequency content updates, design-focused site

### Option 2: Headless CMS + Squarespace Frontend
- Use Squarespace for presentation, external CMS (Contentful, Strapi) for content management
- **Pros:** Full API control over content, keeps Squarespace design
- **Cons:** Complex setup, potential performance issues

### Option 3: Migrate to API-First Platform
- Move to WordPress (REST API), Webflow (CMS API), or headless CMS
- **Pros:** Full programmatic content control, better for content marketing at scale
- **Cons:** Migration effort, learning curve, design rebuild

---

## Next Steps

- [ ] Test JSON endpoints on nuggetdigital.com.au to assess current content structure
- [ ] Determine which Squarespace plan is active (Business vs Commerce)
- [ ] Identify priority content marketing initiatives to scope
- [ ] Evaluate whether Zapier/Make automation is sufficient or if platform migration is warranted
- [ ] Set up a proof-of-concept content pipeline with Claude API

---

*Research conducted: 2026-03-19*

### Sources
- [Squarespace Developer Home](https://developers.squarespace.com/)
- [Commerce APIs Overview](https://developers.squarespace.com/commerce-apis/overview)
- [View JSON Data](https://developers.squarespace.com/view-json-data)
- [Squarespace API Keys](https://support.squarespace.com/hc/en-us/articles/236297987-Squarespace-API-keys)
- [URL Queries](https://developers.squarespace.com/url-queries)
- [viaSocket MCP for Squarespace](https://viasocket.com/mcp/squarespace)
- [Zapier Squarespace Forms MCP](https://zapier.com/mcp/squarespace-forms)
- [Make: Squarespace + Claude](https://www.make.com/en/integrations/squarespace/anthropic-claude)
