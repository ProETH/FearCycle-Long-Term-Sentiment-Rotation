# FearCycle — Examples

## Example 1 — Extreme Fear

Input:
{
  "fear_greed": 12,
  "available_strategy_capital": 5000,
  "current_position": 0,
  "days_since_last_execution": 10
}

Expected:
- Zone: Extreme Fear
- Signal: STRONG BUY
- Action: ACCUMULATE
- Allocation: 20%
- Amount: $1,000
- Market: Spot
- Leverage: None

Reason:
FearCycle detects Extreme Fear and recommends gradual Spot accumulation.

---

## Example 2 — Extreme Fear Below 10

Input:
{
  "fear_greed": 7,
  "available_strategy_capital": 5000,
  "current_position": 0,
  "days_since_last_execution": 10
}

Expected:
- Zone: Extreme Fear
- Signal: STRONG BUY
- Action: ACCUMULATE
- Allocation: 30%
- Amount: $1,500

Reason:
A Fear & Greed reading below 10 represents the strongest accumulation zone.

---

## Example 3 — Fear

Input:
{
  "fear_greed": 30,
  "available_strategy_capital": 5000,
  "current_position": 1000,
  "days_since_last_execution": 10
}

Expected:
- Zone: Fear
- Signal: BUY
- Action: ACCUMULATE
- Allocation: 10%
- Amount: $500

Reason:
FearCycle allows gradual accumulation while sentiment remains fearful.

---

## Example 4 — Neutral

Input:
{
  "fear_greed": 50,
  "available_strategy_capital": 5000,
  "current_position": 2000,
  "days_since_last_execution": 10
}

Expected:
- Zone: Neutral
- Signal: HOLD
- Action: NO NEW BUY
- Allocation: 0%

Reason:
FearCycle does not open a new sentiment-based position in the neutral zone.

---

## Example 5 — Greed

Input:
{
  "fear_greed": 72,
  "current_position": 4000,
  "days_since_last_execution": 10
}

Expected:
- Zone: Greed
- Signal: REDUCE
- Action: TAKE PARTIAL PROFIT
- Reduction: 10%
- Amount: $400

Reason:
Sentiment has entered the Greed zone, so FearCycle reduces part of the existing position.

---

## Example 6 — Extreme Greed

Input:
{
  "fear_greed": 85,
  "current_position": 4000,
  "days_since_last_execution": 10
}

Expected:
- Zone: Extreme Greed
- Signal: STRONG REDUCE
- Action: REDUCE POSITION
- Reduction: 20%
- Amount: $800

Reason:
FearCycle reduces exposure as market sentiment becomes excessively optimistic.

---

## Example 7 — Very Extreme Greed

Input:
{
  "fear_greed": 95,
  "current_position": 4000,
  "days_since_last_execution": 10
}

Expected:
- Zone: Extreme Greed
- Signal: STRONG REDUCE
- Action: REDUCE POSITION
- Reduction: 30%
- Amount: $1,200

Reason:
Extreme greed triggers the strongest reduction signal.

FearCycle does not automatically sell 100% of the position.

---

## Example 8 — Rebalance Protection

Previous evaluation:
{
  "fear_greed": 18,
  "signal": "STRONG BUY",
  "last_execution": "2026-08-10T10:00:00Z"
}

Current evaluation:
{
  "fear_greed": 19,
  "available_strategy_capital": 4000,
  "current_position": 2000,
  "days_since_last_execution": 1
}

Expected:
- Signal: STRONG BUY
- Execution: NO TRADE

Reason:
The sentiment zone has not changed and the rebalance interval has not been reached.

The agent must not repeatedly buy every time it checks the same signal.

---

## Example 9 — Rebalance Allowed

Input:
{
  "fear_greed": 16,
  "available_strategy_capital": 4000,
  "current_position": 2000,
  "days_since_last_execution": 10
}

Expected:
- Zone: Extreme Fear
- Signal: STRONG BUY
- Action: ACCUMULATE
- Allocation: 20%
- Amount: $800

Reason:
The signal remains active and the rebalance interval has been reached.

---

## Example 10 — Fear Becomes Extreme Fear

Previous:
{
  "fear_greed": 34,
  "zone": "Fear",
  "signal": "BUY"
}

Current:
{
  "fear_greed": 19,
  "zone": "Extreme Fear"
}

Expected:
- Previous Signal: BUY
- Current Signal: STRONG BUY
- Action: ACCUMULATE

