# Wagging A Good Time — Product & Business Playbook

**Brand:** Wagging A Good Time (waggingagoodtime.com)
**Tagline:** Better adventures for dogs and their humans.
**Model:** Curated hybrid ecommerce — one storefront, three fulfillment engines.
**Launch target:** 20–30 hand-picked products. Curated, never cluttered.

---

## 1. The Business in One Paragraph

Wagging A Good Time sells dog travel and adventure gear through a single curated catalog. Behind the scenes, every product is fulfilled one of three ways: **dropshipped direct** (CJdropshipping — we keep the retail margin), **print-on-demand** (Printify — our own branded merch), or **affiliate referral** (Amazon, Chewy, Walmart, eBay — we earn commission). The customer never sees the seams: every product card looks identical, only the button changes. The site is plain HTML/CSS/JS with a data-driven product layer, so adding or swapping a product means editing one record — no templates, no rebuilds, no plugins.

---

## 2. Revenue Streams

| Stream | How we earn | Margin profile | Role in the business |
|---|---|---|---|
| **Direct (CJdropshipping)** | Retail price − CJ product/shipping cost | Highest (typically 50–70%) | Core profit engine |
| **Print-on-Demand (Printify)** | Retail price − Printify base + shipping | High (typically 40–60%) | Brand building, zero inventory |
| **Affiliate (Amazon, Chewy, Walmart, eBay)** | Commission on referred sale | Low (1–10%) | Fills catalog gaps, monetizes products we can't beat on price, SEO content revenue |

**Strategic logic:**

- If we can source it well and fulfill it profitably → **direct**.
- If it's branded "Wagging A Good Time" merch → **Printify**.
- If it's a big-brand item customers will buy anyway (or it's price-competitive only at a major retailer) → **affiliate**.
- Never list the same item in two channels. One product, one fulfillment path, one best offer.

---

## 3. The Product Database (the heart of the system)

Every product is **one record** in the `PRODUCTS` array in `index.html`. The storefront renders the correct purchase button automatically from `purchase_type`.

### 3.1 Record schema

```js
{
  title: "Portable Dog Water Bottle with Bowl",
  slug: "portable-dog-water-bottle",      // future URL: /products/<slug>
  category: "Travel",                      // must match a category tile
  image: "assets/product-bottle.jpg",
  purchase_type: "direct",                 // direct | cj | printify | affiliate
  merchant: "wagging",                     // wagging | amazon | chewy | walmart | ebay
  price: 22.99,
  rating: 4.7,
  reviews: 3800,
  badge: null,                             // "staff" | "pod" | null
  affiliate_url: null                      // required when purchase_type is "affiliate"
}
```

### 3.2 How purchase_type drives the button

| purchase_type | Button rendered | Click behavior |
|---|---|---|
| `direct` / `cj` / `printify` | **Add to Cart** (orange, filled) | Our checkout (Stripe, coming in Phase 3) |
| `affiliate` | **"View at {Merchant}"** (outlined) | Opens merchant in new tab with `rel="nofollow sponsored noopener"` + "affiliate link" note under the button |

Merchant button labels live in one map — edit once, applies everywhere:

```js
const MERCHANT_BUTTONS = {
  amazon:  "Check Amazon Price",
  chewy:   "View at Chewy",
  walmart: "View at Walmart",
  ebay:    "See Current Deals"
};
```

### 3.3 Rules of the data layer

1. **Adding a product = adding one record.** Nothing else changes.
2. **Swapping suppliers = editing one field.** Change `purchase_type` and `merchant`; the button follows.
3. **Affiliate products always disclose.** The template prints "Sold by {Merchant} · affiliate link" automatically — never strip it.
4. **Badges are intentional.** `staff` = we vouch for it personally; `pod` = our own brand. Don't badge more than ~30% of the catalog or they lose meaning.

---

## 4. Supplier Playbooks

### 4.1 CJdropshipping (direct fulfillment)

**Use for:** travel gear, car/cleanup products, outdoor essentials — the volume sellers.

