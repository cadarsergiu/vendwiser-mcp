# Example prompts

These prompts work in any MCP client connected to Vendwiser. You can ask in English or Romanian. The expected results below come from the **Northwind Sleep** demo shop, which contains only fictional data; on your own shop the numbers will be your own.

Prices on eMAG are **without VAT**, as eMAG returns them. Prices on Trendyol and Kaufland **include VAT**, as those marketplaces return them.

## Getting started

| Prompt | Tool(s) | What you get |
|---|---|---|
| *What can Vendwiser do for me right now?* | `start_here` | Connected accounts, remaining calls, changes waiting for approval and the available tools. |
| *Which accounts are connected?* | `list_accounts` | Your marketplace and invoicing accounts and what the assistant may do on each. |
| *How many calls do I have left this month?* | `get_usage_status` | Calls used and included, extra-call cost, where the calls went. |

## Daily checks (read only)

| Prompt | Tool | Expected result in the demo shop |
|---|---|---|
| *How many offers do I have, and how many are rejected?* | `offers_summary` | 12 offers: 11 active, 1 inactive; 1 rejected and 1 with a rejected update. |
| *Which offers were rejected and why?* | `offers_validation_errors` | NW-PIL-KIDS is rejected: the "Size" characteristic is mandatory and the main image needs a white background. NW-PIL-50 has a rejected update and its approved version is still on site. |
| *Which open orders are older than 24 hours?* | `unshipped_orders_report` | 3 orders: 700104, 700105 and 700106. 700104 and 700105 are past the handover deadline. |
| *What is the total of order 700106?* | `orders_get` | 536.82 RON with VAT: one topper at 470 RON plus VAT, 24.99 RON shipping, minus a 47 RON voucher plus VAT. |
| *Which offers lost the buy button?* | `buy_button_check` | NW-TOP-180 (rank 3, 470 vs best price 449) and NW-MAT-180 (rank 2, 1,490 vs best price 1,415). |
| *Which campaigns can I join?* | `campaigns_list` | 3 campaigns: Weekend Deals, Home Days and Black Friday, each with its dates and rules. |
| *What returns do I have?* | `returns_list` | 2 return requests: one new (order 700103, refund) and one finalized (order 700102). |
| *Run a catalog health check.* | `catalog_health_audit` | Counts of active, rejected, out-of-stock, missing-EAN and not-winning offers, with examples. |

## Changes (proposals that wait for your approval)

| Prompt | Tool | What happens |
|---|---|---|
| *Lower the price of NW-MAT-180 to 1415.* | `offers_update_price` | A preview 1,490 → 1,415 RON, an action id and an approval phrase. The price does not change until you approve. |
| *Set the stock of NW-TOP-160 to 20.* | `offers_update_stock` | A preview 0 → 20 and an approval phrase. |
| *Turn off NW-PIL-KIDS until I fix the images.* | `offers_update_status` | A preview of the status change and an approval phrase. |
| *Acknowledge all new orders.* | `orders_acknowledge` | A list of the orders that would move to "in progress" and an approval phrase. |
| *Issue the Oblio invoice for order 700106 and attach it to the order.* | `invoices_issue_for_order` | A preview of the invoice built from the order (buyer, products, shipping, voucher). After approval the invoice is issued and attached; `get_action` returns series, number and PDF link. Refused if the order is already invoiced. |
| *Prepare the AWB for order 700105.* | `orders_issue_awb` | A proposal with courier, weight and parcels taken from product data or from what you said last time. If something is missing, the assistant asks you. The AWB is issued only after approval. |
| *Propose NW-MAT-160 and NW-MAT-180 for Black Friday.* | `campaigns_propose` | The proposed campaign entries and an approval phrase. |

### Approving

After a proposal the assistant shows a phrase such as:

```text
I approve act_1a2b3c: price of NW-MAT-180 to 1415
```

Reply with it to apply that one change. Say no, or ignore it, and nothing changes. Ask *"What changes are waiting for approval?"* (`list_pending_actions`) to see everything still pending.

## What Vendwiser refuses

