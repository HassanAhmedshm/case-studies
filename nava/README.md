# NAVA — bilingual luxury-commerce system

**Stack:** Next.js · PostgreSQL · Supabase - compatible

## The problem

A two-person fashion brand in Egypt sold clothing through Instagram with no system behind it: no
catalogue, no orders, no stock control, and no way to change the homepage without a developer.

## What I built

**Storefront** — home, collections, product detail, cart, cash-on-delivery checkout, order
confirmation. Fully bilingual English and Arabic, including right-to-left layout.

**Admin suite** — dashboard KPIs, products and variants, orders and fulfilment, customers,
collections, discounts, and storefront content.

## Decisions worth reading

**Money is integer piastres, never a float.** Every price is stored as an integer number of piastres —
one hundredth of a pound. Floating-point currency bugs are not a risk to be managed; they are removed
by the type.

**Stock lives on the variant, never on the product.** A product is a garment. A variant is one size and
colour, with its own SKU and its own stock count. Putting stock on the product is the mistake that
forces a rewrite the first time a size sells out.

**Orders snapshot everything at placement.** The recipient, the address, the item names and the prices
are copied into the order. A price change next week must not silently rewrite what a customer was
charged last week.

**Order status is a state machine, not free text.** `PENDING → SHIPPED → DELIVERED`, and
`PENDING → CANCELLED`. Delivered and cancelled are terminal. Illegal transitions are rejected in the
database rather than in the admin UI.

**Guest checkout with an upserted customer.** No accounts for shoppers. The order's phone number
upserts a customer record, because requiring a signup to buy one dress is how you lose the sale.

**Collections replace categories.** Merchandising groups like "The Silk Edit" are how the brand
actually thinks, so the data model says collection, not category.
