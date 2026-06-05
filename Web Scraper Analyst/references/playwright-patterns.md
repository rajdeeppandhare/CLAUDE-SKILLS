# Playwright Patterns Reference

> Load this file when writing scraper scripts. Pick the pattern that matches the site type.

---

## Table of Contents
1. [Static HTML Tables](#static-tables)
2. [E-Commerce Product Listings](#ecommerce)
3. [News & Article Feeds](#news)
4. [JS-Heavy SPAs](#spa)
5. [Infinite Scroll](#infinite-scroll)
6. [Pagination Patterns](#pagination)
7. [Anti-Bot & Stealth](#anti-bot)
8. [Authentication](#auth)
9. [Error Handling](#errors)
10. [Performance Tips](#performance)

---

## Static HTML Tables {#static-tables}

Best for: Wikipedia, financial data sites, government data, sports stats.

```python
async def scrape_tables(page, url):
    await page.goto(url, wait_until="domcontentloaded")
    
    # Extract all tables
    tables = await page.evaluate("""
        () => {
            const results = [];
            document.querySelectorAll('table').forEach((table, tableIdx) => {
                const headers = [...table.querySelectorAll('th')]
                    .map(th => th.innerText.trim().toLowerCase().replace(/\s+/g, '_'));
                
                const rows = [...table.querySelectorAll('tbody tr')].map(row => {
                    const cells = [...row.querySelectorAll('td')].map(td => td.innerText.trim());
                    return Object.fromEntries(headers.map((h, i) => [h, cells[i] ?? '']));
                });
                
                if (rows.length > 0) results.push({ table_index: tableIdx, rows });
            });
            return results;
        }
    """)
    return tables

# Target a specific table by index or header content
async def scrape_specific_table(page, url, table_index=0):
    await page.goto(url, wait_until="domcontentloaded")
    rows = await page.evaluate(f"""
        () => {{
            const table = document.querySelectorAll('table')[{table_index}];
            if (!table) return [];
            const headers = [...table.querySelectorAll('th, thead td')]
                .map(h => h.innerText.trim().toLowerCase().replace(/\\s+/g, '_'));
            return [...table.querySelectorAll('tbody tr')].map(row => {{
                const cells = [...row.querySelectorAll('td')].map(td => td.innerText.trim());
                return Object.fromEntries(headers.map((h, i) => [h, cells[i] ?? '']));
            }});
        }}
    """)
    return rows
```

---

## E-Commerce Product Listings {#ecommerce}

Best for: Amazon, eBay, Shopify stores, any product grid.

```python
async def scrape_products(page, url, card_selector, field_map):
    """
    field_map example:
    {
        "name": ".product-title",
        "price": ".price-now",
        "rating": "[data-rating]",
        "review_count": ".reviews-count",
        "image_url": "img[src]",
        "product_url": "a[href]"
    }
    """
    await page.goto(url, wait_until="networkidle")
    
    products = await page.evaluate(f"""
        (cardSelector, fieldMap) => {{
            return [...document.querySelectorAll(cardSelector)].map(card => {{
                const item = {{}};
                for (const [field, selector] of Object.entries(fieldMap)) {{
                    const el = card.querySelector(selector);
                    if (!el) {{ item[field] = ''; continue; }}
                    // Get href for links, src for images, text otherwise
                    if (selector.includes('[href]')) item[field] = el.href;
                    else if (selector.includes('[src]')) item[field] = el.src;
                    else if (selector.includes('[data-')) {{
                        const attr = selector.match(/\[([^\]]+)\]/)[1];
                        item[field] = el.getAttribute(attr);
                    }}
                    else item[field] = el.innerText.trim();
                }}
                return item;
            }});
        }}
    """, card_selector, field_map)
    
    return products

# Price cleaning helper
def clean_price(price_str):
    """Convert '£49.99' or '$1,299.00' to float 49.99"""
    import re
    cleaned = re.sub(r'[^\d.]', '', price_str)
    try:
        return float(cleaned)
    except ValueError:
        return None
```

---

## News & Article Feeds {#news}

Best for: BBC, Reuters, blog feeds, news aggregators.

```python
async def scrape_articles(page, url):
    await page.goto(url, wait_until="networkidle")
    
    articles = await page.evaluate("""
        () => {
            // Try common article selectors
            const selectors = ['article', '.article-card', '.post-item', '[data-testid="article"]'];
            let items = [];
            
            for (const sel of selectors) {
                items = document.querySelectorAll(sel);
                if (items.length > 0) break;
            }
            
            return [...items].map(article => ({
                headline: (
                    article.querySelector('h1, h2, h3, .headline, .title')?.innerText ?? ''
                ).trim(),
                summary: (
                    article.querySelector('p, .summary, .description, .excerpt')?.innerText ?? ''
                ).trim().substring(0, 500),
                author: (
                    article.querySelector('.author, .byline, [rel="author"]')?.innerText ?? ''
                ).trim(),
                date: (
                    article.querySelector('time, .date, .published')?.getAttribute('datetime') ??
                    article.querySelector('time, .date, .published')?.innerText ?? ''
                ).trim(),
                url: (
                    article.querySelector('a[href]')?.href ?? ''
                ),
                category: (
                    article.querySelector('.category, .tag, .section')?.innerText ?? ''
                ).trim()
            })).filter(a => a.headline.length > 0);
        }
    """)
    return articles
```

---

## JS-Heavy SPAs {#spa}

Best for: React/Vue/Angular apps, dashboards, dynamic content.

```python
async def scrape_spa(page, url, content_selector, wait_selector=None):
    # Use networkidle for SPAs — waits for all XHR/fetch to complete
    await page.goto(url, wait_until="networkidle", timeout=30000)
    
    # Wait for specific element if known
    if wait_selector:
        await page.wait_for_selector(wait_selector, timeout=15000)
    
    # Wait for React hydration / Vue mounting
    await page.wait_for_load_state("networkidle")
    await asyncio.sleep(1)  # Brief settle for animations
    
    # Intercept API responses (powerful for data-heavy SPAs)
    api_data = []
    
    async def handle_response(response):
        if "api" in response.url and response.status == 200:
            try:
                data = await response.json()
                api_data.append({"url": response.url, "data": data})
            except:
                pass
    
    page.on("response", handle_response)
    await page.reload(wait_until="networkidle")
    
    return api_data  # Often richer than DOM scraping for SPAs

# Alternative: scroll + wait for dynamic renders
async def scrape_spa_with_scroll(page, url, item_selector):
    await page.goto(url, wait_until="networkidle")
    
    items = set()
    prev_count = 0
    
    for _ in range(10):  # Max scroll attempts
        current_items = await page.query_selector_all(item_selector)
        if len(current_items) == prev_count:
            break  # Nothing new loaded
        prev_count = len(current_items)
        await page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
        await asyncio.sleep(2)
    
    return await page.evaluate(f"""
        () => [...document.querySelectorAll('{item_selector}')]
              .map(el => el.innerText.trim())
    """)
```

---

## Infinite Scroll {#infinite-scroll}

Best for: Twitter/X, LinkedIn feeds, Pinterest, any scroll-to-load.

```python
async def scrape_infinite_scroll(page, url, item_selector, max_scrolls=20):
    await page.goto(url, wait_until="networkidle")
    
    all_items = []
    seen_ids = set()
    scroll_count = 0
    no_new_content_streak = 0
    
    while scroll_count < max_scrolls and no_new_content_streak < 3:
        # Extract current items
        new_items = await page.evaluate(f"""
            () => [...document.querySelectorAll('{item_selector}')].map(el => ({{
                text: el.innerText.trim(),
                html_id: el.id || '',
                data_id: el.getAttribute('data-id') || el.getAttribute('data-key') || ''
            }}))
        """)
        
        added = 0
        for item in new_items:
            uid = item.get('data_id') or item.get('html_id') or item['text'][:50]
            if uid not in seen_ids:
                seen_ids.add(uid)
                all_items.append(item)
                added += 1
        
        if added == 0:
            no_new_content_streak += 1
        else:
            no_new_content_streak = 0
        
        print(f"Scroll {scroll_count + 1}: +{added} items (total: {len(all_items)})")
        
        # Scroll to bottom
        await page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
        await asyncio.sleep(2)  # Wait for new content to load
        scroll_count += 1
    
    print(f"Infinite scroll complete: {len(all_items)} total items")
    return all_items
```

---

## Pagination Patterns {#pagination}

### Pattern A — Next Button

```python
async def paginate_next_button(page, url, item_selector, max_pages=50):
    all_items = []
    await page.goto(url, wait_until="networkidle")
    
    next_selectors = [
        '[aria-label="Next page"]',
        '[aria-label="Next"]',
        'a:has-text("Next")',
        'button:has-text("Next")',
        '.pagination-next',
        '[rel="next"]',
        'a:has-text("›")',
        'a:has-text("→")',
    ]
    
    for page_num in range(1, max_pages + 1):
        items = await page.evaluate(f"""
            () => [...document.querySelectorAll('{item_selector}')]
                  .map(el => el.innerText.trim())
        """)
        all_items.extend(items)
        print(f"Page {page_num}: {len(items)} items (total: {len(all_items)})")
        
        # Find next button
        next_btn = None
        for sel in next_selectors:
            try:
                next_btn = await page.query_selector(sel)
                if next_btn:
                    is_disabled = await next_btn.get_attribute("disabled")
                    aria_disabled = await next_btn.get_attribute("aria-disabled")
                    if is_disabled or aria_disabled == "true":
                        next_btn = None
                    break
            except:
                continue
        
        if not next_btn:
            print(f"No next button found — stopping at page {page_num}")
            break
        
        await next_btn.click()
        await page.wait_for_load_state("networkidle")
        await asyncio.sleep(1)
    
    return all_items
```

### Pattern B — URL Parameter Pagination

```python
async def paginate_url_params(page, base_url, item_selector, 
                               param="page", start=1, max_pages=50):
    """
    Works for: ?page=1, ?page=2 ... or ?offset=0, ?offset=20 ...
    """
    all_items = []
    
    for page_num in range(start, start + max_pages):
        url = f"{base_url}?{param}={page_num}"
        await page.goto(url, wait_until="networkidle")
        
        items = await page.evaluate(f"""
            () => [...document.querySelectorAll('{item_selector}')]
                  .map(el => el.innerText.trim()).filter(t => t.length > 0)
        """)
        
        if not items:
            print(f"No items on page {page_num} — stopping")
            break
        
        all_items.extend(items)
        print(f"Page {page_num}: {len(items)} items")
        await asyncio.sleep(1.5)  # Polite delay
    
    return all_items

### Pattern C — Load More Button

```python
async def click_load_more(page, url, item_selector, 
                           load_more_selector=None, max_clicks=30):
    await page.goto(url, wait_until="networkidle")
    
    load_more_selectors = load_more_selector or [
        'button:has-text("Load more")',
        'button:has-text("Show more")',
        'button:has-text("View more")',
        '[data-testid="load-more"]',
        '.load-more-btn',
    ]
    
    clicks = 0
    while clicks < max_clicks:
        btn = None
        for sel in (load_more_selectors if isinstance(load_more_selectors, list) 
                    else [load_more_selectors]):
            try:
                btn = await page.query_selector(sel)
                if btn and await btn.is_visible():
                    break
                btn = None
            except:
                continue
        
        if not btn:
            break
        
        await btn.click()
        await page.wait_for_load_state("networkidle")
        await asyncio.sleep(1)
        clicks += 1
        print(f"Loaded more ({clicks}/{max_clicks})")
    
    return await page.query_selector_all(item_selector)
```

---

## Anti-Bot & Stealth {#anti-bot}

```python
async def stealth_context(playwright):
    """Browser context that reduces bot detection fingerprint"""
    browser = await playwright.chromium.launch(
        headless=True,
        args=[
            "--disable-blink-features=AutomationControlled",
            "--no-sandbox",
            "--disable-dev-shm-usage",
        ]
    )
    context = await browser.new_context(
        viewport={"width": 1366, "height": 768},
        user_agent=(
            "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
            "AppleWebKit/537.36 (KHTML, like Gecko) "
            "Chrome/124.0.0.0 Safari/537.36"
        ),
        locale="en-GB",
        timezone_id="Europe/London",
        extra_http_headers={
            "Accept-Language": "en-GB,en;q=0.9",
            "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
        }
    )
    
    # Remove webdriver flag
    await context.add_init_script("""
        Object.defineProperty(navigator, 'webdriver', { get: () => undefined });
        Object.defineProperty(navigator, 'plugins', { get: () => [1, 2, 3] });
    """)
    
    return browser, context

# Polite delay between requests
import random

async def polite_delay(min_s=1.0, max_s=3.0):
    await asyncio.sleep(random.uniform(min_s, max_s))
```

**When to flag anti-bot issues:**
- Cloudflare challenge page detected → tell user, suggest Playwright with stealth or manual cookies
- CAPTCHA present → flag, cannot automate without third-party service
- IP rate limiting → suggest reducing speed or using proxies
- Login wall → see Authentication section

---

## Authentication {#auth}

```python
async def login_and_scrape(page, login_url, username, password, 
                            username_selector, password_selector, 
                            submit_selector, success_indicator):
    """
    User must provide credentials. Never hardcode or store.
    """
    await page.goto(login_url)
    await page.fill(username_selector, username)
    await page.fill(password_selector, password)
    await page.click(submit_selector)
    
    # Wait for successful login
    await page.wait_for_selector(success_indicator, timeout=10000)
    print("Login successful")
    
    # Save session state for reuse
    await page.context.storage_state(path="session.json")
    return page

# Reuse saved session
async def load_session(playwright, session_path="session.json"):
    browser = await playwright.chromium.launch(headless=True)
    context = await browser.new_context(storage_state=session_path)
    return browser, await context.new_page()
```

---

## Error Handling {#errors}

```python
async def safe_extract(page, selector, attribute=None, default=""):
    """Extract a single element safely — never crash on missing element"""
    try:
        el = await page.query_selector(selector)
        if not el:
            return default
        if attribute:
            return await el.get_attribute(attribute) or default
        return (await el.inner_text()).strip() or default
    except Exception:
        return default

async def safe_page_scrape(page, url, scrape_fn, retries=3):
    """Retry a scrape function with exponential backoff"""
    for attempt in range(retries):
        try:
            await page.goto(url, wait_until="networkidle", timeout=30000)
            return await scrape_fn(page)
        except PlaywrightTimeout:
            print(f"Timeout on {url} (attempt {attempt + 1}/{retries})")
            await asyncio.sleep(2 ** attempt)
        except Exception as e:
            print(f"Error on {url}: {e} (attempt {attempt + 1}/{retries})")
            await asyncio.sleep(2 ** attempt)
    print(f"Failed to scrape {url} after {retries} attempts")
    return []
```

---

## Performance Tips {#performance}

```python
# Block unnecessary resources to speed up scraping
async def fast_context(playwright):
    browser = await playwright.chromium.launch(headless=True)
    context = await browser.new_context()
    
    # Block images, fonts, media — keep HTML/CSS/JS
    await context.route("**/*.{png,jpg,jpeg,gif,webp,svg,ico}", 
                        lambda route: route.abort())
    await context.route("**/*.{woff,woff2,ttf,otf}", 
                        lambda route: route.abort())
    await context.route("**/analytics*", lambda route: route.abort())
    await context.route("**/tracking*", lambda route: route.abort())
    await context.route("**/ads*", lambda route: route.abort())
    
    return browser, context

# Run multiple pages in parallel
async def scrape_urls_parallel(urls, scrape_fn, concurrency=5):
    semaphore = asyncio.Semaphore(concurrency)
    
    async def scrape_with_limit(url):
        async with semaphore:
            async with async_playwright() as p:
                browser = await p.chromium.launch(headless=True)
                page = await browser.new_page()
                try:
                    return await scrape_fn(page, url)
                finally:
                    await browser.close()
    
    results = await asyncio.gather(*[scrape_with_limit(url) for url in urls])
    return [item for sublist in results for item in sublist]  # Flatten
```

---

## Installation Commands

```bash
# Install Playwright
pip install playwright

# Install browsers (first time only)
playwright install chromium

# Or install all browsers
playwright install
```

---

*Patterns tested against Playwright 1.44+. Always check robots.txt before scraping.*
