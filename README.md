# Smithsonian Art Store

**A dynamic e-commerce storefront that transforms public domain museum artwork into purchasable prints — powered by the Smithsonian Open Access API.**

> 🔗 **[Live Demo](https://nathanmcmullendev.github.io/etsy-smithsonian/)** · Test Card: `4242 4242 4242 4242`

![Smithsonian Art Store](https://res.cloudinary.com/dh4qwuvuo/image/upload/v1765827687/rapidwoo/products/ke3oee48rrmtlkrft9fm.png)

---

## Why This Exists

Museum collections contain thousands of public domain artworks that could be beautiful prints — but accessing them requires navigating complex APIs and metadata structures. This project transforms raw museum data into a shoppable storefront with real checkout functionality.

**What it demonstrates:**
- Consuming a real public API (Smithsonian Open Access)
- Transforming messy museum metadata into clean product data
- Building an Etsy-style UI from scratch
- Integrating checkout without a backend (Snipcart)

---

## Key Engineering Challenges

| Challenge | Solution |
|-----------|----------|
| **Inconsistent API responses** | Defensive extraction functions that handle missing/malformed data |
| **Image optimization** | Dynamic sizing via Smithsonian's IIIF-compatible URLs |
| **Search with debouncing** | 300ms debounce prevents API hammering on keystroke |
| **Product detail routing** | Hash-based SPA routing without a framework |
| **Cart persistence** | Snipcart handles state across page navigation |
| **Fallback handling** | Graceful degradation when API fails or returns empty |

---

## How It Works

```
User searches "Georgia O'Keeffe"
         ↓
Smithsonian API returns raw JSON
         ↓
Transform: extract title, artist, image, metadata
         ↓
Render: Etsy-style product cards
         ↓
Click: Product detail page with size/frame options
         ↓
Add to Cart: Snipcart handles checkout
```

---

## Features

### Search & Filter
- Free-text search across Smithsonian American Art Museum
- Pre-populated artist dropdown (Georgia O'Keeffe, Edward Hopper, etc.)
- Configurable result count

### Product Cards
- Museum-quality image previews
- Artist attribution
- "Smithsonian" source badge
- Quick-view options

### Product Detail Page
- High-resolution image with lightbox
- Size selection (8×10 to 24×36)
- Frame options (Unframed, Black, White, Natural)
- Dynamic pricing based on selections
- Full artwork metadata (medium, dimensions, accession number)

### Checkout
- Snipcart integration
- Real cart persistence
- Test mode for demonstration

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Data Source | Smithsonian Open Access API |
| Frontend | Vanilla JavaScript (ES6+) |
| Styling | Custom CSS (Etsy-inspired) |
| Checkout | Snipcart v3.2 |
| Hosting | GitHub Pages |

**No build step. No framework. No dependencies (except Snipcart).**

---

## API Integration

The Smithsonian Open Access API provides CC0-licensed artwork:

```javascript
const query = `unit_code:SAAM AND media_usage:CC0 AND (${searchTerm})`;
const response = await fetch(`https://api.si.edu/openaccess/api/v1.0/search?api_key=${API_KEY}&q=${query}`);
```

### Data Extraction

Museum data is deeply nested and inconsistent. The extraction layer handles:
- Arrays vs objects
- Missing fields
- Nested content structures
- Multiple image resolutions

```javascript
function extractContent(data) {
  if (!data) return "";
  if (Array.isArray(data)) return data[0]?.content || data[0] || "";
  return data.content || data.toString();
}
```

---

## What I Learned

1. **Public APIs are messy** — real-world data requires defensive coding and graceful fallbacks.

2. **Debouncing matters** — without it, search hammers the API on every keystroke.

3. **Hash routing works** — for a single-page feel without a framework, `window.location.hash` is sufficient.

4. **Etsy's UI is deceptively complex** — badges, ratings, pricing tiers, and hover states add up.

---

## Try It

1. Visit the [live demo](https://nathanmcmullendev.github.io/etsy-smithsonian/)
2. Search for an artist or term
3. Click a product to see details
4. Add to cart (test card: `4242 4242 4242 4242`)

---

## Related Project

**[RapidWoo](https://github.com/nathanmcmullendev/claude)** — A serverless e-commerce system with browser-based product editor. Uses similar patterns (Snipcart, GitHub persistence) but focuses on creating/managing products rather than consuming external data.

---

## License

MIT

---

*Built by [Nathan McMullen](https://github.com/nathanmcmullendev)*
