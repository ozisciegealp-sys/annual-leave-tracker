# Annual Leave Tracking System

[Türkçe](README.md) | **English**

An Excel workbook that tracks **accrued annual leave balances** and **upcoming statutory entitlements** across a 6-area retail operation from periodic HR exports, generating area-level alarm summaries and prioritized action lists.

> **Confidentiality & Synthetic Data Note:** This workbook is a **rebuilt version** of the annual leave tracking system I built and ran for field staff across 6 areas. In compliance with confidentiality and data protection rules, no real employee records, IDs, store names, or brand identifiers are included; all data is synthetic.

## What problem it solves

Unused annual leave accumulates across multi-store field operations, creating both statutory liability and shift-planning bottlenecks when taken in large blocks. This system:

- Calculates each employee's statutory annual leave entitlement by tenure and age under **Turkish Labour Law No. 4857 (Art. 53)**.
- Flags staff earning a new entitlement within the next **60 days**.
- Derives leave taken during the period from the delta between two HR snapshots, flagging anomalies (`Kontrol et`) as well as new hires and leavers.
- Ranks staff with balances **≥ 14 days** descending and generates a ready-to-send manager notification draft.
- Outputs area-level **▲ ALARM** or **● Planla** status based on staff exceeding 20 days, while automatically isolating the 6 tracked areas when country-wide HR exports are pasted.

## Before

Leave balances and anniversary dates were filtered manually from raw HR spreadsheets; all statutory rules, alarms, and area lists were moved to dynamic formulas.

## Workflow

When a new periodic HR export arrives:

1. Paste columns A–H from `Veri_Guncel` into `Veri_Onceki` (**as values**) and update the previous snapshot date on `Ayarlar`.
2. Paste the new HR export into `Veri_Guncel` in the same column order and update the current date on `Ayarlar`.
3. Review area comparisons, alarm statuses, and charts on `Ozet`.
4. Select an area in `Ayarlar` > **Liste bölgesi** to share the prioritized list and notification text on `Liste_14_Gun` with area and store managers.

## Sheets

| Sheet | Purpose |
|---|---|
| `Nasil_Kullanilir` | Workflow, statutory rules, and colour coding |
| `Ozet` | Per-area headcount, accrued leave, 14+ and 20+ day counts, alarm status, upcoming entitlements, leave used, period delta, and two charts |
| `Liste_14_Gun` | Employees above threshold sorted descending + manager notification draft |
| `Takip` | Row-level tenure, entitlement, anniversary, and delta formulas |
| `Ayarlar` | Snapshot dates, planning/alarm thresholds (14 / 20 days), tracked areas, and list filter |
| `Veri_Guncel` / `Veri_Onceki` | Current and previous HR data paste tabs |

## Calculations

**Statutory annual leave** (Turkish Labour Law No. 4857, Article 53):

| Tenure / Age Bracket | Entitlement (Days) |
|---|---|
| Under 1 year | None |
| 1–5 years (excl. 5) | 14 |
| 5–15 years (excl. 15) | 20 |
| 15+ years | 26 |
| Age ≤ 18 or ≥ 50 | Minimum 20 |

- **Next entitlement date** = next hire-date anniversary
- **Earned in period** = annual entitlement if anniversary falls between the two snapshot dates, else `0`
- **Used in period** = `previous balance + earned in period − current balance` (negative results flag as `Kontrol et`; new rows flag as `Yeni personel`)
- **Area alarm**: triggers **▲ ALARM** when staff over 20 days reaches threshold (default `5`), otherwise **● Planla** if any staff exceed 14 days

## Setup and data format

Macro-free; `izin-takip.xlsx` opens directly in Excel 2010+ and LibreOffice. Paste HR exports into columns `A:H` (`Employee ID | Full Name | Area | Store | Hire Date | Birth Date | Role | Remaining Leave`).

## License

MIT. See `LICENSE` for details.
