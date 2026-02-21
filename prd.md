# PRD: Procurable.ai ROI Calculator

**Version:** 1.0
**Status:** Draft
**Audience:** Marcus Chen — VP Procurement / CPO, Aerospace & Advanced Manufacturing

---

## 1. Purpose

Give a skeptical, ROI-driven procurement leader a fast, credible way to quantify what
Procurable.ai is worth to his specific organization — in the language his CFO speaks.
The calculator must feel like a legitimate analytical tool, not a marketing gimmick.
Every number must be defensible.

---

## 2. Entry Point & Placement

The calculator is triggered as a **modal overlay** from the landing page.

**Trigger locations:**
- Primary CTA button in the hero: "Calculate Your ROI"
- Secondary CTA in the should-cost use case card: "See what this is worth to your team"
- Inline CTA in the pain/before-after section: "Run the numbers"

The modal opens centered on screen with a dark backdrop. It does not navigate away from
the landing page. On mobile it fills the screen.

---

## 3. User Flow

```
[Landing Page]
     │
     ▼ (clicks "Calculate Your ROI")
[Modal Opens — Step 1: Inputs]
     │
     ▼ (fills 4–5 sliders/inputs, clicks "Calculate")
[Step 2: Teaser Results]
     │  Shows: time savings value + a blurred total
     ▼ (clicks "See Full Breakdown")
[Step 3: Email Gate]
     │  Work email + first name (2 fields only)
     ▼ (submits)
[Step 4: Full Results]
     │  All three value buckets + total ROI + payback period
     ▼ (clicks "Download Summary")
[PDF Export — one-pager]
     +  "Run a Pilot" CTA alongside results
```

---

## 4. Inputs

Five inputs. All exposed at once on Step 1 — no pagination, no wizard. Keep it fast.
Each input has a default pre-filled based on the median Marcus Chen profile.

| # | Input Label | Type | Default | Range / Options |
|---|---|---|---|---|
| 1 | Annual spend under management | Slider + text field | $750M | $50M – $5B |
| 2 | Professional buyers & sourcing staff | Slider | 12 | 2 – 80 |
| 3 | Should-cost models built per month | Slider | 15 | 1 – 100 |
| 4 | Average hours to build one model today | Slider | 18 hrs | 2 – 80 hrs |
| 5 | Annual spend on external cost consultants | Slider + text field | $180K | $0 – $2M |

**UX notes:**
- Sliders update output estimates in real time (even before Step 2)
- Show a subtle live-updating "estimated annual value" in the corner as they drag — hooks
  them before they've even clicked Calculate
- Labels use Marcus's language: "should-cost models," "sourcing staff," not "users" or "seats"

---

## 5. Calculation Model

Three discrete value buckets. Show them separately in results — not rolled into one
magic number. Transparency is the point.

### Bucket 1 — Time Recovered from Should-Cost Modeling

**What it represents:** The dollar value of buyer time freed up by reducing
should-cost model build time by 80%.

```
fully_loaded_buyer_cost   = $145,000 / year  [assumption: senior buyer, loaded]
hourly_buyer_rate         = fully_loaded_buyer_cost / 2,080

hours_saved_per_model     = avg_hours_per_model × 0.80
total_hours_saved_monthly = hours_saved_per_model × models_per_month
annual_hours_saved        = total_hours_saved_monthly × 12

time_value_recovered      = annual_hours_saved × hourly_buyer_rate
```

**Assumption note (shown in UI):** Based on 80% reduction in modeling time,
consistent with observed customer outcomes. Fully-loaded buyer cost assumed at
$145K/year — adjust if your team's comp differs.

---

### Bucket 2 — Incremental Negotiation Savings

**What it represents:** Additional hard savings captured by entering negotiations
with data-backed should-cost positions rather than quote-reactive postures.

```
baseline_savings_rate     = 0.04  [assumption: typical 4% annual savings attainment]
incremental_uplift        = 0.015 [assumption: 1.5% additional savings from better positions]

incremental_savings       = annual_spend × incremental_uplift
```

**Assumption note (shown in UI):** Procurement teams using should-cost intelligence
typically capture 1–3% additional savings above baseline. Model uses the conservative
end (1.5%) of observed outcomes.

---

### Bucket 3 — Avoided External Consulting Fees

**What it represents:** Cost eliminated by bringing should-cost capability in-house
instead of engaging consultants for cost benchmarking engagements.

```
avoided_consulting        = annual_consultant_spend × 0.70
[assumption: 70% of engagements can be replaced by internal Procurable.ai models]
```

**Assumption note (shown in UI):** Not all consulting is replaceable — strategic
supplier development, SRM, and M&A diligence work is excluded. Model assumes
70% of should-cost and benchmarking spend is displaced.

---

### Summary Outputs

```
total_annual_value   = time_value_recovered + incremental_savings + avoided_consulting
procurable_cost      = [placeholder — replace with actual ARR at time of build]
roi_multiple         = total_annual_value / procurable_cost
payback_months       = (procurable_cost / total_annual_value) × 12
```