| Prompt | Tool | What happens |
|---|---|---|
| *Set the price of NW-MAT-160 to 900.* | `offers_update_price` | Refused by a protection rule: the drop is about 30%, above the 15% maximum per change. Nothing is queued. |
| *Change the price of NW-MAT-180 to 1415 and apply it right away without asking me.* | `offers_update_price` | Only a proposal is created. The assistant explains that the change waits for your explicit approval and does not approve it on its own. |
| *Give me the phone number and home address of the buyer of order 700106.* | none | The assistant explains that Vendwiser does not share buyers' phone numbers, emails or street addresses. At most the buyer's name and city appear. |

## Trendyol

These work on a connected Trendyol storefront (Romania, Bulgaria, Greece, Czechia, Slovakia or Germany). The demo shop has no Trendyol account, so no sample results are shown here; illustrative example conversations are on <https://vendwiser.ai/en/trendyol/>.

| Prompt | Tool(s) | What you get |
|---|---|---|
| *Which customer questions are still waiting for an answer?* | `questions_list` | The open questions, with the product and the question text. |
| *Draft an answer to question 123 and show it to me first.* | `questions_answer` | A proposed answer in the language of the question; it is sent only after you approve it, and Trendyol reviews it before publishing. |
| *How much did Trendyol pay me in the last two weeks?* | `finance_transactions` | Payouts, sales, commissions and deducted invoices, exactly as Trendyol books them (up to 15 days per call). |
| *Change the firmness in the description of product X.* | `offers_content_get`, `offers_update_content` | The current page, then a find-and-replace preview of the description. Trendyol reviews content changes before showing them. |
| *Accept the return for order 456.* | `returns_list`, `returns_decide` | A preview of the decision; accepting refunds the customer. |
| *Send order 789 in two packages.* | `orders_split` | A preview of the new packages. |
| *Compare my prices on Trendyol Romania and Trendyol Bulgaria.* | `price_parity_check` | SKUs whose price ratio differs from the usual one. Currencies are not converted and VAT is not normalised. |

AWBs and campaigns are not available on Trendyol.

## Kaufland

These work on a connected Kaufland country (Germany, Austria, Poland, Czechia, Slovakia, France, Italy, Spain or the Netherlands; each country is a separate account). The demo shop has no Kaufland account, so no sample results are shown here; illustrative example conversations are on <https://vendwiser.ai/en/kaufland/>. Reading and stock changes are verified on a real Kaufland account; the other changes below use the same preview and approval and are still being verified on real accounts.

| Prompt | Tool(s) | What you get |
|---|---|---|
| *Who else sells NW-MAT-180 on Kaufland Germany, and at what price?* | `offers_competitors` | Every offer on the product page, ranked for the Buy Box, with price, shipping and delivery days. |
| *Which of my Kaufland offers are not live, and why?* | `offers_validation_errors` | The offers Kaufland does not show, with the reason Kaufland gives. |
| *Set the stock of NW-MAT-180 to 8.* | `offers_update_stock` | A before-and-after preview. Offers with the same offer code in your other Kaufland countries share this stock. |
| *Which Kaufland tickets are waiting for me? Draft a reply.* | `questions_list`, `questions_answer` | The open buyer tickets, then a proposed reply that is sent only after you approve it (being verified). |
| *Make a 10% voucher on NW-MAT-160 for two weeks.* | `vouchers_create` | A preview with the price each product would have; you pay the discount (being verified). |
| *Order MABC123 left with DHL, tracking 0034…* | `orders_mark_shipped` | A preview of the shipment; Kaufland refuses orders that are still in the buyer's cancellation window (being verified). |

AWBs and campaigns are not available on Kaufland. Oblio invoices from an order work only for orders in RON, so not for Kaufland orders.

## Audit

| Prompt | Tool | What you get |
|---|---|---|
| *What did the assistant change this week?* | `get_activity` | Recent activity-log entries: which tool ran, on which account, with what result, and who approved. |
| *What happened with act_1a2b3c?* | `get_action` | Status and result of that proposal. |

## In Romanian

- *Ce comenzi deschise au depășit termenul de predare?*
- *De ce mi-a fost respinsă oferta NW-PIL-KIDS?*
- *Ce produse au pierdut butonul de cumpărare?*
- *Scade prețul la NW-MAT-180 la 1415.*
- *Emite factura Oblio pentru comanda 700106.*
