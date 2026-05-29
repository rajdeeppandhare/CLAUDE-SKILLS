# Schema Markup Guide (Structured Data)

Schema markup helps Google show rich results (stars, FAQs, prices, etc.) in search.
Always use JSON-LD format inside `<script type="application/ld+json">` in the `<head>`.

## Article / Blog Post

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Your Article Title",
  "description": "Brief description of the article",
  "image": "https://yoursite.com/image.jpg",
  "author": {
    "@type": "Person",
    "name": "Author Name",
    "url": "https://yoursite.com/author/name"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Brand Name",
    "logo": {
      "@type": "ImageObject",
      "url": "https://yoursite.com/logo.png"
    }
  },
  "datePublished": "2024-01-15",
  "dateModified": "2024-06-01"
}
```

## FAQ Page (triggers FAQ rich results in Google)

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is your question here?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Your answer here. Can include HTML."
      }
    },
    {
      "@type": "Question",
      "name": "Second question?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Second answer."
      }
    }
  ]
}
```

## Local Business

```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Business Name",
  "image": "https://yoursite.com/image.jpg",
  "url": "https://yoursite.com",
  "telephone": "+44-20-1234-5678",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main Street",
    "addressLocality": "London",
    "postalCode": "SW1A 1AA",
    "addressCountry": "GB"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 51.5074,
    "longitude": -0.1278
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
      "opens": "09:00",
      "closes": "17:00"
    }
  ]
}
```

## Product Page

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Product Name",
  "image": "https://yoursite.com/product.jpg",
  "description": "Product description",
  "brand": {
    "@type": "Brand",
    "name": "Brand Name"
  },
  "offers": {
    "@type": "Offer",
    "url": "https://yoursite.com/product",
    "priceCurrency": "GBP",
    "price": "29.99",
    "availability": "https://schema.org/InStock"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.5",
    "reviewCount": "127"
  }
}
```

## Breadcrumb

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://yoursite.com"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Category",
      "item": "https://yoursite.com/category"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "This Page"
    }
  ]
}
```

## Validation
Always test schema at: https://search.google.com/test/rich-results
