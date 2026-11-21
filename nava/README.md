# NAVA — bilingual luxury-commerce system: design system and specification

**Status: designed and specified. Not built.** There is no application code. This is the brand system,
the product specification and a set of rendered HTML mockups — and saying so plainly is the point.

![Arabic home](./screenshots/nava-home-arabic.png)

## The problem

A two-person fashion brand in Egypt sells clothing through Instagram with no system behind it: no
catalogue, no orders, no stock control, and no way to change the homepage without a developer. Their
customers browse on a phone, in Arabic, and buy cash-on-delivery.

## What exists

**The brand and design system** — palette, type scale, spacing and sizing system, component rules,
product photography direction, and the tone of the Arabic copy. Written down, not implied.

**Thirteen rendered mockups** covering the whole product, in the order a customer meets it:
storefront home (English and Arabic), shop, product detail, cart, checkout, order confirmation; then
the admin: dashboard, products, product edit, orders, content. These are HTML files that open in a
browser and are the reference for the build, not screenshots of a running app.

![Home](./screenshots/nava-home.png)

**A build specification** — data model, routes, out-of-scope register, and a numbered decision log
covering stack, auth, money representation and order lifecycle.

## Decisions worth reading

**Money is integer piastres, never a float.** Every price is an integer number of piastres. Float
currency bugs are not a risk to be managed; they are removed by the type.

**Stock lives on the variant, never on the product.** A product is a garment; a variant is one size and
colour with its own SKU and its own count. Putting stock on the product is the mistake that forces a
rewrite the first time a size sells out.

**Order status is a state machine.** `PENDING → SHIPPED → DELIVERED`, and `PENDING → CANCELLED`.
Delivered and cancelled are terminal. The spec puts the check in the database, not in the admin UI.

**Guest checkout with an upserted customer.** No accounts for shoppers. The order's phone number
upserts a customer record, because requiring a signup to buy one dress is how the sale is lost.

**Collections replace categories.** Merchandising groups are how the brand thinks, so the data model
says collection, not category.

**The scope register is a feature.** Returns, wishlist, reviews, multi-currency, customer accounts,
card payments, dark mode and email notifications are each written down as deliberately out of v1, with
the reason. A spec that only lists what is in is a spec that will grow forever.

## The specification, in short

Next.js App Router with one codebase for storefront and admin; Postgres with Prisma; integer piastres;
custom session auth for a single owner account; Arabic and English as peers, with RTL as a layout
direction rather than a stylesheet patch.

![Product detail](./screenshots/nava-product.png)

## Screenshots

| | |
|---|---|
| ![Checkout](./screenshots/nava-checkout.png) | ![Admin](./screenshots/nava-admin.png) |
| Guest checkout, cash on delivery, Arabic | Admin overview — the mockups carry demo data |

## Why it is here

Because a design system and a written specification are real work, and because a portfolio that only
shows finished things hides how the thinking happens. The build is the next phase, not a claim I made
early.
