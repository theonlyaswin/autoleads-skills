# Invoice Template

## Markdown

```markdown
# Invoice INV-2026-001

**From**
<Business name>
<Address>
<Email> · Tax ID: <id, if any>

**Bill to**
<Client name / company>
<Address>
<Email>

| Issued | Due | Terms |
|---|---|---|
| 2026-10-11 | 2026-10-18 | Due in 7 days |

| Description | Qty | Unit price | Amount |
|---|---:|---:|---:|
| Milestone 1: Design & wireframes (50% deposit) | 1 | 1,200.00 | 1,200.00 |
| CO-001: Gift-wrap option at checkout | 1 | 450.00 | 450.00 |

| | |
|---|---:|
| Subtotal | 1,650.00 |
| Tax (0%) | 0.00 |
| **Total due (USD)** | **1,650.00** |

**How to pay**
<Payment methods exactly as the user provided them>

Please include INV-2026-001 as the payment reference. Thank you!
```

Omit the Tax row if the user doesn't charge tax. Omit Tax ID if none.

## HTML

Self-contained, no external fonts, scripts or images. Prints cleanly on A4 and US Letter.

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Invoice INV-2026-001</title>
<style>
  @page { size: auto; margin: 18mm; }
  * { box-sizing: border-box; }
  body { font: 14px/1.5 -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; color: #111; max-width: 780px; margin: 32px auto; padding: 0 16px; }
  header { display: flex; justify-content: space-between; align-items: flex-start; gap: 24px; margin-bottom: 32px; }
  h1 { font-size: 28px; margin: 0; letter-spacing: -0.02em; }
  .muted { color: #666; }
  .parties { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; margin-bottom: 24px; }
  .label { font-size: 11px; text-transform: uppercase; letter-spacing: 0.08em; color: #666; margin-bottom: 4px; }
  table { width: 100%; border-collapse: collapse; margin: 16px 0; }
  th, td { padding: 10px 8px; border-bottom: 1px solid #e5e5e5; text-align: left; vertical-align: top; }
  th { font-size: 11px; text-transform: uppercase; letter-spacing: 0.06em; color: #666; font-weight: 600; }
  .num { text-align: right; font-variant-numeric: tabular-nums; white-space: nowrap; }
  .totals { width: 320px; margin-left: auto; }
  .totals td { border: 0; padding: 4px 8px; }
  .totals .grand td { border-top: 2px solid #111; font-weight: 700; font-size: 16px; padding-top: 10px; }
  .pay { margin-top: 32px; padding: 16px; background: #f6f6f6; border-radius: 8px; }
  @media print { body { margin: 0; } .pay { background: none; border: 1px solid #ddd; } }
</style>
</head>
<body>
  <header>
    <div><h1>Invoice</h1><div class="muted">INV-2026-001</div></div>
    <div class="num"><div><span class="muted">Issued</span> 2026-10-11</div><div><span class="muted">Due</span> 2026-10-18</div></div>
  </header>
  <section class="parties">
    <div><div class="label">From</div><!-- business name, address, email, tax id --></div>
    <div><div class="label">Bill to</div><!-- client name, address, email --></div>
  </section>
  <table>
    <thead><tr><th>Description</th><th class="num">Qty</th><th class="num">Unit price</th><th class="num">Amount</th></tr></thead>
    <tbody><!-- one <tr> per line item --></tbody>
  </table>
  <table class="totals">
    <tr><td>Subtotal</td><td class="num">1,650.00</td></tr>
    <tr class="grand"><td>Total due (USD)</td><td class="num">1,650.00</td></tr>
  </table>
  <section class="pay"><div class="label">How to pay</div><!-- payment methods --><p class="muted">Please use INV-2026-001 as the payment reference.</p></section>
</body>
</html>
```

Escape any `<`, `>` and `&` in user-provided text.
