# FearCycle

**FearCycle** is a simple long-term Spot trading strategy that uses the Crypto Fear & Greed Index as a market-sentiment signal.

The core idea is:

> Buy when the market is fearful and reduce exposure when the market becomes excessively greedy.

## Strategy

| Fear & Greed | Signal |
|---|---|
| 0–20 | Strong Buy |
| 21–35 | Buy |
| 36–64 | Hold |
| 65–79 | Reduce |
| 80–100 | Strong Sell |

The strategy is designed for **Spot trading and long-term positioning**.

## Core Logic

1. Read the current Fear & Greed Index.
2. Classify the market sentiment.
3. Check the current Spot position.
4. Generate a Buy, Hold, Reduce, or Sell signal.
5. Adjust the position gradually instead of making unnecessary frequent trades.

## Position Management

FearCycle uses gradual allocation rather than investing the entire capital at once.

Example:

- Extreme Fear → larger accumulation
- Fear → smaller accumulation
- Neutral → no new position
- Greed → partial profit taking
- Extreme Greed → stronger reduction

This allows the agent to treat sentiment as a long-term positioning signal rather than a short-term trading indicator.

## Market

- **Market:** Spot
- **Style:** Long-term
- **Leverage:** None
- **Primary Signal:** Fear & Greed Index

## Risk Notice

Fear & Greed is a sentiment indicator and does not guarantee future price movements.

Extreme fear can continue for a long time, and extreme greed can persist before a market reversal.

The strategy should therefore use gradual position sizing, avoid leverage, and maintain appropriate risk limits.

FearCycle is an experimental trading strategy and is not financial advice.
