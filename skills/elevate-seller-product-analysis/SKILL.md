---
name: elevate-seller-product-analysis
description: Analyse an Amazon product for a seller with the Elevate Seller MCP tools - read the seller's twelve-criterion grid, read the product's data, judge each criterion, decide, and record the opinion in the application. Use when asked to analyse, judge or screen products by ASIN or EAN for sourcing.
---

# Elevate Seller product analysis

This skill lets you analyse a product the way Elevate Seller does, with the seller's own grid and data, and record your opinion where the seller reads it.

## When to use

- The seller asks you to analyse, judge, screen or compare products for sourcing.
- The seller gives products by ASIN or by EAN/UPC barcode.
- The seller asks what you already concluded on a product, or what they answered to your opinions.

## When not to use

- To change a price, a listing or a setting: the server cannot do it.
- To replace the deep analysis of the application: your opinion sits next to it, it does not overwrite it.

## Tools

- `get_analysis_grid`: the twelve criteria, their thresholds, the decision rule and the seller's business rules. Free.
- `get_product_analysis_data`: everything the application knows about one product for the twelve criteria. Uses one analysis from the monthly quota when it returns data or a definitive not found. A `pending` answer is free.
- `submit_product_opinion`: records your opinion in the application. Free. Needs a read-write API key.
- `get_product_opinions`: the opinions recorded for one product and the seller's answers to them. Free.
- `list_product_opinions`: the displayed opinions of the organization, page by page. Free.
- `check_product_eligibility`: whether the seller's Amazon account may sell the product. Free. Use it only when the eligibility is missing from the product's data.

Every tool call also counts against the rate limit of the API key.

## Procedure

1. Read the grid once per session with `get_analysis_grid`. Keep `grid_version`, `language`, `business_rules`, `freshness`, `criteria` and `decision_rule` for the whole session.
2. Read the product with `get_product_analysis_data`.
   - Pass the ASIN or the barcode as `identifier`.
   - When the seller gives a purchase cost, pass it as `purchase_cost`.
   - On `pending`: wait `retry_after_seconds`, then ask again. Never ask faster. After a few waits, tell the seller the product is still being prepared and move to the next product.
   - On `not_found`: tell the seller the application does not follow this product.
   - Pass `freshness` as `any` only when the seller accepts old data. Each source then says whether it is `stale`.
3. Judge each of the twelve criteria.
   - Use the thresholds of the grid: the `ok`, `tolerance`, `fail` and `unknown` texts and the `thresholds` of each criterion. Never use a threshold from memory.
   - Use the figures the application computed in `metrics` rather than computing your own.
   - Read the state of each criterion in the `criteria` list of the product's data: its `source`, `stale` and `unknown_reason`.
   - A criterion whose source is `stale` is `UNKNOWN`.
   - Otherwise the criterion follows its own `unknown` rule from the grid.
   - An `unknown_reason` says which datum is missing. It does not say that the criterion is unknown. Criterion two can be judged on the sales velocity (`velocity`, `sales_basis`) without measured sales. Criterion three can be judged on the rank (`sales_rank`) without the percentage.
   - A `null` price in the series of Amazon's own prices means that Amazon had no offer that day, not that the price is unknown.
   - When the eligibility is missing from `account`, call `check_product_eligibility`.
   - Never assume a value. A missing datum makes the criterion `UNKNOWN` unless the grid says otherwise.
4. Apply the `decision_rule` of the grid to the twelve statuses: `BUY`, `INVESTIGATE` or `REJECT`.
5. Write the opinion.
   - For each criterion: the observed value with its unit, the threshold exactly as the grid states it, your calculation, and a comment.
   - Then the reasons for your decision.
   - Write in the `language` of the grid, and pass it as `language`.
6. Record it with `submit_product_opinion`.
   - Pass the `grid_version` you read in step one.
   - Pass a `submission_ref` of your own for this opinion. Reuse the same one each time you retry the same opinion: the server then records it once. `already_recorded` means it is already there.
   - On `invalid_opinion`: fix every item of `details`, then send the whole opinion again. `criteria[N]` in a `field` is the criterion number N, not its place in the list.
   - On `write_key_required`: tell the seller, with the `action` of the refusal. Do not retry.
   - When `submit_product_opinion` is not in the list of tools, the server does not accept opinions yet. Tell the seller, give your analysis in the conversation, and do not retry.
7. Read the seller's answers with `get_product_opinions` and use them in your next analyses.
   - Each criterion may carry a `feedback`, and the decision a `decision_feedback`.
   - `relevant` false with a `comment` says what the seller disagrees with.
   - `list_product_opinions` shows which products are already analysed.
   - `rules_changed` true means the grid or the business rules changed since that opinion: analyse the product again.

## Refusals

A refusal carries a `code`, a `message` and an `action`. Always relay the `message` and the `action` to the seller.

| Code | What to do |
|---|---|
| `quota_exceeded` | Stop reading product data. Tell the seller when the quota renews (`renews_at`). The free tools still work. |
| `rate_limited` | Wait `retry_after_seconds`, then carry on. |
| `queue_full` | Wait `retry_after_seconds` before asking for new products. |
| `subscription_required` | Stop. Tell the seller to subscribe. Do not retry. |
| `timeout` | Wait `retry_after_seconds` and retry once. |
| `invalid_identifier` | Correct the ASIN or barcode, then call again. |
| `invalid_argument` | Correct the parameter the `message` names, then call again. |
| `product_unknown` | Call `get_product_analysis_data` for this product first, then submit again. |
| `unsupported_marketplace` | Do not retry for this product. |
| `invalid_opinion` | Fix every item of `details` and send the whole opinion again. |
| `write_key_required` | Tell the seller a read-write key is needed. Do not retry. |
| `internal_error` | Retry later. If it keeps happening, tell the seller. |

## Rules

- Invent nothing. Every value in an opinion comes from what the server returned in this session.
- Never copy a threshold or a business rule from memory. Always use the ones of the grid read in this session.
- Tell the seller what stays unknown, and why.
- Pace yourself on the quota (`analyses_remaining`) and on the rate limit of the key.
- A read-only key cannot record an opinion.
- The seller reads your opinion as you wrote it. Write plainly, in the seller's language.

The fields of an opinion, their limits and a complete example are in `references/opinion-format.md`.
