# Exercise: Fast-Delivery Network Design Under Demand Uncertainty

## Background

You are the supply chain analytics engineer at a mid-sized consumer goods company. The company operates **some regional distribution centers (DCs)** with known, fixed daily throughput capacity. These DCs ship product to **64 downstream regional markets**, each served today through a mix of direct trucking lanes.

Leadership has launched a "**Fast Delivery**" initiative: customers should receive orders within a **next-day window** wherever possible, and unmet or late demand is costing the company in lost sales and expedite fees. At the same time, Finance wants total network cost (fixed facilities&lane commitments + variable transportation + penalties) kept under control.

The challenge: **market demand cannot be predicted with certainty**. It varies day to day due to promotions, seasonality, weekday effects, and local market dynamics. Demand also tends to move together across regions during shared events (e.g., a national promotion or holiday), so regions are not statistically independent. Supply capacity at each DC, on the other hand, is fixed and known — it depends on physical warehouse throughput, not on demand.

You can have these data:
- **Historical daily demand data** per market (date, day-of-week, demand volume)
- **Known daily supply capacity** at each of the DCs
- **A candidate depot list** between every DC–market pair, each with an estimated per-unit transportation cost and an expected transit time (1–3 days)

## The Problem

Leadership wants a **network design recommendation**: which DC–market depots should be committed to (opened), and how much capacity should be reserved on each, so that:

1. The network reliably meets a **≥95% service level** (fraction of demand fulfilled) across the range of demand outcomes that could realistically occur — not just the "average" day.
2. Order flow that travel on a **slow delivery (>next day delivery)** or that goes **unmet entirely** is penalized, since both undermine the Fast Delivery promise.
3. **Total expected cost** (fixed depot commitments + variable transport + penalties) is minimized.
4. The **fixed supply capacity** at each DC is never exceeded.
5. Decisions about which depot to commit to must be made **before** actual demand for a given day is known — but once demand for a given day is realized, delivery can be routed adaptively across whichever depots were committed.

## Your Task

Propose and simulate an end-to-end analytical approach to this problem. Specifically, address:

### A. Problem framing
- What decisions are "here-and-now" (must be locked in before demand is known) versus "wait-and-see" (can adapt after demand is observed)? Why does this distinction matter for how you model the problem?
- Is minimizing cost and maximizing speed/service really one objective or two competing ones? How would you reconcile them in a single optimization formulation?

### B. Modeling demand uncertainty (ML direction)
- Given only historical demand data, how would you go from raw demand history to something an optimization model can use? Should you produce a single forecast number, or something richer?
- How would you account for the fact that demand across markets is correlated rather than independent?
- What forecasting approach would you choose, and why — and what would you need to check (validation) before trusting its output downstream?

### C. Choosing an optimization model
- What class of optimization model is appropriate for a decision that is made *before* uncertainty resolves, but whose consequences depend on *how* uncertainty resolves?
- How would you formally encode the "≥95% service level" requirement — as a hard constraint, a soft penalty, or something else? What are the trade-offs?
- How would you formulate the trade-off between committing to more/faster (but costlier) order flow versus fewer/cheaper (but slower or riskier) ones?
- At what point would you consider alternative optimization framings — e.g., if you didn't trust the demand distribution's tails, or if you needed hard per-market guarantees rather than network-wide averages?

### D. Validation
- Once you have a proposed network design (which depot to open, how much capacity to reserve), how would you check that it will actually perform well in practice, rather than just performing well on the data used to build it?

## Deliverable

Write up your recommended approach, covering:
1. A clear statement of the decision variables, uncertainty, and objective/constraints (in words or math).
2. Your recommended ML approach for characterizing demand uncertainty, with justification.
3. Your recommended optimization model class, with justification, and how it incorporates the ML output.
4. At least one alternative modeling direction you considered and why you did or didn't choose it.
5. A working prototype — even on synthetic data — implementing your recommended approach.