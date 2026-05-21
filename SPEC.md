# Product Spec: FX REQ Converter

**Author:** Altorien Lockwood, Analyst, FP&A  
**Date:** May 20, 2026  
**Status:** V1 Shipped

---

## Problem Statement

FP&A analysts receive 15-20 REQs per week in foreign currencies (JPY, SGD, GBP, EUR, etc.) that need to be converted to USD for forecasting. Today this requires manually referencing the Indeed Intercompany FX Rate Schedule, finding the correct rate, calculating the monthly run rate, and converting — all in a spreadsheet. This is repetitive, error-prone, and slow.

## Solution

A simple web tool that converts a REQ's total cost from any currency to USD using Indeed's official intercompany FX rates, and displays both the total and monthly breakdown so the analyst can verify the conversion before pasting into their forecast.

## User

FP&A analysts who manage Outpost / vendor forecasting across multiple currencies.

## Inputs

| Field | Description | Example |
|---|---|---|
| **Total Cost** | Full cost of the REQ in local currency | 15,000,000 |
| **Currency** | Source currency of the REQ | JPY |
| **Start Date** | REQ start date | 2026-06-01 |
| **End Date** | REQ end date | 2027-03-31 |

## Outputs

| Output | Description | Example |
|---|---|---|
| **FX Rate Used** | Latest available Indeed intercompany rate | JPY to USD: 0.006279 |
| **Rate As Of** | Month of the rate | April 2026 |
| **Total Cost (USD)** | Converted total | $94,185.00 |
| **Total Days** | Duration of REQ in days | 304 days |
| **Monthly Cost (USD)** | Prorated by actual days in each month | Varies by month |
| **Monthly Cost (Local)** | Prorated by actual days in source currency | Varies by month |

## User Flow

1. User opens the web tool
2. Enters total cost, selects currency from dropdown, enters start and end dates
3. Clicks **Convert**
4. Tool displays:
   - The FX rate used and its effective date
   - Total cost in USD
   - Monthly breakdown table with day-level proration (partial months show day count)
5. User verifies the total matches expectations
6. User copies the USD values into their forecast gsheet via Copy buttons

## Key Features

- **Day-level proration**: Partial months are calculated by actual days, not straight-line division
- **30+ currencies**: All currencies from Indeed Intercompany FX Rate Schedule
- **Copy buttons**: "Copy Table" (all columns) or "Copy USD Only" for pasting into gsheets
- **Audit trail**: Shows FX rate used, effective date, and day counts for verification
- **Auto-formatting**: Numbers format with commas as you type

## FX Rate Source

- **File:** Indeed Intercompany FX Rate Schedule
- **Sheet:** "New Income Statement - Average Rate"
- **Update cadence:** Monthly (published by Accounting)
- **Rate logic:** Always use the latest available month's rate
- **Current rates as of:** April 30, 2026

## Supported Currencies

AED, AUD, BRL, CAD, CHF, CNY, CZK, DKK, EUR, GBP, HKD, HRK, IDR, ILS, INR, JPY, KRW, MXN, MYR, NOK, NZD, PHP, PLN, QAR, RUB, SAR, SEK, SGD, THB, ZAR

## Nice-to-Haves (V2)

- [ ] Batch mode — paste multiple REQs at once
- [ ] Auto-pull latest rates from the Google Sheet instead of manual upload
- [ ] Export to CSV for direct paste into forecast
- [ ] Historical rate toggle (use a specific month's rate instead of latest)

## Technical Details

- Single-page HTML/CSS/JS application
- No backend or dependencies required
- FX rates embedded in the file (manual update each month)
- Runs locally in any browser
