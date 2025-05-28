---
title: "Kline/Candlestick Stream"
weight: 2
---

_Date Created: 2025-05-27_

# Mark Price Stream

Stream Name is `<symbol>@markPrice@1s`. It is defaulted to be 3s o/w 1s.

## Response Example

```json
{
  "e": "markPriceUpdate", // Event type
  "E": 1562305380000, // Event time
  "s": "BTCUSDT", // Symbol
  "p": "11794.15000000", // Mark price
  "i": "11784.62659091", // Index price
  "P": "11784.25641265", // Estimated Settle Price, only useful in the last hour before the settlement starts
  "r": "0.00038167", // Funding rate
  "T": 1562306400000 // Next funding time
}
```

# Kline/Candlestick Stream

Binance provides channel for different Stream Name `<symbol>@kline_<interval>`, updated speed is `250ms`.

Reference: https://developers.binance.com/docs/derivatives/usds-margined-futures/websocket-market-streams/Kline-Candlestick-Streams

## Response Example

```json
{
  "e": "kline", // Event type
  "E": 1638747660000, // Event time
  "s": "BTCUSDT", // Symbol
  "k": {
    "t": 1638747660000, // Kline start time
    "T": 1638747719999, // Kline close time
    "s": "BTCUSDT", // Symbol
    "i": "1m", // Interval
    "f": 100, // First trade ID
    "L": 200, // Last trade ID
    "o": "0.0010", // Open price
    "c": "0.0020", // Close price
    "h": "0.0025", // High price
    "l": "0.0015", // Low price
    "v": "1000", // Base asset volume
    "n": 100, // Number of trades
    "x": false, // Is this kline closed?
    "q": "1.0000", // Quote asset volume
    "V": "500", // Taker buy base asset volume
    "Q": "0.500", // Taker buy quote asset volume
    "B": "123456" // Ignore
  }
}
```

I will show `Close price` and `Base asset volume` in the stat component in my own app.
