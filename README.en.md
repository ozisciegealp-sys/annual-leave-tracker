# Annual Leave Tracking System

[Türkçe](README.md) | **English**

An Excel workbook that tracks **accrued annual leave balances** and **upcoming leave entitlements** across a multi-area retail operation from periodic HR data exports, generating area-level summaries and action lists.

> **Note:** This file is a **rebuilt version** of the system I built and ran for all staff across 6 areas at a coffee chain. Because the original file is no longer in my possession, the structure was rebuilt from memory. All employee data (names, IDs, dates, balances) is synthetic.
>
> In the original workbook, summaries were generated via pivot tables and converted into infographics for area and operations managers. In this version, the same summaries are formula-driven (`COUNTIFS`, `SUMIFS`) with linked charts, so pasting new data updates everything automatically without refreshing pivots.

## What problem it solves

Unused annual leave accumulates across areas. Accrued leave is both a legal entitlement and a shift-planning risk when taken in large blocks. This system:

- Calculates each employee's statutory annual leave entitlement based on tenure and age under Turkish Labour Law.
- Flags employees earning a new entitlement within the next 60 days.
- Derives leave taken during the period from the delta between two HR snapshots.
- Ranks staff with balances ≥ 14 days descending and generates a ready-to-send notification draft for the area manager.
- Displays accrued leave per area, alarm status based on staff over 20 days, and period-over-period change.
- Automatically filters out regions outside the 6 tracked areas when full country-wide HR data is pasted.

## Before

Leave balances were tracked manually; this system moved all calculations and alerts to formulas.

## Workflow

When the periodic HR export arrives:

1. Copy columns A–H from `Veri_Guncel` to `Veri_Onceki` (**as values**) and update the previous snapshot date on `Ayarlar`.
2. Paste the new HR export into `Veri_Guncel` in the same column order and update the current date on `Ayarlar`.
3. Review area comparisons and charts on `Ozet`.
4. Select an area in `Ayarlar` > **Liste bölgesi**; `Liste_14_Gun` outputs the prioritized list and notification text for the area manager to share with store managers.

## Sheets

| Sheet | Purpose |
|---|---|
| `Nasil_Kullanilir` | Workflow, formulas, and colour coding |
| `Ozet` | Per-area headcount, accrued leave, 14+ and 20+ day counts, alarm status, upcoming entitlements, leave used, period delta, new hires and leavers, plus two charts |
| `Liste_14_Gun` | Employees above the planning threshold sorted descending + notification message draft |
| `Takip` | Row-level calculations for every employee |
| `Ayarlar` | Snapshot dates, thresholds, tracked areas, and list filter |
| `Veri_Guncel` / `Veri_Onceki` | Current and previous HR data paste sheets |

## Calculations

**Annual leave entitlement** (Turkish Labour Law No. 4857, Article 53):

| Tenure | Days |
|---|---|
| Under 1 year | None |
| 1–5 years (excl. 5) | 14 |
| 5–15 years (excl. 15) | 20 |
| 15+ years | 26 |
| Age ≤ 18 or ≥ 50 | Minimum 20 |

- **Next entitlement date** = next hire-date anniversary
- **Earned in period** = annual entitlement if the anniversary falls between the two snapshot dates, else 0
- **Used in period** = previous balance + earned in period − current balance
  - Negative results flag as **Kontrol et** (Check — data error or manual adjustment).
  - Staff absent from the previous snapshot flag as **Yeni personel** (New hire).
- **Balance status**: ≥ 14 days = **Planla** (Plan), ≥ 20 days = **ALARM**
- **Area alarm**: triggers **▲ ALARM** when staff over 20 days reaches the threshold (default 5), otherwise **● Planla** if any staff are over 14 days

All thresholds are configurable on `Ayarlar`.

## Setup and data format

No macros or setup required. `izin-takip.xlsx` opens in Excel 2010+ and LibreOffice.

Paste HR data in this column order:

| A | B | C | D | E | F | G | H |
|---|---|---|---|---|---|---|---|
| Employee ID | Full Name | Area | Store | Hire Date | Birth Date | Role | Remaining Leave (days) |

Leave column F blank if birth date is unavailable (age rule is skipped for that row). Column I on `Veri_Onceki` holds a helper formula for detecting leavers—paste only into columns A–H.

## Adapting to your data

| Location | What to adjust |
|---|---|
| `Ayarlar` A15–A20 | Tracked area names (must match HR data spelling) |
| `Ayarlar` B6–B9 | Planning/alarm thresholds, area alarm headcount, upcoming entitlement window |

Pre-sized for up to 500 employees; fill formulas down on `Takip` for larger datasets.

## Limitations

- Partial-year rules, unpaid leave, or company-specific extra leave are excluded.
- Balances come directly from HR exports; the workbook tracks changes rather than computing balances from scratch.
- Notification emails are not sent automatically; the draft text and list are copied and sent manually.

## License

MIT. See `LICENSE` for details.
