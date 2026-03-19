# PRD: ROI Calculator

## Overview
A single-page interactive calculator that lets ops leaders (Dana persona) input their company's current numbers and instantly see how much time and money Headcount Zero would save them. Lives at `/roi.html` and is linked from the main site.

## Goal
Convert skeptical evaluators into free trial signups by making the value concrete and personal — not generic marketing claims, but *their* numbers reflected back at them.

## Target User
VP/Head/Dir of Operations at a 50–500 person company who is mid-evaluation and thinking "this looks nice, but is it worth the switch?" The calculator gives her something to screenshot and drop into a Slack thread or email to her CEO.

## Design Principles
- **Fast.** Calculator should be usable in under 60 seconds. No signup, no email gate.
- **Honest.** Use conservative multipliers. Show the math. Dana distrusts inflated claims.
- **Personal.** Results should feel specific to her company, not generic.
- **Shareable.** Results summary should be easy to copy or screenshot for internal buy-in.

---

## Inputs (Left Side)

All inputs have sensible defaults pre-filled (based on median for 50–200 person companies) so the calculator shows results immediately on load. User adjusts to match their reality.

| Input | Type | Default | Range | Why |
|-------|------|---------|-------|-----|
| Number of employees | Slider + number | 120 | 10–1,000 | Drives per-employee cost calc and scales all estimates |
| Open roles per quarter | Slider + number | 8 | 1–50 | Drives hiring time savings |
| Current avg. time-to-fill (days) | Slider + number | 45 | 14–90 | Delta against HCZ benchmark (19 days) = hiring speed gain |
| Hours your team spends on review cycles (per quarter) | Slider + number | 40 | 5–200 | Direct time savings from automated nudges + tracking |
| Comp conversations that feel like guesswork (%) | Slider | 60% | 0–100% | Drives comp module value — translates to better offer acceptance |
| Number of states you hire in | Slider + number | 4 | 1–50 | Drives compliance value — more states = more risk = more value |

## Outputs (Right Side)

Results update in real-time as inputs change. No "Calculate" button — it's always live.

### Primary Metrics (Large, prominent)
1. **Hours saved per month** — Sum of time savings across hiring, reviews, onboarding/offboarding, compliance
2. **Annual cost savings** — Hours saved × blended ops team hourly cost ($75/hr) + reduced cost-per-hire + compliance risk reduction
3. **Payback period** — Annual cost of Headcount Zero (employees × $12/mo × 12) vs. annual savings. Show "X weeks" or "X months"

### Breakdown Table
Show how each module contributes to the total so Dana can see which pain point delivers the most value:

| Module | Hours saved / month | Annual value |
|--------|-------------------|--------------|
| Hiring ops | calculated | calculated |
| Performance reviews | calculated | calculated |
| Comp benchmarking | calculated | calculated |
| Compliance | calculated | calculated |
| Onboarding & offboarding | calculated | calculated |
| **Total** | **sum** | **sum** |

### Calculation Logic (Conservative Estimates)

**Hiring ops:**
- Current cost-per-hire overhead: 8 hrs of ops team time per role (sourcing coordination, scheduling, pipeline management)
- With HCZ: 2.5 hrs per role
- Hours saved/quarter = open_roles × 5.5 hrs
- Time-to-fill improvement: (current_ttf - 19) days × open_roles = total days reclaimed (shown as secondary stat)

**Performance reviews:**
- Input is direct: hours spent per quarter on review admin
- HCZ reduces by 90% (auto-nudges, tracking, templates)
- Hours saved/quarter = input × 0.9

**Comp benchmarking:**
- Comp conversations that are guesswork × estimated 1.5 hrs each wasted in back-and-forth
- Estimated comp conversations/quarter = employees × 0.05 (5% get comp adjustments per quarter)
- Hours saved = guesswork_pct × conversations × 1 hr saved per conversation
- Secondary value: improved offer acceptance (show delta: 69% → 87% projected)

**Compliance:**
- Base: 3 hrs/month for single-state compliance management
- Each additional state adds ~2 hrs/month of policy review/updates
- HCZ reduces to 0.5 hrs/month total (review flagged changes only)
- Hours saved/month = 3 + (states - 1) × 2 - 0.5

**Onboarding & offboarding:**
- Estimated hires/quarter = open_roles (rough proxy)
- Estimated departures/quarter = employees × 0.04 (quarterly turnover)
- Current: 4 hrs per onboard + 3 hrs per offboard
- With HCZ: 1 hr per onboard + 0.5 hrs per offboard
- Hours saved = hires × 3 + departures × 2.5

**Cost conversion:**
- All hours valued at $75/hr (blended ops team cost)
- Annual HCZ cost = employees × $12 × 12 (Growth plan, most common)
- Net annual savings = total value - HCZ cost
- Payback period = HCZ annual cost / (total annual value / 12) in months

## CTA
Below the results: "Start your 14-day free trial" button + "No credit card required" note. Same CTA style as main site.

## Secondary CTA
"Share these results" — copies a plain-text summary to clipboard:
```
Our estimated ROI with Headcount Zero:
- 47 hours saved per month
- $42,300 in annual savings
- Payback period: 6 weeks
- Biggest win: Hiring ops (22 hrs/mo saved)
Based on: 120 employees, 8 open roles/qtr, 4 states
```

## Technical Requirements
- Single HTML file, same visual style as main site (dark theme, same CSS variables)
- Pure HTML/CSS/JS — no framework, no build step
- Responsive (works on mobile, though primary use is desktop)
- All calculation happens client-side
- No tracking, no email gate, no analytics requirement for v1

## Out of Scope
- Saved/bookmarkable URLs with encoded inputs
- PDF export
- Comparison against specific competitors
- Industry-specific benchmarks
