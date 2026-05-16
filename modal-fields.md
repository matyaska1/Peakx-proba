# Ledger — Modal Field Specifications

All modals: 370px wide, slides in from the right edge of the screen.
Layout: title + CTA sentence on top, fields stacked vertically using the Input component.
Amounts always displayed in larger, prominent typography.
Dropdowns use a chevron-down arrow inside the input field.
Multi-choice type selectors use Chips (multi-select).

---

## 1. New entry — Log a transaction by hand

**Title:** New Entry
**CTA:** Record a transaction manually — it will appear in your ledger immediately.

| Field | Type | Notes |
|---|---|---|
| Date | Input (date picker) | Default: today |
| Description | Input (text) | Merchant or payee name |
| Type | Chips (multi) | Debit / Credit |
| Amount | Input (number, large) | Prominent size |
| Account | Input + chevron (dropdown) | Checking / Savings / Credit · Amex / Brokerage |
| Category | Chips (multi) | Groceries / Transport / Subscription / Dining / Shopping / Utilities / Other |
| Note | Input (text, optional) | e.g. "9th of 12" |

---

## 2. New transfer — Move money between accounts

**Title:** New Transfer
**CTA:** Move funds between your accounts — both sides of the entry are logged automatically.

| Field | Type | Notes |
|---|---|---|
| From account | Input + chevron (dropdown) | Checking / Savings / Credit · Amex / Brokerage |
| To account | Input + chevron (dropdown) | Same list, must differ from From |
| Amount | Input (number, large) | Prominent size |
| Date | Input (date picker) | Default: today |
| Note | Input (text, optional) | e.g. "Monthly sweep" |

---

## 3. New goal — Start a savings goal

**Title:** New Goal
**CTA:** Define a savings target and track your progress toward it over time.

| Field | Type | Notes |
|---|---|---|
| Goal name | Input (text) | e.g. "Japan in autumn" |
| Category / type | Chips (multi) | Reserve / Travel / Property / Other |
| Target amount | Input (number, large) | Prominent size |
| Target date | Input (date picker) | e.g. August 2026 |
| Linked account | Input + chevron (dropdown) | Savings / Checking / Brokerage |
| Monthly contribution | Input (number, optional) | Auto-calculated if left blank |

---

## 4. New category — Add a spending category

**Title:** New Category
**CTA:** Create a custom budget category to organise and track your spending.

| Field | Type | Notes |
|---|---|---|
| Category name | Input (text) | e.g. "Dining out" |
| Type | Chips (multi) | Expense / Income |
| Monthly budget | Input (number, large) | Prominent size |
| Color tag | Chips (multi) | Blue / Gold / Red / Olive / Ink |

---

## 5. Connect account — Link a new bank

**Title:** Connect Account
**CTA:** Link a bank or institution so your balances and transactions sync automatically.

| Field | Type | Notes |
|---|---|---|
| Bank / institution | Input (text) | e.g. "ING", "N26", "Amex" |
| Account type | Chips (multi) | Checking / Savings / Credit / Brokerage |
| Account nickname | Input (text) | e.g. "Checking" |
| Last 4 digits | Input (number) | e.g. 2814 |
| Color tag | Chips (multi) | Blue / Gold / Red / Olive / Ink |
| Opening balance | Input (number, large) | Prominent size |
| Connection method | Chips (multi) | Open Banking / Manual CSV |
