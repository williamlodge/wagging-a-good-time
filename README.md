# Wagging A Good Time

**Better adventures for dogs and their humans.**

Curated dog travel gear, outdoor essentials, and gift-worthy finds for beach days, road trips, hikes, and every tail-wagging moment in between.

## Stack

Plain HTML/CSS/JS — no build step. Deploy by dropping `index.html` + `assets/` into any web root (e.g. OpenLiteSpeed `public_html`).

## Product data layer

All products live in the `PRODUCTS` array in `index.html`. Each record carries a `purchase_type`:

| `purchase_type` | Button rendered | Fulfillment |
|---|---|---|
| `direct` / `cj` | Add to Cart | Our checkout / CJdropshipping |
| `printify` | Add to Cart | Print-on-demand (our brand) |
| `affiliate` | View at {merchant} ↗ | Amazon / Chewy / Walmart / eBay |

Add a product = add one record. No template changes needed.

## Live

https://waggingagoodtime.com
