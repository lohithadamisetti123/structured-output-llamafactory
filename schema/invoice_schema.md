# Invoice JSON Schema

## Overview
Each invoice must be represented as a single JSON object with the following keys.
All keys are required and must always be present, even when the source field is absent (in which case `null` is used where specified).

## Top-level keys

- `vendor` (string)  
  The legal or trading name of the vendor issuing the invoice; use plain text with no surrounding whitespace; if vendor is not present or illegible, use the empty string "".

- `invoice_number` (string)  
  The invoice identifier exactly as shown on the document; preserve alphanumeric characters and dashes; if missing, use "".

- `date` (string, format `YYYY-MM-DD`)  
  The invoice issue date; convert any format (e.g., `03/15/2024`, `15-03-2024`) into `YYYY-MM-DD`; if the date cannot be determined, use "".

- `due_date` (string or null, format `YYYY-MM-DD`)  
  The payment due date; convert to `YYYY-MM-DD`; if no due date is provided anywhere in the document, set this field to `null`.

- `currency` (string, 3-letter ISO code)  
  Three-letter currency code such as "USD", "EUR", "INR", "GBP", "JPY"; if only a symbol is present (e.g. `$`), map to the most likely ISO code; if currency is truly unknown, use "UNK".

- `subtotal` (float)  
  The pre-tax subtotal amount; strip commas and symbols, parse as a floating-point number; if the invoice has only a total and no explicit subtotal, set this equal to the total.

- `tax` (float or null)  
  The tax amount; parse as float; if no tax is mentioned (e.g., tax-inclusive pricing with no separate line), set to `null`.

- `total` (float)  
  The final total amount due; must always be present and parsed as float; strip commas and symbols.

- `line_items` (array of objects)  
  An array of line item objects, each describing one billed item; must be an array (empty array `[]` is allowed if the document has no itemized lines).

## Line item object

Each object in `line_items` must contain:

- `description` (string)  
  Human-readable description of the billed item or service; if no explicit description is present, use a short synthetic description derived from context (e.g. "Miscellaneous charge").

- `quantity` (float)  
  Quantity of the item; parse numeric content as float to allow partial quantities (e.g. `1`, `2.0`, `0.5`); if quantity is missing, default to `1.0`.

- `unit_price` (float)  
  Price per unit of the item; parse as float; if only total per line is present and quantity is available, compute `unit_price = line_total / quantity`; if neither explicit unit price nor computable value is available, set to `0.0`.

## Handling absent or ambiguous fields

- Dates that cannot be confidently normalised are set to "" (empty string) for `date` and `null` for `due_date`.
- Missing vendor or invoice_number fields use "" rather than `null` to keep them string-typed.
- Missing tax uses `null` to distinguish from a real zero tax line.
- Missing currency uses "UNK".
- Missing line items result in `line_items: []` (empty array), not `null`.

All training and evaluation JSON objects for invoices **must** follow this schema exactly.
