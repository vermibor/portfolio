---
title: "Bitcoin Wheel Strategy"
date: 2026-09-10T19:30:00+02:00
draft: false
badges: ["Crypto", "API", "python", "weekend project", "algotrading"]
description: "Python-based paper trading engine and public dashboard to test four variations of the Wheel options strategy on Bitcoin."
---

For the past two months, a small script has been running on my old laptop. I wanted to answer a simple question that turned out to have a remarkably complicated implementation: *Does the classic Wheel options strategy actually hold up when applied to daily Bitcoin options?*

To find out without setting my own capital on fire first, I built a fully automated paper-trading engine connected to the Deribit Testnet, complete with a live public dashboard where anyone can watch the bots succeed—or fail—in real time. You can check out the live numbers right now at **[wheel.robak.org](https://wheel.robak.org)**.

Here is the story of why I built it, how the mechanics work under the hood, and why radical transparency (including publishing the raw decision logs) is the best part of the project.

---

### The Mechanics: Why the Wheel? Why Bitcoin?

If you come from traditional equity markets, you probably know **The Wheel**. It is one of the most popular income-generating option strategies because of its mechanical simplicity. It works in a continuous loop:

1. **Cash-Secured Puts:** You start with cash and sell Out-of-the-Money (OTM) Put options, collecting the premium. If the option expires worthless, you keep the premium and sell another Put.
2. **Assignment:** Eventually, the market drops below your strike price at expiration (the option finishes In-the-Money, or ITM). You get "assigned"—meaning you are forced to buy the underlying asset at the strike price.
3. **Covered Calls:** Now that you hold the underlying asset, you flip to the other side of the book. You start selling OTM Call options against your holdings, collecting premiums while you wait for the price to recover.
4. **Back to Cash:** Once the market rallies past your Call strike at expiration, your asset gets called away (sold), and you are back in cash, ready to sell Puts again.

In traditional finance, people run this on slow-moving dividend stocks or ETFs like SPY using monthly or weekly expirations. 

Bitcoin, however, is a completely different beast. Its volatility is famous for blowing up textbook strategies, which also means the option premiums are juicy. Furthermore, **Deribit** (the dominant crypto options exchange) offers daily expirations. Instead of waiting 30 days to see how a cycle plays out, you can compress the entire feedback loop into 24-hour cycles. 

I didn't just want to backtest this on historical CSV files. Backtests are notorious for hiding slippage, weird spread anomalies, and execution quirks. I wanted forward-testing in a live order book environment.

---

### The 10:15 AM Ritual: Architecture & Execution

On Deribit, daily options expire every day at **08:00 UTC**. During my builds, that translates to **10:00 AM local time**. 

Anyone who has traded around expiration knows that the 15 minutes immediately following 08:00 UTC are chaotic. Settlements are processing, margin requirements are recalculating, and market makers are widening spreads or resetting their quotes for the next cycle. 

To avoid getting caught in post-expiry noise, I scheduled my Python engine to wake up every day at exactly **10:15 AM**. Here is what happens during that daily run:

* **State Reconciliation:** First, the script hits the Deribit API to check what happened at the 10:00 AM expiry. Did our Put expire worthless, or did we effectively "take delivery" of BTC? (Since Deribit options are cash-settled in crypto, the engine simulates the exact spot transition required to maintain a pure Wheel state).
* **Market Snapshot & Risk Modeling:** Next, it pulls the live option chain, order book depth, current implied volatility (IV), and Greeks (Delta, Gamma, Theta). 
* **Evaluating Four Independent Strategies:** Rather than betting on a single set of parameters, I realized early on that "the Wheel" isn't one strategy—it depends entirely on how aggressive you are with your strikes and risk filters. So, the engine runs **four distinct strategy profiles** in parallel. Some hunt for higher premiums closer to the money; others play it deeply conservative or pause entirely when volatility regimes shift out of bounds.
* **Simulated Execution:** Once each of the four models calculates its risk scores and selects an optimal strike, the script executes the simulated trades against the Deribit Testnet environment and records the fill prices.

---

### Beyond Basic PnL: The Dashboard at wheel.robak.org

A Python script logging trades to a database is useful for me, but it isn't very fun to look at. I wanted a single pane of glass where I could track performance day by day, compare the four strategy variants side-by-side, and share the progress publicly. That led to **[wheel.robak.org](https://wheel.robak.org)**. Adding more is just a simple edit of json file, but for now 4 startegies is enough.

After the 10:15 AM execution finishes, the backend crunches the daily numbers, updates the historical series, and pushes the fresh data to the dashboard. 

Tracking options strategies requires more than a simple "profit/loss" line, especially when you are cycling between holding cash and holding a volatile asset like Bitcoin. On the site, I set up tracking for:

* **Granular Trade History:** Every single Put and Call sold across all four strategies, including strike prices, premiums collected, and settlement outcomes.
* **Annualized Metrics (CAGR):** Projecting how the compound returns look on an annualized basis so they can be compared fairly against a simple "buy and hold BTC" benchmark.
* **Max Drawdown & Equity Curves:** Because selling options often looks like a smooth upward line right up until a sharp market crash, tracking capital drawdowns (obsunięcie kapitału) is critical. If a strategy makes 2% a week but suffers a 40% drawdown during a Bitcoin flush, the risk-adjusted return is terrible.

---

### What's Next?

Right now, the project is strictly in the **paper trading** phase. The goal is to let these four strategies run untouched through different market regimes—sideways chop, violent pumps, and ugly liquidations—to see which parameterization survives with the cleanest drawdown profile.

Quantitative trading is a humble game; what looks brilliant in a two-month bull market often breaks when volatility dries up or spikes too hard. By running this live every day at 10:15 AM and publishing every trade and log entry out in the open, there is nowhere for bad assumptions to hide.

If you are into crypto derivatives, algorithmic trading, or just want to see if a Python script can systematically harvest volatility on Bitcoin, take a look at **[wheel.robak.org](https://wheel.robak.org)**. I’d love to hear your thoughts—especially if you've experimented with automated options strategies yourself.
