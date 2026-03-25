# Purchase Order JSON Schema

## Overview
Each purchase order must be represented as a single JSON object with the following keys.
All keys are required and must always be present, even when the source field is absent.

## Top-level keys

- `buyer` (string)  
  Name of the buying organisation or person issuing the PO; if not explicitly present, use "".

- `supplier` (string)  
  Name of the supplier / vendor receiving the PO; if missing or illegible, use "".

- `po_number` (string)  
  Purchase order identifier exactly as shown; preserve alphanumeric characters and dashes; if missing, use "".

- `date` (string, format `YYYY-MM-DD`)  
  PO creation or issue date; normalise to `YYYY-MM-DD`; if unknown, use "".

- `delivery_date` (string or null, format `YYYY-MM-DD`)  
  Expected delivery or shipping date; normalise to `YYYY-MM-DD`; if no such date appears, set to `null`.

- `currency` (string)  
  Three-letter ISO currency code like "USD", "EUR", "INR", "GBP", "JPY"; map symbols when possible; if unknown, use "UNK".

- `total` (float)  
  Total PO monetary amount; parse as float; strip commas and symbols; if the document has line items but no explicit total, compute as sum of `quantity * unit_price` over items.

- `items` (array of objects)  
  Array of ordered items; may be empty (`[]`) if there is genuinely no itemisation.

## Item object

Each object inside `items` must contain:

- `item_name` (string)  
  Name or description of the ordered item; if not present, use a short synthetic description (e.g. "Miscellaneous item").

- `quantity` (float)  
  Ordered quantity; parse as float; if missing, default to `1.0`.

- `unit_price` (float)  
  Price per unit; parse as float; if only line total is present and quantity is known, compute `unit_price = line_total / quantity`; if not determinable, use `0.0`.

## Handling absent or ambiguous fields

- All string fields default to "" when absent.
- Date fields: `date` uses "" when unknown; `delivery_date` uses `null` when absent.
- Currency defaults to "UNK" when unidentifiable.
- `items` must always be an array; if there are no clear line items, use `[]`.

All training and evaluation JSON objects for purchase orders **must** follow this schema exactly.
