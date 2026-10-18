# User Manual

How to update and check stock through Telegram.

## Contents

1. [Quick start](#1-quick-start)
2. [Typed changes](#2-typed-changes-one-product-per-message)
3. [Questions](#3-asking-questions-nothing-is-changed)
4. [Uploading a file](#4-uploading-a-file) (including [Request IDs](#43-request-ids-preventing-double-counting))
5. [Dry run](#5-dry-run-check-before-you-change)
6. [Reading the replies](#6-reading-the-replies)
7. [Low-stock alerts](#7-low-stock-alerts)
8. [Using it effectively](#8-using-it-effectively)
9. [Quick reference](#9-quick-reference)

## 1. Quick start

1. Open the bot in Telegram and send `/myid`. Give the number to the administrator, who must add it before you can change anything.
2. Send `help` to see the commands.
3. Try a question: `list`.
4. Try a change as a dry run: `dry run add 5 bread`.
5. Make the real change: `add 5 bread`.

## 2. Typed changes (one product per message)

| Goal | Type | Result |
|---|---|---|
| Increase stock | `add 20 bread` | Adds 20 to current stock |
| Decrease stock | `remove 5 milk` | Subtracts 5 |
| Set an exact count | `set 30 bread` | Replaces the stock with 30 |
| Register a new product | `create Bottled Water 100 bottle 20` | Name, starting quantity, unit, reorder level |

Rules:

- Names must match the inventory exactly, ignoring capital letters. A wrong name is rejected and close matches are suggested.
- Use the Product ID instead of the name when names are similar: `add 20 P001`.
- `add` and `remove` need a quantity above zero. `set` can be zero but never negative.
- You cannot remove more than is in stock.
- For `create` the order is always: name, quantity, unit, reorder level.
- One change per message.

The bot replies to each message, for example `OK: Stock updated and verified: Bread went from 20 to 40 loaf.`

## 3. Asking questions (nothing is changed)

| Type | You get |
|---|---|
| `stock bread`, `how much bread do we have?`, `do we have milk?`, `check P002` | Quantity, unit, reorder level, whether it is low, last updated |
| `low stock`, `what is low` | Every product at or below its reorder level |
| `list`, `inventory`, `stock` | All products and quantities |
| `history` | The last 10 changes, newest first, including rejected ones |
| `history bread` | The last 10 changes for that product |

Questions use fixed patterns. Long lists are cut off with a note saying how many more there are.

## 4. Uploading a file

Use a file for a batch, such as a delivery note or a stock count.

### 4.1 Preparing the file

- Types: `.csv`, `.xlsx`, `.xls`. For Excel, only the first sheet is read.
- Maximum 500 rows.
- The first row is the headings. One product operation per row.

| Column | Meaning |
|---|---|
| Action | `create`, `add`, `remove` or `set` |
| Product ID | Optional if the Item name is given. Leave blank for `create` |
| Item name | The exact product name |
| Quantity | A number |
| Unit | Required for `create`. Otherwise optional, but if given it must match the product's unit |
| Reorder level | Required for `create` only |
| Request ID | Optional, strongly recommended (see 4.3) |

Headings are matched loosely (`Item name`, `item_name`, `Item Name`). Blank lines are ignored. Rows run in order, so a `create` followed by an `add` for the same product works. See the [`samples/`](../samples) folder.

### 4.2 Sending the file

1. Tap the paperclip and choose **File**, not Photo or Gallery.
2. For a test, type `dry run` in the caption.
3. The bot acknowledges, then sends a summary listing every row as OK or REJECTED with the reason. The same summary is emailed to the manager.

### 4.3 Request IDs: preventing double counting

Every row has a Request ID. A Request ID that has already **succeeded** is refused, so the same file cannot be applied twice.

- If you leave it blank, one is generated from the file's contents. Sending the identical file again is skipped.
- **Warning:** if you edit a file and resend it, the contents change, so every row gets a new generated ID and old rows would be applied again.
- To avoid this, give each row a unique Request ID, such as a delivery note number plus a row number (`GRN-1001-1`). Rows that already succeeded are then skipped even in an edited file.
- Rejected rows can be fixed and resent with the same Request ID.

## 5. Dry run: check before you change

A dry run works out what each row would do against current stock, including the effect of earlier rows in the same file, and writes nothing to either sheet.

- File: put `dry run` in the caption.
- Typed change: start the message with `dry run`.

A dry run is a prediction. If stock changes between the dry run and the real run, results can differ.

## 6. Reading the replies

- **OK**: the change was made and confirmed by reading the row back.
- **REJECTED**: nothing was changed for that row. The reason is given and the attempt is logged.
- A file import can be partly successful. Good rows are applied even if others are rejected.

| Message | Meaning | Fix |
|---|---|---|
| Product not found | Name or ID not in the inventory | Check spelling with `list`, or use the Product ID |
| More than one inventory row matches | Ambiguous name | Use the Product ID |
| Only X available, cannot remove Y | Not enough stock | Check the count, or use `set` |
| Unit mismatch | Unit differs from the product's unit | Use the product's unit or leave it blank |
| This product already exists | `create` for an existing name | Use `add` or `set` |
| Quantity is missing or not a number | Bad quantity | Fix the number |
| Unknown action | Not create, add, remove or set | Correct the action |
| A valid reorder level is required | `create` without a reorder level | Add one |
| Duplicate Request ID | Already processed, or repeated in the file | Expected on resends. Use a new ID for a genuinely new change |

## 7. Low-stock alerts

- When a change leaves stock at or below the reorder level, an alert email is sent and the product is marked as alerted. No further email for that product until it is restocked above the reorder level.
- **File imports:** per-row alerts are muted by default. Low products are listed at the end of the summary instead.
- **Typed changes:** the normal alert email is sent.
- `low stock` shows the current list at any time.

## 8. Using it effectively

**Daily routine:** start with `low stock`; record deliveries as they arrive; record usage with `remove`; finish with `history`.

**Stock counts:** use `set` with the counted quantity. The change log keeps the previous quantity, so every correction is traceable. Do a physical count regularly: the system prevents many mistakes but cannot know that a typed number was wrong.

**Good habits:** dry run files first; use Product IDs when names are similar; keep units consistent (do not mix kg and g for one product); give every row a unique Request ID; fix rejected rows the same day.

**Avoid:** sending a photo of a list; editing and resending a file without Request IDs; using `set` when you mean `add`.

## 9. Quick reference

| Task | Message |
|---|---|
| Add stock | `add 20 bread` |
| Remove stock | `remove 5 milk` |
| Set a count | `set 30 bread` |
| New product | `create Bottled Water 100 bottle 20` |
| Test any change | `dry run` before the command |
| Check one product | `stock bread` |
| What is low | `low stock` |
| Everything | `list` |
| Recent changes | `history` or `history bread` |
| Bulk update | Send a CSV or Excel file (caption `dry run` to test) |
| Chat ID | `/myid` |
| Commands | `help` |
