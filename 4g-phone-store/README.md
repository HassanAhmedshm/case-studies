# 4G Phone Store — production storefront and owner admin

**Live:** [www.4g-phone.com](https://www.4g-phone.com)
**Stack:** Next.js · Supabase (Postgres, Auth, Storage) · Vercel · Sentry

## The problem

A phone retailer in Egypt was selling through direct messages. Customers had to ask for prices, ask
what was in stock, and wait for a reply. The owner had no way to change prices or hide a product
without a developer, and prices move constantly in that market.

## What I built

**Public storefront** — server-rendered and SEO-ready. Catalogue with search, filter and sort, product
detail with an image gallery, and an "Order via WhatsApp" action that opens a pre-filled message. No
cart, no online payment: the sale closes in a chat, which is how the customer already wanted to buy.

**Owner admin** — create products with multiple images, edit everything, one-click sold-out and hidden
toggles, and a price column editable inline in the product list, including bulk updates across the
whole catalogue.

## Screenshots

![Home](./screenshots/4g-home.png)

![Product page](./screenshots/4g-product.png)

The product page is the whole design in one screen: variants with their own prices, stock state,
and a single red action that opens a pre-filled WhatsApp order.

## Decisions worth reading

**The success criteria were the design.** Two numbers set the build: a full catalogue price sweep in
under five minutes, and landing-to-WhatsApp-order in three clicks or fewer. The inline-editable price
table exists because of the first. The product page has exactly one primary action because of the
second.

**Hidden products are unreachable, sold-out products stay up.** A hidden product returns nothing on
direct URL access, not just a missing card on the listing. A sold-out product keeps its page and its
SEO value but disables the order action. Losing a page from search results because a phone sold out
would have been a permanent, invisible cost.

**Storage and auth are platform services, not code I maintain.** Supabase covered Postgres, auth and
image storage on a free tier that the shop's volume actually fits, so there is no server to patch.

## What I would do differently

Price history should have been recorded from the first version. Once prices are edited in bulk, the
owner wants to know what a product cost last month, and that data cannot be recovered after the fact.