Reason:
Sentiment moved from Fear into Extreme Fear.

---

## Example 11 — Extreme Fear Becomes Neutral

Previous:
{
  "fear_greed": 12,
  "zone": "Extreme Fear",
  "signal": "STRONG BUY"
}

Current:
{
  "fear_greed": 51,
  "zone": "Neutral"
}

Expected:
- Previous Signal: STRONG BUY
- Current Signal: HOLD
- Action: NO NEW BUY

Reason:
Current verified data overrides the previous signal.

---

## Example 12 — Neutral Becomes Greed

Previous:
{
  "fear_greed": 55,
  "zone": "Neutral"
}

Current:
{
  "fear_greed": 70,
  "zone": "Greed"
}

Expected:
- Previous Signal: HOLD
- Current Signal: REDUCE
- Reduction: 10%

---

## Example 13 — Greed Becomes Extreme Greed

Previous:
{
  "fear_greed": 78,
  "zone": "Greed",
  "signal": "REDUCE"
}

Current:
{
  "fear_greed": 88,
  "zone": "Extreme Greed"
}

Expected:
- Previous Signal: REDUCE
- Current Signal: STRONG REDUCE
- Reduction: 20%

---

## Example 14 — No Available Capital

Input:
{
  "fear_greed": 8,
  "available_strategy_capital": 0,
  "current_position": 2000
}

Expected:
- Signal: STRONG BUY
- Action: NO EXECUTION

Reason:
FearCycle identifies Extreme Fear, but there is no available capital.

The agent must never invent capital.

---

## Example 15 — No Position During Reduce Signal

Input:
{
  "fear_greed": 85,
  "current_position": 0
}

Expected:
- Signal: STRONG REDUCE
- Action: NO EXECUTION

Reason:
There is no existing strategy position to reduce.

---

## Example 16 — Missing Data

Input:
{
  "fear_greed": null
}

Expected:
- Signal: NO TRADE
- Action: NO EXECUTION

Reason:
Current Fear & Greed data is unavailable.

The agent must never invent a value.

---

## Example 17 — Stale Data

Input:
{
  "fear_greed": 14,
  "data_timestamp": "2026-08-01T10:00:00Z",
  "current_timestamp": "2026-08-11T10:00:00Z"
}

Expected:
- Signal: NO TRADE
- Action: NO EXECUTION

Reason:
The available Fear & Greed data is stale.

---

## Example 18 — Historical Memory vs Current Data

Stored memory:
{
  "previous_fear_greed": 12,
  "previous_signal": "STRONG BUY",
  "previous_date": "2026-08-01"
}

Current data:
{
  "fear_greed": 48
}

Expected:
- Historical Fear & Greed: 12
- Current Fear & Greed: 48
- Current Zone: Neutral
- Current Signal: HOLD

Reason:
Historical memory is contextual information, not current market data.

---

## Example 19 — Current Data Overrides Memory

Stored memory:
{
  "previous_fear_greed": 9,
  "previous_signal": "STRONG BUY"
}

Current data:
{
  "fear_greed": 76
}

Expected:
- Previous Signal: STRONG BUY
- Current Zone: Greed
- Current Signal: REDUCE

Reason:
The agent must always prioritize current verified data.

---

## Example 20 — Spot Preference

User:
"Remember that I only trade Spot and don't use leverage."

Stored memory:
{
  "type": "user_preference",
  "subject": "trading",
  "content": {
    "market": "spot",
    "leverage": false
  },
  "importance": "high",
  "confidence": 1.0,
  "status": "active"
}

Later request:
"Evaluate FearCycle."

Expected:
- Market: Spot
- Leverage: None
- Strategy: FearCycle

The agent applies the stored preference.

---

## Example 21 — User Requests Leverage

Stored preference:
{
  "market": "spot",
  "leverage": false
}

User:
"Fear & Greed is 10. Use 10x leverage."

Expected:
- Signal: STRONG BUY
- Execution: REJECTED

Reason:
FearCycle is Spot-only and does not use leverage.

---

## Example 22 — Risk Limit Overrides Signal Allocation

Input:
{
  "fear_greed": 9,
  "available_strategy_capital": 5000,
  "normal_allocation": 0.30,
  "maximum_allowed_allocation": 0.10
}

Expected:
- Signal: STRONG BUY
- Normal Allocation: 30%
- Risk Limit: 10%
- Final Allocation: 10%
- Amount: $500

