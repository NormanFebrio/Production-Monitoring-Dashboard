# Production Target Monitoring Dashboard

## Project Overview

This project is a **Production Target Monitoring Dashboard** built in Microsoft Excel, connected to an OLAP data source. The dashboard was built to help a production manager track how each production line is performing against its monthly target — in real time, every single day.

The data used here has been fully anonymized (line names and production numbers) to protect company confidentiality.

## Problem Background

Before this dashboard existed, the production team had no easy way to know if a line was falling behind — until it was already too late. Performance gaps were only visible during end-of-month review meetings, which meant by the time anyone noticed a problem, there was almost no time left to fix it.

The production manager needed something simple but powerful: a tool that could show, on any given day, exactly which lines are on track and which ones are not — so the team can act fast, not just react.

The core challenge wasn't just displaying numbers. It was about building a **meaningful comparison** — because comparing today's actual output to the full monthly target isn't fair. You have to compare it to what *should have been produced by today*, given how many working days have passed.

## Objective

- Monitor actual production vs target for each line, every day
- Detect underperforming lines **before** the end of the month — not after
- Adjust the daily target based on how many working days have actually passed (prorated target)
- Calculate the minimum production needed tomorrow (or today, on the last working day) for each line to still hit the monthly target
- Auto-refresh via OLAP connection so there's no manual data entry involved

## Data & Key Metrics

**Data Source:** OLAP-connected production database (all values anonymized)

**Granularity:** Per production line, per working day, per period

The metrics below weren't just pulled from the data — they were designed from scratch based on what the business actually needed to know:

**1. Achievement Rate (All Period)** Actual production from the start of the year ÷ cumulative target up to end of this month. This gives a big-picture view of how the factory is tracking overall.

**2. Prorated Daily Target (the most important one)** Instead of comparing today's output to the full monthly target, this metric adjusts the target based on how far into the month we are:

> Prorated Target = Monthly Target × (Working Days Elapsed ÷ Total Working Days This Month)

So if the monthly target is 260M units and today is day 20 out of 26, the fair target for today is 200M — not 260M. This is what makes the dashboard actually useful for daily decision-making.

**3. Minimum Production Tomorrow (or Today)** How many units does each line need to produce tomorrow to still be able to hit the monthly target by the end of the month?

> Min Tomorrow = (Monthly Target − Actual to Date) ÷ Remaining Working Days

**4. Per-Line Gap (Kurang)** How many units is each line currently short of its target? Shown both for the full period and for the current month.

## Dashboard Breakdown

The dashboard has 4 main sections:

**1. Target All Period** Shows overall achievement from the start of the year to today, compared against the cumulative target up to end of this month. Think of this as the "big picture" health check.

**2. Target Bulan Ini (This Month's Target)** Compares actual production this month vs the full monthly target. Two numbers side by side, plus a gauge chart to make it instantly readable.

**3. Target Produksi Bulan Ini Per Hari Ini (Prorated Monthly Target)** This is the most important section for daily decision-making. It shows the adjusted target — how much *should* have been produced by today based on working days used. If a line is below this number, it's already behind schedule.

It also shows **Hari Kerja Terpakai** (Working Days Used) — today was day 25.8 out of 25.8, meaning this data was captured at the very end of the last working day of the month.

**4. Produksi Minimal Besok / Hari Ini (Minimum Production Tomorrow)** Automatically calculates the minimum units each line needs to hit tomorrow (or today, if it's the last working day) to still reach the monthly target. The section title changes dynamically based on the date.

**5. Capaian Target Tiap Line — All Period & Per Hari Ini** Breaks down achievement rate per production line (Line A through G), for both the full period and the current month. Each line has its own gauge chart and shows exactly how many units it's still short.

**6. Produksi Minimal Hari Ini per Line** At the bottom, a quick reference table showing the exact minimum units each line needs to produce today to still hit target.

## Design Decisions

Every design choice here was made with the end user in mind — a production manager who needs to make fast decisions, not stare at a spreadsheet.

| Design Choice | Why |
| --- | --- |
| Gauge charts for achievement % | Instant visual read — no need to interpret numbers, green/yellow/red tells the story |
| 3 monitoring levels (All Period / This Month / Per Today) | Different questions need different time frames. "Are we on track this year?" vs "Are we on track today?" are different questions |
| Prorated daily target | Comparing today's output to the full monthly target would be misleading. Adjusting for working days makes it a fair, actionable comparison |
| Forward-looking "Minimum Tomorrow" section | Shifts the focus from "how bad are we doing" to "what do we need to do tomorrow" — much more useful for a manager |
| OLAP auto-refresh | No manual data entry = no human error. One click, data is always up to date |
| Dynamic section title (Besok/Hari Ini) | Small detail, but it means the dashboard is always contextually correct depending on the date |

## Impact

Before this dashboard, production performance was only reviewed at end-of-month meetings. By that point, there was nothing left to do — the month was over.

This dashboard moved the visibility from **T+30 days → same day**. Underperforming lines can now be spotted and acted on within the same working day, giving the team time to actually do something about it.

Specifically:

- Production managers can now see at a glance if a line is behind the prorated target — not just the monthly target
- The "Minimum Tomorrow" section gives floor supervisors a concrete number to aim for, without having to do any calculation themselves
- The OLAP connection means there's zero lag between actual production data and what's shown on the dashboard