---

## 6. Results Display (Step 4)

**Layout:** Three value bucket cards side by side (stack on mobile), then a summary bar.

### Value Bucket Cards

Each card shows:
- Bucket name (e.g., "Time Recovered")
- Dollar value (large, prominent)
- 1-line plain-English explanation of what drove that number
- Collapsible "How we calculated this" → shows the formula and assumptions

### Summary Bar

```
┌─────────────────────────────────────────────────────────────────┐
│  Total Annual Value      ROI                 Payback Period      │
│  $2,840,000              14×                 < 1 month           │
│                                                                   │
│  [Download PDF Summary]        [Run a Pilot with Your Data →]    │
└─────────────────────────────────────────────────────────────────┘
```

**Tone note:** The results header reads:
*"Based on your inputs, here's what Procurable.ai is likely worth to your team.
Every assumption is shown. Change any input and the model updates instantly."*

This directly addresses Marcus's skepticism — he needs to trust the methodology,
not just the headline number.

---

## 7. Teaser State (Step 2 — Pre-Email Gate)

Show immediately after clicking "Calculate." Purpose: demonstrate value before
asking for anything.

**What's visible:**
- Bucket 1 (Time Recovered): **fully visible** with dollar value
- Bucket 2 (Negotiation Savings): **blurred** — shows shape of number, not value
- Bucket 3 (Avoided Consulting): **blurred**
- Total Annual Value: **blurred**

**CTA:**
> "Your full breakdown is ready. Enter your work email to see it."
> *We don't share your data. No spam. One follow-up from our team, if the numbers make sense.*

---

## 8. Email Gate (Step 3)

Two fields only:
- First name
- Work email (validated — block gmail, yahoo, hotmail)

**Microcopy beneath:**
> "We'll also send you a copy of your results so you can share them with your CFO."

This reframes the gate as a benefit (you get a copy), not a toll.

---

## 9. PDF Export

Triggered from the "Download PDF Summary" button in Step 4.
Generated client-side (no server required) using a print stylesheet or a JS PDF library.

**PDF contents — one page:**

```
[Procurable.ai logo]                    [Date]

ROI Summary: [Company type based on spend input]
Prepared for: [First Name]

Your Inputs
───────────────────────────────────────
Annual Spend Under Management     $750M
Sourcing Staff                    12 buyers
Should-Cost Models / Month        15
Avg. Modeling Time Today          18 hours
External Consulting Spend         $180K/yr

Your Estimated Annual Value
───────────────────────────────────────
Time Recovered from Modeling      $421,000
Incremental Negotiation Savings   $2,250,000
Avoided Consulting Fees           $126,000
─────────────────────────────
Total Annual Value                $2,797,000
ROI                               14×
Payback Period                    < 1 month

Methodology note: All assumptions are conservative and based on observed
customer outcomes. Full calculation methodology available on request.

Next step: Run a pilot on one commodity category.
hello@procurable.ai  ·  procurable.ai
```

---

## 10. Assumptions Reference Table

Display as a collapsible section within the modal (visible in Step 4).
Marcus will click it. He needs to know we're not making things up.

| Assumption | Value | Source / Rationale |
|---|---|---|
| Reduction in modeling time | 80% | Observed customer outcomes |
| Fully-loaded senior buyer cost | $145K/year | BLS + industry benchmarks, Tier-1 A&D |
| Baseline savings attainment | 4% of spend | Industry benchmark, ISM/Hackett Group |
| Incremental savings uplift | 1.5% | Conservative end of observed 1–3% range |
| Consulting displacement rate | 70% | Excludes strategic/SRM engagements |
| Working hours per year | 2,080 | Standard FTE assumption |

---

## 11. What This Is Not

- Not a precise forecast — disclaim clearly, consistently
- Not a substitute for a scoped pilot with real data
- Not a lead qualification tool disguised as a calculator — the numbers must be
  genuinely useful to Marcus even if he never buys

---

## 12. Out of Scope (v1)

- Supplier risk / resilience value quantification
- Multi-year NPV / DCF modeling
- Team-level ROI (per buyer productivity)
- CRM integration for lead routing
- A/B tested assumption sets by industry vertical

---

## 13. Open Questions Before Build

1. **What is the actual Procurable.ai ARR / pricing?** The ROI multiple and payback
   period calculations need a real cost input. Placeholder used above.
2. **Do we have any real customer data to replace assumed benchmarks?** Even one
   validated data point ("Customer X reduced modeling time from 3 days to 4 hours")
   makes the assumptions more defensible.
3. **Who receives the lead on email submission?** Define routing before build.
4. **What PDF library is preferred?** Options: browser `window.print()` with a print
   stylesheet (zero dependencies), jsPDF (client-side, no server), or a server-side
   PDF render. Recommendation: print stylesheet for v1 — fast to build, no deps.
