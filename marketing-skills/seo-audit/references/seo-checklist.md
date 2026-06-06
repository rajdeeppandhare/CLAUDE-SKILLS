# SEO Audit Checklist

Complete reference checklist for technical, on-page, and off-page SEO. Use as a systematic audit tool — check each item, note the status, and prioritise fixes by impact.

---

## Section 1: Indexing & Crawlability

### Robots.txt
- [ ] File exists at `/robots.txt`
- [ ] Not blocking important pages or directories
- [ ] References XML sitemap location
- [ ] No `Disallow: /` accidentally blocking everything

### XML Sitemap
- [ ] Sitemap exists and is accessible
- [ ] Submitted to Google Search Console
- [ ] Submitted to Bing Webmaster Tools
- [ ] Contains all important pages (no orphan pages excluded)
- [ ] Excludes noindexed, paginated, and duplicate pages
- [ ] Last modified dates are accurate
- [ ] Under 50,000 URLs (split if larger)

### Indexing Status
- [ ] Check `site:domain.com` — expected number of pages indexed?
- [ ] No important pages accidentally noindexed
- [ ] Search Console coverage report reviewed
- [ ] No "Crawled — currently not indexed" issues at scale
- [ ] No "Discovered — currently not indexed" issues at scale

### Canonicals
- [ ] Canonical tags present on paginated pages
- [ ] Self-referencing canonicals on all key pages
- [ ] No canonical conflicts (canonical pointing to noindexed page)
- [ ] Hreflang and canonicals aligned (multilingual sites)

### Redirects
- [ ] No redirect chains (A → B → C — should be A → C)
- [ ] No redirect loops
- [ ] All 404 pages for previously-ranking URLs redirected
- [ ] Old domain fully redirected (post-migration)

---

## Section 2: Technical Foundation

### Page Speed & Core Web Vitals
- [ ] **LCP** (Largest Contentful Paint) < 2.5 seconds
- [ ] **INP** (Interaction to Next Paint) < 200ms
- [ ] **CLS** (Cumulative Layout Shift) < 0.1
- [ ] PageSpeed Insights score: aim for 70+ on mobile
- [ ] Images compressed and in next-gen format (WebP/AVIF)
- [ ] Render-blocking JavaScript deferred or async
- [ ] Critical CSS inlined
- [ ] CDN in use for static assets
- [ ] Server response time < 200ms (TTFB)

### Mobile Usability
- [ ] Passes Google Mobile-Friendly Test
- [ ] No text too small to read
- [ ] No clickable elements too close together
- [ ] No content wider than screen
- [ ] No Flash used

### HTTPS & Security
- [ ] Full HTTPS — no pages on HTTP
- [ ] No mixed content warnings
- [ ] SSL certificate valid and not expiring soon
- [ ] HSTS header in place

### URL Structure
- [ ] URLs lowercase
- [ ] Words separated by hyphens (not underscores)
- [ ] No unnecessary parameters in URLs
- [ ] Short and descriptive (no keyword stuffing)
- [ ] Consistent trailing slash policy
- [ ] www / non-www resolved with 301 to preferred version

### Duplicate Content
- [ ] www and non-www redirect to one version
- [ ] HTTP and HTTPS redirect to one version
- [ ] Trailing slash resolved (domain.com/ vs domain.com)
- [ ] No duplicate content from URL parameters (add to GSC parameter handling or use canonicals)
- [ ] Paginated pages handled (canonicals or consolidation)
- [ ] Printer-friendly pages excluded or canonicalised

---

## Section 3: On-Page SEO

### Title Tags
- [ ] Unique title tag on every page
- [ ] Primary keyword near the beginning
- [ ] Under 60 characters (to avoid truncation in SERPs)
- [ ] Compelling — would you click it?
- [ ] Brand name at end (if applicable): `Keyword — Page Topic | Brand`
- [ ] No keyword stuffing

### Meta Descriptions
- [ ] Unique meta description on every important page
- [ ] Under 155 characters
- [ ] Includes primary keyword naturally
- [ ] Contains a value proposition or call to action
- [ ] Written to earn the click, not just describe the page

### Heading Structure
- [ ] One H1 per page — contains primary keyword
- [ ] H2s cover major subtopics
- [ ] H3s used for supporting points under H2s
- [ ] No heading tags used for styling (bold text ≠ H2)
- [ ] Logical hierarchy — no H3 before H2

### Content
- [ ] Primary keyword in first 100 words
- [ ] Content matches search intent for target keyword
- [ ] Content depth matches or exceeds top 3 ranking pages
- [ ] No thin content (under 300 words with no unique value)
- [ ] No keyword stuffing
- [ ] Natural use of related terms and synonyms (semantic SEO)
- [ ] Last updated date accurate and visible (for content that ages)

### Images
- [ ] Descriptive file names (blue-widget.jpg not IMG_4839.jpg)
- [ ] Alt text on all images — descriptive, includes keyword where natural
- [ ] Compressed to appropriate size
- [ ] Lazy loading enabled for below-fold images

### Internal Linking
- [ ] Key money pages receive internal links from high-authority pages
- [ ] Anchor text is descriptive (not "click here")
- [ ] No orphan pages (pages with zero internal links)
- [ ] Navigation links flow authority to key pages
- [ ] Related posts / related products linked where relevant

### URL
- [ ] Primary keyword in URL
- [ ] URL is short (3–5 words)
- [ ] No stop words in URL (the, a, and, etc.)
- [ ] No dates in URL for evergreen content

---

## Section 4: Structured Data

- [ ] Schema markup present on relevant page types
- [ ] Organisation schema on homepage
- [ ] Product schema on product/pricing pages
- [ ] Article/BlogPosting schema on blog posts
- [ ] FAQ schema where FAQs exist
- [ ] BreadcrumbList schema on inner pages
- [ ] Review/AggregateRating schema where reviews exist
- [ ] Validated with Google Rich Results Test
- [ ] No errors or warnings in Search Console Rich Results report

---

## Section 5: Authority & Off-Page

### Backlink Profile
- [ ] Backlink count and quality reviewed (Ahrefs, Moz, Semrush)
- [ ] Domain Rating / Domain Authority benchmarked vs. top competitors
- [ ] No manual actions in Search Console
- [ ] Toxic/spammy links identified (consider disavow if significant)
- [ ] Anchor text distribution natural (not over-optimised for money keywords)

### Internal Authority Flow
- [ ] Homepage passes authority to key category/product pages
- [ ] High-traffic blog posts link to relevant money pages
- [ ] Footer and navigation links prioritise key pages

### Local SEO (if applicable)
- [ ] Google Business Profile claimed and optimised
- [ ] NAP (Name, Address, Phone) consistent across directories
- [ ] Local schema markup on contact/location pages
- [ ] Reviews being generated and responded to

---

## Audit Status Key

Use this when recording audit results:

| Status | Meaning |
|---|---|
| ✅ Pass | No issue found |
| ⚠️ Improve | Not broken, but could be better |
| 🔴 Fix | Issue found — needs fixing |
| N/A | Not applicable to this site |
| ? | Needs investigation |
