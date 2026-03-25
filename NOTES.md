# Product & Strategy Notes

---

## ML Matching vs. Research-Backed Weights

**Question:** Should we use an ML matching algorithm to fine-tune compatibility weights (via profile selection + post-date feedback), or are hard research-backed weights sufficient?

**Answer:** Hybrid — but with strict constraints on what ML is allowed to touch.

---

### Where hard research weights are sufficient (and ML would hurt)

The research backing for the current weights is empirically strong — Gottman's attachment/communication findings, Tashiro's values similarity data, the Big Five relationship literature. For a global model with no user data yet, hard weights are not just "good enough" — they're likely more accurate than anything ML could produce from early-stage platform data.

**Critical risk: ML trained on profile selection will learn the wrong thing.**

When someone selects a profile, they're responding to attractiveness, a photo, a bio line — not compatibility. If profile selection data is used to fine-tune compatibility weights, the model will systematically bias toward superficial factors and away from the dimensions research weights correctly prioritise. Dating apps that optimised on engagement (Tinder, early Bumble) proved this at scale: high match rates, poor relationship outcomes.

Eastwick & Finkel documented this directly — stated preferences and revealed preferences diverge significantly when people actually meet.

---

### Where ML is genuinely valuable

**Post-date feedback is the right signal.**
"Would you see them again?" and "What worked / didn't work?" collected after a date is causally closer to compatibility than any profile interaction data. Build collection infrastructure for this from day one, even if processed manually early on.

**Individual-level weight correction.**
Research weights are correct *on average* — but individuals vary. Someone who grew up in a deeply religious household may effectively weight values alignment at 50%, not 35%. A lightweight personalization layer ("based on your feedback, we're tuning your personal weights") built on top of the global research model is both meaningful and tractable. Fall back to research weights when a user has no feedback history — this solves the cold start problem gracefully.

**Question predictive power analysis.**
Not all 35 questions will prove equally discriminating on the platform. Some will have high variance and high outcome correlation; others will cluster users without predicting who connects. ML can identify which questions earn their weight over time, informing questionnaire pruning and future version design.

---

### The layered architecture

Deploy in sequence as data accumulates:

| Layer | What it does | When to deploy |
|---|---|---|
| Research weights | Global model — defensible baseline, no cold start | Now |
| Outcome-based weight tuning | Adjust global weights using post-date feedback across all users | After ~500 dates with feedback |
| Personalization layer | Per-user weight corrections using individual feedback history | After a user has 3–5 rated dates |

**Bayesian framing:** Research weights = prior. Post-date feedback = likelihood. The posterior updates toward each user's revealed preferences while staying anchored to the research baseline when data is sparse.

---

### What to avoid

- Using profile selection or match rate as a training signal — wrong target variable
- Optimising for re-engagement or time-in-app — misaligned with actual goal
- Replacing the global model with ML before sufficient outcome data — trading a validated model for statistical noise
- Letting the model become uninterpretable — users should be able to understand why they were matched

---

### Bottom line

Post-date feedback driving incremental weight personalisation on top of a research-grounded baseline is the right architecture. Profile selection as a training signal should be excluded — it optimises for attraction, not compatibility, and will quietly corrupt the model over time. Build feedback collection infrastructure now, keep hard weights until real outcome data exists, and treat ML as a layer on top rather than a replacement.

---

## Validation Strategy: Question Set & Successful Couples

**Question:** Should we validate the question set by asking what successful couples say are important metrics?

**Consideration:** Asking successful couples what mattered will give a retrospective, survivor-filtered view. Couples reframe their history through the lens of current happiness. The couples who matched on the same dimensions and *didn't* work out aren't in the data.

**Real validation requires a longitudinal cohort approach:**
- Singles complete the profile at t=0
- Track relationship formation and quality at 6, 12, 24 months
- Correlate initial profile dimensions with outcomes

Until that data exists, the model is theoretically grounded but not empirically validated — which is an acceptable starting state, but worth being explicit about.

**Interim approach:** Grounding weight decisions in published research (Gottman, Tashiro, Bowlby, Eastwick/Finkel) before platform-specific data exists is the correct move. Successful couples check-ins over time (longitudinal, not retrospective) could be a meaningful supplement down the road.

---

## Weighting Rationale (Singles Model)

| Dimension | Weight | Research basis |
|---|---|---|
| Values & Life Direction | 35% | Gottman (2015): shared meaning/purpose predicts stability. Tashiro (2014): value similarity is the #1 LTR predictor. |
| Attachment & Communication | 28% | Bowlby/Ainsworth attachment theory. Gottman (1994): communication patterns predict divorce with >90% accuracy. Johnson (EFT, 2013). |
| Lifestyle Alignment | 17% | Elevated vs. typical models — daily friction is real for singles building from scratch. Stafford & Canary (2006). |
| Personality Fit | 10% | Big Five: conscientiousness and agreeableness most predictive (McCrae & Costa). Luo & Klohnen (2005). |
| Shared Interests | 10% | Aron et al. (2000): self-expansion through shared novel activities increases closeness. Moderate evidence; weighted above strict research default given bonding value. |

---

## Couples App vs. Singles App: Key Design Difference

The couples app (README.md) asks **state questions** — "how are you doing on X right now?"
The singles app (singles.html) asks **trait/disposition questions** — "how do you tend to behave / what do you value?"

This distinction is load-bearing. Singles can't answer state questions meaningfully without an existing relationship. Conflating the two would produce a profile that reflects an imagined ideal rather than an accurate self-portrait.