- Source products with US warehouses where possible (target: 5–8 day delivery, not 3 weeks).
- Order a sample of anything before it becomes a Staff Pick. No exceptions.
- Margin floor: **2.5× landed cost** (product + shipping). If a product can't hold 2.5× at a competitive retail price, it goes affiliate or it goes away.
- Set retail prices ending in .99; keep hero SKUs in the $19.99–$49.99 band (impulse-friendly, margin-rich).
- Track CJ cost changes monthly; a $2 cost increase on a $24.99 item is a margin event — reprice or swap.

### 4.2 Printify (print-on-demand, our brand)

**Use for:** Wagging A Good Time branded merch — towels, tees, mugs, bandanas, tote bags.

- Brand-first products: they exist to make customers feel like pack members, margin second.
- Always order one sample per design before listing (print quality varies by provider).
- Prefer providers with US print facilities for delivery speed.
- Badge these `pod` ("Our Brand") — it's a trust and story signal, not just a label.
- Seasonal drops (summer beach drop, holiday drop) create urgency without inventory risk.

### 4.3 Affiliate partners (Amazon, Chewy, Walmart, eBay)

**Use for:** big-brand items, electronics (GPS trackers, cameras), premium gear we can't source competitively.

- Amazon Associates: strict rules — prices must come via their API (or be omitted), links need the Associate tag, disclosure required. Commission ~1–3%.
- Chewy (via Impact/partner networks): pet-specialist audience fit, better commissions than Amazon.
- Walmart & eBay: deal-hunting angle — "See Current Deals" framing works for price-sensitive items.
- Paste the full affiliate URL into the record's `affiliate_url` field. The `#` placeholders in the launch catalog mark where links go.
- **FTC compliance (non-negotiable):** sitewide disclosure in the footer (already live) + per-product "affiliate link" note (automatic). Guides with affiliate links get a disclosure line near the first link too.

---

## 5. Product Selection Scorecard

Every candidate product scores 0–2 on each criterion. **List only 8+ scorers; 10+ earns Staff Pick consideration.**

| Criterion | 0 | 1 | 2 |
|---|---|---|---|
| Adventure fit | Generic pet product | Useful at home + travel | Made for adventures |
| Margin | <2× landed cost | 2–2.5× | 2.5×+ |
| Photo quality | Supplier photos only, weak | Usable supplier photos | Great photos / we can shoot it |
| Social proof | No reviews anywhere | Some reviews | 1,000+ reviews at 4.5★+ |
| Shipping speed | 3+ weeks | ~2 weeks | US warehouse, <8 days |
| Story | Hard to describe | Easy to describe | Sells itself in one sentence |

