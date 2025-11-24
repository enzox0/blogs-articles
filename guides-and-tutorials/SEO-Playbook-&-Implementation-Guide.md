# SEO Playbook & Implementation Guide

**By:** Renz Siguenza  
**Date:** November 24, 2025

This document outlines an end-to-end SEO process you can apply to portfolio-like sites, SaaS products, or marketing pages. It breaks the workflow into clear phases, lists deliverables for each, and provides implementation checklists so you can turn strategy into deployment-ready tasks.

---

## 1. Discovery & Goal Setting
- **Business context:** Define target audience segments, primary conversion goals, and key differentiators. Collect product roadmaps and seasonal campaigns that might influence keyword focus.
- **Benchmarking:** Capture current organic traffic, impressions, click-through rates, conversions, and top landing pages using tools like Google Search Console (GSC) and analytics dashboards.
- **Competitive landscape:** Identify 3–5 core competitors. Compare their ranking keywords, SERP features, backlink profiles, and content cadence using tools such as Ahrefs, SEMrush, or Moz.
- **Success metrics:** Document KPIs (organic sessions, conversions, keyword movements, backlink velocity, Core Web Vitals) and acceptable timelines (e.g., 3, 6, 12 months).

### Implementation
1. Create an `SEO-Brief.md` summarizing audience personas, goals, and KPIs.
2. Spin up shared dashboards (Looker Studio, GA4) with baseline snapshots for ongoing comparison.
3. Stakeholder sign-off before research begins so priorities stay aligned.

---

## 2. Keyword & SERP Research
- **Seed keyword expansion:** Start from product features, user pains, and brand terms. Use auto-suggest, People Also Ask, and competitor ranking keywords to expand the list.
- **SERP intent mapping:** For each high-value keyword, classify intent (informational, transactional, navigational, local) and observe the dominant SERP format (articles, videos, product listings, etc.).
- **Keyword scoring:** Score keywords by search volume, difficulty, business value, and intent fit. Cluster them into topic groups to inform site architecture and content strategy.

### Implementation
1. Maintain a `keywords.xlsx` (or Airtable/Notion database) with scoring columns: volume, difficulty, intent, funnel stage, mapped page.
2. Tag each keyword cluster to a URL (existing or planned). Unmapped clusters should trigger new page briefs.
3. Update the sitemap plan to reflect cluster hierarchy (pillar → subpage → supporting content).

---

## 3. Technical SEO Foundation
- **Crawl health:** Ensure clean architecture with HTML sitemaps, XML sitemaps, and robots.txt alignment. Fix crawl waste (orphan pages, redirect chains, duplicate paths).
- **Performance:** Target Core Web Vitals (LCP <2.5s, CLS <0.1, INP <200ms). Optimize images (AVIF/webp), lazy loading, code splitting, and CDN caching.
- **Index control:** Implement canonical tags, hreflang (if multilingual), structured data (JSON-LD for relevant content types), and handle pagination correctly.
- **Security & accessibility:** Enforce HTTPS, HSTS, valid SSL, and WCAG-compliant markup to reduce ranking risk.

### Implementation
1. Run automated crawls (Screaming Frog, Sitebulb) monthly; export critical issues to a backlog.
2. Track Core Web Vitals via PageSpeed Insights API or CrUX dashboards; tie regressions to deploys.
3. Version-control `robots.txt`, `sitemap.xml`, and structured data templates so changes go through PR review.

---

## 4. On-Page Optimization
- **Content alignment:** Match page copy to target keyword clusters. Maintain clear H1/H2 hierarchy, descriptive intros, and FAQ or supporting sections that satisfy related intents.
- **Meta data:** Write unique, user-focused title tags (50–60 chars) and meta descriptions (140–160 chars) with primary and secondary keywords.
- **Internal linking:** Build contextual links to reinforce topical authority and guide users toward conversions. Use keyword-informed anchor text but keep it natural.
- **UX elements:** Use descriptive CTAs, scannable bullet lists, imagery with optimized alt text, and address trust signals (testimonials, certifications).

