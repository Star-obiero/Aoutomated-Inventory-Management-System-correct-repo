# Manager Brief

A plain-language introduction: what the Inventory Bot is, whether it is relevant to your business, and how to try it at low risk.

## 1. The problem it solves

Most small and mid-sized businesses track stock in a spreadsheet or in someone's head. That works until it does not:

- You find out something has run out when a customer asks for it.
- Records fall behind because updating the sheet waits until the end of the day or week.
- Two people edit the same file and one overwrites the other.
- Numbers do not match at month end, and nobody can say who changed what.
- Money is tied up in stock ordered twice because nobody was sure what was on the shelf.

## 2. What it is

A chat in Telegram. You send a short message such as `add 20 bread` when stock arrives or `remove 5 milk` when it is used. For bigger jobs, such as a delivery with 30 items, you send one spreadsheet file. You can ask questions at any time, such as `stock bread` or `low stock`.

Behind the chat, the system checks every change, saves it to your inventory sheet and keeps a record of it. You do not log into any system or learn new software.

## 3. What you get

| What matters to you | How it helps |
|---|---|
| Knowing what you have now | Ask `stock bread` or `list` from your phone |
| Not running out | `low stock` shows everything at or below its reorder level, and you get an email when a product gets low |
| Fewer mistakes | Every change is checked first and bad ones are rejected with a reason |
| Knowing who changed what | Every attempt is written to a change log |
| Saving time | One message for one change, one file for a whole delivery |
| Control | Only approved Telegram accounts can change stock |
| Confidence | A dry run previews any change without applying it |

## 4. A normal day

- **Morning:** send `low stock` and see what needs ordering.
- **Delivery arrives:** send `add 40 flour`, or one file for the whole delivery. You get a summary of what went in and what was rejected.
- **During the day:** `remove 3 milk` when stock is used or damaged.
- **A colleague asks "do we have sugar?":** send `stock sugar`.
- **End of day:** send `history` to see everything changed today.

## 5. What it does not do

- It does not count stock. It records what people tell it, so regular physical counts are still needed.
- It does not track prices, costs or sales values.
- It does not connect to a till or point-of-sale system.
- It does not place supplier orders.
- Typed changes are one product per message. Use a file for many.
- Questions use set phrasings, not free-form questions.
- There is no undo button. Corrections are recorded as new entries and the log keeps the trail.

## 6. Is it relevant to you?

Worth trying if you answer yes to two or more:

- Have you run out of something unexpectedly in recent months?
- Is stock tracked in a spreadsheet or notebook that is often out of date?
- Do two or more people receive or use stock?
- Do you spend noticeable time each week updating or reconciling stock?
- Have you been unable to explain a gap between the records and the shelf?

Probably less useful if you hold only a handful of products you can check at a glance every day.

## 7. A low-risk two-week trial

**Day 1.** Send `/myid` to the bot and give the number to your administrator. They load your current stock from a file, starting with a dry run. Send `list` and check it.

**Week 1.** Record deliveries and usage through the bot while keeping your old method running. At the end of the week, count a few key products and compare with `stock <product>`. Fix differences with `set`.

**Week 2.** Start the day with `low stock` and end it with `history`. Note any rejected entries and their causes.

**Decide.** Good signs: fewer surprise stock-outs, counts that match the records, less time on stock admin. If they are missing, little has been lost.

## 8. Control and safety

- Only approved chat accounts can change or query stock.
- Nothing changes without being checked, and the system refuses to remove more than is in stock.
- The same file cannot be applied twice by accident.
- Every attempt, successful or rejected, is recorded with a time and a source.

## 9. Common questions

**Do I need technical skills?** No. If you can send a Telegram message, you can use it.

**Can staff use it?** Yes, once the administrator adds their accounts.

**What if I enter something wrong?** Record a correction with `set`, or the opposite `add` or `remove`. The log shows both entries.

**What if I mistype a product name?** It is rejected, nothing changes, and the reply suggests close matches.

**What if I send a file twice?** An identical file is skipped the second time. If you edit and resend a file, give every row its own Request ID so nothing is counted twice (see the [User Manual](USER_MANUAL.md#43-request-ids-preventing-double-counting)).

**Will I get a flood of emails?** For file imports, low-stock products are listed once in the summary email instead of one email per row.
