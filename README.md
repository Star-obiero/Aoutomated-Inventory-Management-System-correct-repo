# Sample files

Use these to test the system. They use made-up product names. Send them to the bot as a **File**, with the caption `dry run` first.

| File | Purpose | Expected result |
|---|---|---|
| `01_clean_import.csv` | Creates Rice, Sugar, Cooking Oil, Salt, then adds, removes and sets stock | All 8 rows OK. Cooking Oil ends at 8 litre, which is low stock. A second send is skipped as duplicates |
| `02_import_with_errors.csv` | Deliberate mistakes. Send after file 01 has been applied | Unknown product, over-removal, negative quantity, unit mismatch, unknown action, existing product, missing reorder level and a duplicate Request ID are rejected. The valid rows pass |
| `03_excel_import.xlsx` | Same shape as file 01 with different product names, to prove Excel works | All 8 rows OK |

Delete the test products from your Inventory sheet before using real stock.
