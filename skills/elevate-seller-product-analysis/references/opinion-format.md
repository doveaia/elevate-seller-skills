# Opinion format

What `submit_product_opinion` accepts, and what it answers.

## Fields

| Field | Type | Required | Limit |
|---|---|---|---|
| `identifier` | string | yes | The ASIN or the EAN/UPC barcode of a product the application already knows. |
| `decision` | string | yes | `BUY`, `INVESTIGATE` or `REJECT`. |
| `rationale` | string | yes | From one to 4000 characters. |
| `criteria` | list | yes | Exactly twelve items, numbered from one to twelve, each number once. |
| `id` | number | yes, in each item | The criterion number, from one to twelve. |
| `status` | string | yes, in each item | `OK`, `TOLERANCE`, `FAIL` or `UNKNOWN`. |
| `observed` | string | no | Up to 300 characters. |
| `threshold` | string | no | Up to 300 characters. |
| `calculation` | string | no | Up to 2000 characters. |
| `comment` | string | no | Up to 2000 characters. |
| `grid_version` | string | no | The `grid_version` returned by `get_analysis_grid`. Up to 32 characters. |
| `purchase_cost` | number | no | The purchase cost per unit your analysis used, excluding tax. Greater than zero. |
| `purchase_cost_currency` | string | no | `EUR`. |
| `agent_name` | string | no | The name you give yourself. Up to 80 characters. |
| `language` | string | no | `fr` or `en`. The language of the organization by default. |
| `submission_ref` | string | no | From one to 64 characters: letters, digits, underscore and hyphen. |

Rules that apply to the whole opinion:

- No text may contain a null character.
- The whole opinion has a maximum size. `invalid_opinion` says when it is too large.
- The server lists every problem at once in `details`: fix them all before you send the opinion again.
- The server does not check that your statuses follow the thresholds, nor that your decision follows the decision rule. That is your job.

## Receipt

The receipt has a `status`: `recorded` when the opinion was written, `already_recorded` when your `submission_ref` names an opinion recorded before (nothing is written again).

`opinion_ref` identifies the opinion. `replaced_previous` says whether another opinion was displayed for this product before yours. `kept` is the number of opinions the application keeps for this product, yours included. `grid_version_current` says whether the `grid_version` you sent is the current one: when it is false, read the grid again and analyse the product again.

## Example

Example of FORM only: every value in angle brackets is a placeholder, never a figure or a threshold to apply.

```json
{
  "identifier": "<the ASIN or barcode of the product>",
  "decision": "INVESTIGATE",
  "rationale": "<why the decision rule of the grid gives this decision, criterion by criterion>",
  "criteria": [
    {
      "id": 1,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 2,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 3,
      "status": "TOLERANCE",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 4,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 5,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 6,
      "status": "UNKNOWN",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<why this criterion is unknown: the stale source or the missing datum, as the server returned it>"
    },
    {
      "id": 7,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 8,
      "status": "FAIL",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 9,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 10,
      "status": "TOLERANCE",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 11,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    },
    {
      "id": 12,
      "status": "OK",
      "observed": "<observed value, with its unit, from get_product_analysis_data>",
      "threshold": "<the threshold exactly as the grid states it>",
      "calculation": "<your calculation, from the observed value to the status>",
      "comment": "<your comment on this criterion>"
    }
  ],
  "grid_version": "<the grid_version returned by get_analysis_grid>",
  "agent_name": "<the name you give yourself>",
  "language": "<the language of the grid>",
  "submission_ref": "<your own reference for this opinion>"
}
```
