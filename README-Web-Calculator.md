# Zakat & Fitra Web Calculator

A fully interactive, multi-language Islamic Zakat and Fitra calculator built as a single self-contained HTML file — no frameworks, no backend, no installation required.

---

## Features

- **Zakat Calculator** — calculates 2.5% Zakat on net zakatable wealth
- **Fitra Calculator** — calculates Zakat al-Fitr per household member
- **5 Languages** — English, Hindi, Urdu, Arabic, Bengali (with full RTL support for Urdu & Arabic)
- **8 Currencies** — INR, PKR, USD, GBP, EUR, SAR, AED, CAD, AUD
- **Gold & Silver by Weight** — enter grams; value auto-calculated using live price inputs
- **Nisab Toggle** — choose Gold (87.48g) or Silver (612.36g) basis
- **Export Report** — save full report as PNG image or PDF (single page)
- **Responsive** — works on desktop, tablet, and mobile browsers

---

## Default Prices (INR)

| Metal  | Price per gram |
|--------|---------------|
| Gold   | ₹ 16,331      |
| Silver | ₹ 290         |

> Update these values in the Settings section to match current market rates.

---

## File Structure

```
ttol/
├── zakat-fitra-calculator.html   ← Main calculator (single file, fully self-contained)
├── manifest.json                  ← PWA manifest (for installable web app)
├── sw.js                          ← Service worker (offline PWA support)
└── README-Web-Calculator.md      ← This file
```

---

## How to Use

### Locally
1. Double-click `zakat-fitra-calculator.html`
2. Opens in your default browser — no internet needed (except for PDF/Image export)

### On a Website
Upload to your web server:
```
public_html/
├── zakat-fitra-calculator.html
├── manifest.json
└── sw.js
```
Access at: `yourdomain.com/zakat-fitra-calculator.html`

### Clean URL (Recommended)
```
public_html/
└── zakat/
    ├── index.html        ← renamed from zakat-fitra-calculator.html
    ├── manifest.json
    └── sw.js
```
Access at: `yourdomain.com/zakat/`

---

## Zakat Calculator — Inputs

### Settings
| Field               | Description                                         |
|---------------------|-----------------------------------------------------|
| Currency            | Select your local currency                          |
| Nisab Based On      | Gold (87.48g) or Silver (612.36g)                  |
| Gold Price per gram | Current gold market price in selected currency      |
| Silver Price per gram | Current silver market price in selected currency  |
| Nisab Threshold     | Auto-calculated — read only                         |

### Assets
| Field                        | Description                              |
|------------------------------|------------------------------------------|
| Cash in Hand                 | Physical cash                            |
| Bank Savings & Deposits      | All bank account balances                |
| Investments & Stocks         | Shares, mutual funds, etc.               |
| Business Inventory           | Trade goods at market value              |
| Gold Weight (grams)          | Weight of gold — value auto-calculated   |
| Silver Weight (grams)        | Weight of silver — value auto-calculated |
| Money Owed To You            | Loans given, pending payments            |
| Rental Income / Other Wealth | Other zakatable income                   |

### Liabilities (Deducted)
| Field               | Description                          |
|---------------------|--------------------------------------|
| Loans & Debts Owed  | Short-term debts to be repaid        |
| Expenses Due        | Pending bills, rent, etc.            |

---

## Fitra Calculator — Inputs

| Field               | Description                                          |
|---------------------|------------------------------------------------------|
| Currency            | Select your local currency                           |
| Fitra Rate Per Person | Set by local masjid/scholars each year             |
| Adults              | Number of adult members                              |
| Children            | Minor dependents                                     |
| Elderly / Dependents | Senior or dependent family members                  |

**Total Fitra = Rate per person × Total members**

---

## Export

Both calculators support exporting a formatted report:

| Format | Output                                      |
|--------|---------------------------------------------|
| PNG    | Image file saved to Downloads               |
| PDF    | Single-page PDF saved to Downloads          |

The report includes: date, settings, all asset/liability values, net wealth, and final Zakat/Fitra amount with status.

> Export requires an internet connection to load `html2canvas` and `jsPDF` from CDN.

---

## Languages Supported

| Code | Language | Direction |
|------|----------|-----------|
| en   | English  | LTR       |
| hi   | Hindi    | LTR       |
| ur   | Urdu     | RTL       |
| ar   | Arabic   | RTL       |
| bn   | Bengali  | LTR       |

---

## Technical Details

- **Pure HTML/CSS/JS** — zero dependencies, zero build steps
- **CDN Libraries** (export only): `html2canvas 1.4.1`, `jsPDF 2.5.1`
- **Browser Support**: Chrome 80+, Firefox 78+, Safari 14+, Edge 80+
- **File Size**: ~60KB (single file)

---

## Disclaimer

This calculator is a guide only. Zakat rules may vary by school of thought. Always verify your calculation with a qualified Islamic scholar.