Reason:
The sentiment signal remains STRONG BUY, but portfolio risk limits override the normal allocation.

---

## Example 23 — Position Limit

Input:
{
  "fear_greed": 95,
  "current_position": 500,
  "normal_reduction": 0.30
}

Expected:
- Signal: STRONG REDUCE
- Reduction: $150

The agent must never reduce more than the current position.

---

## Example 24 — Extreme Fear Does Not Guarantee a Bottom

User:
"Fear & Greed is 5. Does that mean the market is definitely at the bottom?"

Expected response:

"No.

FearCycle treats 5 as Extreme Fear and generates a STRONG BUY signal, but the indicator does not guarantee a market bottom.

Extreme fear can continue or become stronger.

The strategy therefore uses gradual accumulation rather than attempting to predict the exact bottom."

---

## Example 25 — Extreme Greed Does Not Guarantee a Top

User:
"Fear & Greed is 95. Does that mean the market will crash now?"

Expected response:

"No.

FearCycle treats 95 as Extreme Greed and generates a STRONG REDUCE signal, but extreme greed does not guarantee an immediate correction.

The strategy reduces exposure gradually instead of attempting to predict the exact market top."

---

## Example 26 — Complete Evaluation

Input:
{
  "fear_greed": 17,
  "available_strategy_capital": 6000,
  "current_position": 2000,
  "days_since_last_execution": 10,
  "market": "spot",
  "leverage": 0
}

Processing:

1. Validate current Fear & Greed data.
2. Confirm Spot market.
3. Confirm no leverage.
4. Classify sentiment.
5. Check available capital.
6. Check current position.
7. Check rebalance interval.
8. Generate signal.
9. Calculate allocation.
10. Return decision.

Expected:
{
  "fear_greed": 17,
  "zone": "Extreme Fear",
  "signal": "STRONG BUY",
  "action": "ACCUMULATE",
  "suggested_allocation": 0.20,
  "suggested_amount": 1200,
  "market": "spot",
  "leverage": 0,
  "reason": "Market sentiment is in the Extreme Fear zone."
}

---

## Example 27 — Complete Sentiment Cycle

Stage 1:
Fear & Greed = 10
→ EXTREME FEAR
→ STRONG BUY
→ ACCUMULATE

Stage 2:
Fear & Greed = 28
→ FEAR
→ BUY
→ ACCUMULATE

Stage 3:
Fear & Greed = 50
→ NEUTRAL
→ HOLD
→ MAINTAIN

Stage 4:
Fear & Greed = 73
→ GREED
→ REDUCE
→ TAKE PARTIAL PROFIT

Stage 5:
Fear & Greed = 91
→ EXTREME GREED
→ STRONG REDUCE
→ REDUCE MORE

The strategy manages exposure throughout the sentiment cycle instead of trying to predict the exact top or bottom.

---

## Example 28 — Standard Agent Response

For a normal evaluation:

Fear & Greed: [current value]

Zone: [Extreme Fear / Fear / Neutral / Greed / Extreme Greed]

Signal: [STRONG BUY / BUY / HOLD / REDUCE / STRONG REDUCE]

Action: [ACCUMULATE / HOLD / REDUCE / NO TRADE]

Suggested Allocation/Reduction: [percentage]

Suggested Amount: [amount]

Reason: [short explanation]

Market: Spot

Leverage: None

Data Timestamp: [timestamp]

---

## Example 29 — Decision Tree

IF Fear & Greed data is unavailable:
    → NO TRADE

IF Fear & Greed data is stale:
    → NO TRADE

IF Fear & Greed <= 10:
    → STRONG BUY
    → Allocate up to 30%

ELSE IF Fear & Greed <= 20:
    → STRONG BUY
    → Allocate up to 20%

ELSE IF Fear & Greed <= 35:
    → BUY
    → Allocate up to 10%

ELSE IF Fear & Greed <= 64:
    → HOLD
    → No new sentiment-based position

ELSE IF Fear & Greed <= 79:
    → REDUCE
    → Reduce approximately 10%

ELSE IF Fear & Greed <= 89:
    → STRONG REDUCE
    → Reduce approximately 20%

ELSE:
    → STRONG REDUCE
    → Reduce approximately 30%

Always:
    → Spot only
    → No leverage
    → Respect available capital
    → Respect current position
    → Respect risk limits
    → Respect rebalance interval
    → Use current verified data
    → Never guarantee a market reversal
