# Curation Log

This log documents each reviewed candidate document and the decision to keep or reject it.

| example_id | document_type  | source                             | kept_or_rejected | reason                                      | schema_issues_found                    |
|-----------:|----------------|-------------------------------------|------------------|---------------------------------------------|----------------------------------------|
| 1          | invoice        | naver-clova-ix/cord-v2:train:0      | kept             | Clear layout, standard invoice fields       | None                                   |
| 2          | invoice        | naver-clova-ix/cord-v2:train:1      | rejected         | Ambiguous vendor name                       | Vendor string not confidently extractable |
| 3          | purchase_order | katanaml-org/invoices-donut-data-v1:train:0 | kept             | Good multi-item PO with clear totals        | None                                   |

<!--
Add rows until you have at least 80 kept examples:
- 50 invoices
- 30 purchase orders
Each candidate (kept or rejected) should be logged with a short justification.
-->
