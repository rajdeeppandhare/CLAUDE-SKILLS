# Meta Tags Complete Reference

## Essential Tags (every page needs these)

```html
<!-- Page title — most important SEO tag -->
<title>Primary Keyword - Secondary Keyword | Brand Name</title>
<!-- Rules: 50-60 chars, include primary keyword near start, unique per page -->

<!-- Meta description — shown in search results -->
<meta name="description" content="Compelling description with primary keyword. Tell users what they'll get. Include a soft CTA.">
<!-- Rules: 150-160 chars, include keyword naturally, unique per page, write for humans -->

<!-- Canonical — prevents duplicate content -->
<link rel="canonical" href="https://yoursite.com/exact-page-url/">
<!-- Always use absolute URLs, include trailing slash consistently -->

<!-- Robots — control indexing -->
<meta name="robots" content="index, follow">
<!-- Options: index/noindex, follow/nofollow, max-snippet:-1, max-image-preview:large -->
```

## Open Graph Tags (social sharing)

```html
<meta property="og:title" content="Page Title Here">
<meta property="og:description" content="Description for social shares (can differ from meta description)">
<meta property="og:image" content="https://yoursite.com/images/og-image.jpg">
<!-- OG image: 1200x630px, under 1MB, shows in Facebook/LinkedIn/WhatsApp previews -->
<meta property="og:url" content="https://yoursite.com/page-url/">
<meta property="og:type" content="website"> <!-- or article, product, etc -->
<meta property="og:site_name" content="Your Brand Name">
```

## Twitter Card Tags

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Page Title">
<meta name="twitter:description" content="Description for Twitter">
<meta name="twitter:image" content="https://yoursite.com/images/twitter-image.jpg">
<!-- Twitter image: 1200x628px minimum -->
<meta name="twitter:site" content="@yourtwitterhandle">
```

## Technical Tags

```html
<!-- Viewport — critical for mobile SEO -->
<meta name="viewport" content="width=device-width, initial-scale=1">

<!-- Language -->
<html lang="en">

<!-- Charset -->
<meta charset="UTF-8">

<!-- Refresh/redirect (avoid for SEO — use server-side 301 instead) -->
<!-- <meta http-equiv="refresh" content="0;url=https://newurl.com"> -->
```

## Title Tag Formulas

**Blog post**: `Target Keyword: How to Do X (Year) | Brand`
**Product page**: `Product Name - Key Benefit | Brand`
**Category page**: `Category Name - Browse [N] Products | Brand`
**Homepage**: `Brand Name - Primary Value Proposition`
**Local business**: `Service in City | Business Name`

## Common Mistakes

| ❌ Wrong | ✅ Right |
|---|---|
| `<title>Home</title>` | `<title>Affordable Web Design London | BrandCo</title>` |
| Title over 60 chars | Keep to 50-60 chars |
| Same title on every page | Unique title per page |
| Missing meta description | Write unique 150-160 char description |
| Meta keywords tag | Ignore — Google doesn't use it |
| Multiple canonical tags | One canonical per page only |