**Kill criteria (any one = don't list):** safety concerns, trademark/branded knockoff risk, can't be described honestly as high quality.

---

## 6. Launch Catalog Plan (20–30 products)

### 6.1 Category mix targets

| Category | Products | Primary fulfillment |
|---|---|---|
| Travel (bottles, bowls, carriers) | 5–6 | Direct |
| Car & Cleanup (seat covers, rollers) | 4–5 | Direct |
| Walks & Outdoors (harnesses, packs, boots) | 4–5 | Direct + Affiliate |
| Beach (towels, cooling, floatation) | 3–4 | Direct + POD |
| Play (toys, sunglasses, fetch gear) | 3–4 | Affiliate + Direct |
| Gifts / Our Brand (POD merch) | 3–4 | Printify |
| **Total** | **22–28** | ~60% direct, ~20% POD, ~20% affiliate |

### 6.2 Already live on the homepage (6 products)

Reusable pet hair roller (direct, Staff Pick) · Portable water bottle (direct) · Collapsible bowl (direct) · Dog sunglasses (affiliate/Chewy) · Back seat protector (direct, Staff Pick) · WAGT beach towel (Printify, Our Brand).

### 6.3 Expansion principles

- Add products in **themed batches** that match a guide (e.g. the Road Trip guide launch adds tether, seat cover, water bottle, cleanup kit).
- Every new product must belong to at least one existing or planned guide. Products without content support don't get traffic; content without products doesn't earn.
- Review catalog monthly: bottom 10% by clicks → re-shoot, reprice, or remove.

---

## 7. Operations Workflows

### 7.1 Add a product (5 minutes)

1. Score it against the scorecard (§5). 8+ or it stops here.
2. Prepare one square image (~800×800, <100KB) → `assets/product-<slug>.jpg`.
3. Add one record to `PRODUCTS` with the correct `purchase_type`.
4. If affiliate: paste the tagged URL into `affiliate_url`.
5. Commit and push; deploy by copying `index.html` + `assets/` to `public_html`.

### 7.2 Order flow by type

- **Direct/CJ:** customer pays us (Stripe) → order placed at CJ → CJ ships → tracking synced to customer email.
- **Printify:** customer pays us → Printify prints & ships → tracking auto-syncs.
- **Affiliate:** customer clicks out → merchant handles everything → commission accrues in the affiliate dashboard.

### 7.3 Customer service boundaries

- We own service for **direct and POD** orders (refunds, replacements, tracking).
- Affiliate purchases are the merchant's responsibility — the site copy already makes the seller explicit, keeping us clear of support liability.
- Standard policy: 30-day "happy pup" guarantee on direct/POD — cheap insurance, big trust signal.

### 7.4 Weekly rhythm (30 min)

- Check CJ order statuses and tracking sync.
- Check affiliate dashboards for clicks/conversions.
- Review site analytics: top products, dead products, search terms used.

---

## 8. Content & SEO Engine

Guides are the traffic engine; products are the monetization layer. **One guide supports 5–8 products; every batch of products gets a guide.**

### 8.1 Launch content queue

| Priority | Guide | Products it monetizes |
|---|---|---|
| 1 | Best Dog Road Trip Accessories for 2026 | Seat cover, water bottle, bowl, tether, roller |
| 2 | How to Keep Dog Hair Out of Your Car | Roller, seat protector, cleanup kit |
| 3 | The Complete Dog Beach Packing Checklist | Towel, sunglasses, floatation, cooling gear |
| 4 | Hiking With Your Dog: Gear That Actually Helps | Pack harness, boots, bottle, first-aid |
| 5 | Wagging A Good Time Gift Guide | POD merch + affiliate picks |

### 8.2 Guide format rules

- 1,500–2,500 words, genuinely useful first, product plugs second.
- Each product mention links to its catalog card (direct/POD) or out (affiliate).
- Affiliate disclosure line right before the first affiliate link.
- Every guide ends with an email signup ("Join the pack").

---

## 9. Compliance Checklist

- [x] Sitewide affiliate disclosure in footer (live)
- [x] Per-product "affiliate link" notes (automatic in template)
- [x] `rel="nofollow sponsored"` on all affiliate links (automatic)
- [ ] Disclosure near first affiliate link in each guide (add with blog build)
- [ ] Amazon Associates: no hardcoded Amazon prices unless via API
- [ ] Privacy Policy + Terms pages before Stripe checkout goes live
- [ ] Refund/return policy page before first direct sale

---

## 10. Build Roadmap

| Phase | Deliverable | Status |
|---|---|---|
| 1 | Brand + logo + homepage with data-driven catalog | ✅ Done (repo: williamlodge/wagging-a-good-time) |
| 2 | Category page template (one template, all categories) | Next |
| 3 | Product detail page template | After categories |
| 4 | Affiliate link insertion (real tagged URLs) | With first affiliate approval |
| 5 | CJdropshipping product connection + pricing pass | With first direct order |
| 6 | Stripe checkout for direct/POD items | Phase 3 of plan |
| 7 | Printify integration + first branded drop | After Stripe |
| 8 | Blog/guide system + first 3 guides | After catalog stabilizes |
| 9 | Analytics + monthly review cadence | Ongoing |

**Guiding principle for every phase:** keep the stack simple, keep the catalog small, and make every addition earn its place — in margin, in brand, or in traffic.
