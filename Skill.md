# FearCycle

## Skill Name

FearCycle

## Description

FearCycle is a long-term Spot trading strategy that uses the Crypto Fear & Greed Index as its primary market-sentiment signal.

The strategy follows a simple contrarian approach:

- Accumulate during market fear.
- Hold during neutral conditions.
- Reduce exposure during greed.
- Reduce exposure more aggressively during extreme greed.

FearCycle does not use leverage or Futures positions.

---

## Objective

The objective is to help an AI trading agent make disciplined long-term Spot allocation decisions based on market sentiment instead of short-term price movements.

The Skill is designed to avoid trying to predict exact tops and bottoms.

Instead, it gradually changes exposure as market sentiment moves between predefined zones.

---

## Market

- Market: Spot
- Trading Style: Long-term
- Leverage: None
- Futures: Not used
- Primary Signal: Crypto Fear & Greed Index

---

## Fear & Greed Zones

| Index | Market Condition | Signal |
|---|---|---|
| 0–20 | Extreme Fear | STRONG BUY |
| 21–35 | Fear | BUY |
| 36–64 | Neutral | HOLD |
| 65–79 | Greed | REDUCE |
| 80–100 | Extreme Greed | STRONG REDUCE |

---

## Core Logic

The agent must:

1. Retrieve the latest Fear & Greed Index.
2. Verify that the value is current.
3. Identify the corresponding sentiment zone.
4. Check the current Spot position.
5. Check available capital.
6. Generate a signal.
7. Determine the appropriate allocation or reduction.
8. Respect the rebalance interval.
9. Record the signal and action.
10. Re-evaluate when sentiment changes or the rebalance interval is reached.

---

## Position Allocation

FearCycle uses gradual allocation instead of committing the entire available capital at once.

### Extreme Fear

When the index is between 0 and 20:

- 0–10 → allocate up to 30% of available strategy capital
- 11–20 → allocate up to 20%

Signal:

`STRONG BUY`

### Fear

When the index is between 21 and 35:

- Allocate up to 10% of available strategy capital.

Signal:

`BUY`

### Neutral

When the index is between 36 and 64:

- Do not initiate a new position based on Fear & Greed alone.
- Maintain the existing position.

Signal:

`HOLD`

### Greed

When the index is between 65 and 79:

- Reduce approximately 10% of the current strategy position.

Signal:

`REDUCE`

### Extreme Greed

When the index is between 80 and 100:

- 80–89 → reduce approximately 20%
- 90–100 → reduce approximately 30%

Signal:

`STRONG REDUCE`

The strategy should not automatically close 100% of a position solely because the index reaches extreme greed.

---

## Execution Rules

### Rule 1 — Current Data

Always obtain the latest available Fear & Greed Index before generating a new signal.

Historical values must not be treated as current.

### Rule 2 — Spot Only

FearCycle must only generate Spot trading actions.

It must not open Futures positions or use leverage.

### Rule 3 — Gradual Position Changes

The agent should avoid committing the entire portfolio based on a single sentiment reading.

Buying and selling should occur progressively.

### Rule 4 — Rebalance Protection

The agent must not repeatedly execute the same order while the Fear & Greed value remains in the same zone.

A configurable rebalance interval should be used.

Recommended default:

`7 days`

### Rule 5 — Capital Protection

Never allocate more capital than is available for the strategy.

If there is insufficient available capital, the agent must not execute the Buy signal.

### Rule 6 — Position Protection

The agent must never reduce more than the currently available strategy position.

### Rule 7 — Signal vs Execution

A signal and an executable order are different things.

Example:

Fear & Greed = 8

Signal:

`STRONG BUY`

But available capital = 0.

Result:

`NO EXECUTION`

Reason:

`Insufficient available capital`

---

## Core Parameters

```text
EXTREME_FEAR_MAX = 20
FEAR_MAX = 35
NEUTRAL_MAX = 64
GREED_MAX = 79

BUY_EXTREME_0_10 = 30%
BUY_EXTREME_11_20 = 20%
BUY_FEAR = 10%

REDUCE_GREED = 10%
REDUCE_EXTREME_GREED_80_89 = 20%
REDUCE_EXTREME_GREED_90_100 = 30%

DEFAULT_REBALANCE_INTERVAL = 7 days

LEVERAGE = 0
MARKET = SPOT