### Implementation
1. Create page-specific optimization briefs with target keyword, title/meta draft, header outline, and conversion goals.
2. Use CMS components or Markdown templates with predefined metadata fields to avoid omissions.
3. Include on-page QA checklist in the PR template (H1 exists, meta tags set, schema included, links tested).

---

## 5. Content Strategy & Production
- **Editorial calendar:** Prioritize pillar pages, supporting blog posts, and nurturing assets (guides, case studies, tools) based on keyword clusters and funnel gaps.
- **Content quality:** Require SME input, original data, or unique POV to differentiate. Include multimedia (video, infographics) where SERP signals demand it.
- **Refresh cadence:** Audit existing content quarterly. Update outdated stats, expand thin sections, and consolidate cannibalizing posts.

### Implementation
1. Maintain a Kanban board (Ideate → Brief → Draft → Edit → Publish → Measure) with owners and due dates.
2. Store reusable briefs and style guides in the repo (`content/briefs/`) or a shared knowledge base.
3. Tag published content with UTM parameters and structured data (`Article`, `FAQ`, `HowTo`) to improve SERP appearance.

---

## 6. Off-Page & Authority Building
- **Digital PR:** Pitch compelling stories, product updates, or community contributions to earn mentions in relevant publications.
- **Partnerships:** Collaborate with complementary brands, podcasts, or webinar hosts for co-marketing and backlink opportunities.
- **Community & UGC:** Encourage satisfied users to submit testimonials, reviews, and forum posts pointing back to key landing pages.

### Implementation
1. Build a media list with contact info, coverage notes, and status tracking.
2. Use outreach templates but personalize pitches with data or product insights.
3. Monitor backlink profile monthly; disavow toxic links and reward high-quality referrers with continued collaboration.

---

## 7. Measurement & Iteration
- **Dashboards:** Track keyword rankings, traffic, conversions, and engagement per landing page. Segment by device and geography.
- **Testing:** Run SEO-focused experiments (title tag variants, structured data types, CTA placements) and log hypotheses, test windows, and outcomes.
- **Feedback loop:** Tie insights back to roadmap prioritization. If a page stalls, revisit intent alignment, content depth, and internal link support.

### Implementation
1. Automate weekly data pulls from GSC/API into a warehouse or spreadsheet for trend analysis.
2. Maintain an `experiments.md` log with hypothesis, metrics, and result notes.
3. Schedule monthly SEO standups to review progress, blockers, and upcoming launches that impact search.

---

## Deployment Checklist
Use this quick list before shipping SEO-related updates:
1. Target keyword mapped and validated against intent.
2. Title, meta description, canonical, schema, and OG tags present.
3. Page passes Core Web Vitals thresholds in lab tests.
4. Internal links updated; sitemap and navigation reflect new URL.
5. Content QA complete (grammar, accessibility, multimedia optimization).
6. Tracking parameters and events configured (scroll depth, CTA clicks, form submissions).
7. Deployment logged with date, owner, and expected outcome so results can be traced back.

---

## Tooling Stack Recommendations
- **Research:** Ahrefs, SEMrush, Google Trends, AlsoAsked.
- **Technical audits:** Screaming Frog, Sitebulb, PageSpeed Insights, Lighthouse CI.
- **Content ops:** Notion/Airtable for briefs, Grammarly/Hemingway for QA, Figma for design collaboration.
- **Monitoring:** GA4, GSC, Looker Studio, BigQuery, and Rank Tracker tools (AccuRanker, Serpstat).
- **Automation:** GitHub Actions for sitemap + schema validation, webhooks to notify Slack when Core Web Vitals regress, Zapier/Make for backlink monitoring alerts.

---

## Governance & Workflow Tips
- Treat SEO tasks like product work: use issue trackers, estimation, and sprint planning.
- Review SEO-critical files (`robots.txt`, meta components, structured data helpers) in every release checklist.
- Document every major change (new sections, redirects, experimentation) so future analyses account for context.
- Encourage cross-team visibility—share quick wins and failures to keep stakeholders invested in organic growth.

With this playbook in place, you can manage SEO as a repeatable, data-driven process rather than a one-off initiative. Adapt each phase to your team’s capacity, automate wherever possible, and keep feedback loops short to compound gains.

