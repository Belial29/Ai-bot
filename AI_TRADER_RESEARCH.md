# "AI trader" GitHub repos for day trading a Lucid account with TradingView

Researched 2026-10-07. Search used: GitHub repositories matching `Ai trader`,
sorted by stars (5,598 results). These are the top results, checked against one
setup: **day trading futures on a Lucid Trading prop account, with TradingView
charts and alerts.**

## Bottom line

**None of the top-starred "AI trader" repos can place trades on a Lucid account.**
They target crypto exchanges, Robinhood, Chinese A-shares or Hyperliquid. Lucid
trades CME futures (ES/NQ/MES/MNQ, etc.) through Tradovate or Rithmic.

There is a second problem. Lucid's prop accounts on Tradovate **do not get direct
Tradovate API access**. So most do-it-yourself "Tradovate bot" repos (which log
in with a personal API key) will not reach a Lucid account either.

What does work with Lucid and TradingView:

```
TradingView Pine Script strategy
        │  (alert → webhook)
        ▼
Authorized bridge: TradersPost / PickMyTrade / CrossTrade (via NinjaTrader)
        │
        ▼
Lucid account (Tradovate or Rithmic)
```

AI is most useful for writing and refining the Pine Script strategy and for
backtesting it. Execution should go through one of those bridges.

## Top repos checked

| # | Repo | Stars | What it is | Fit for Lucid + TradingView |
|---|------|------:|-----------|-----------------------------|
| 1 | [HKUDS/AI-Trader](https://github.com/HKUDS/AI-Trader) | 22.7k | Social/"agent-native" signal and copy-trading platform (ai4trade.ai). Paper trading, Polymarket. Syncs with Binance, Coinbase, IBKR. | ❌ No Tradovate/Rithmic/TradingView execution. Your agent signs up with a third-party hosted service. |
| 2 | [charliedream1/ai_quant_trade](https://github.com/charliedream1/ai_quant_trade) | 6.6k | Learning collection: ML/DL/RL, LLM stock forecasting, JoinQuant (China) code. | ❌ For study only. No US futures execution. |
| 3 | [jarrodwatts/jev-trader](https://github.com/jarrodwatts/jev-trader) | 2.9k | One AI trade per Monad block, MON-USDC on-chain. | ❌ Crypto only. |
| 4 | [whchien/ai-trader](https://github.com/whchien/ai-trader) | 1.1k | Backtrader backtesting framework, 20+ strategies, CLI and **MCP server for Claude**. | ⚠️ **Backtesting only.** You can load your own ES/NQ CSV data and test ideas. No live trading. GPL-3.0. |
| 5 | [garagesteve1155/PowerTrader_AI](https://github.com/garagesteve1155/PowerTrader_AI) | 814 | Crypto price prediction plus tiered DCA. | ❌ DCA (adding to losers) breaks prop drawdown rules. |
| 6 | [YoungCan-Wang/WyckoffTradingAgent](https://github.com/YoungCan-Wang/WyckoffTradingAgent) | 740 | Wyckoff screener for A-shares. | ❌ China stocks. |
| 7 | [dromara/northstar](https://github.com/dromara/northstar) | 593 | Java quant platform for China CTP futures. | ❌ Chinese futures only. |
| 8 | [jason8745/llm-agent-trader](https://github.com/jason8745/llm-agent-trader) | 492 | LLM-assisted stock backtester (FastAPI + Next.js). | ❌ Stocks, backtest only. |
| 9 | [Superior-Trade/trading-terminal](https://github.com/Superior-Trade/trading-terminal) | 412 | Draw on a TradingView chart; the AI writes a Freqtrade strategy for Hyperliquid. | ❌ Hyperliquid crypto only. A good idea, wrong market. |
| 10 | [rnikitin/QuantGPT](https://github.com/rnikitin/QuantGPT) | 229 | Chat copilot for quant research (crypto). | ❌ |
| 11 | [YizhiSong/FriesTrader](https://github.com/YizhiSong/FriesTrader) | 209 | Claude Code + Robinhood Agentic Trading MCP. Two sessions a day, strong mechanical risk rules. | ❌ Robinhood stocks, swing trading. Its risk-rule design is worth copying. |
| 12 | [qrak/LLM_trader](https://github.com/qrak/LLM_trader) | 131 | Vision-LLM chart reading, memory and reflection. Crypto, paper trading by default. | ❌ Crypto. Interesting ideas only. |
| – | [longsurf-ai/openchart](https://github.com/longsurf-ai/openchart) | 90 | Open-source TradingView-style app with your own AI agent (Claude/Codex/Gemini). | ⚠️ Analysis and alerts only, not execution. |

Others in the list (ORSTAC = Deriv binary options, Polymarket tools, MT5 gold
bot, Binance futures tools) don't apply.

### Code check: HKUDS/AI-Trader (most stars)

I read the code at commit `d03ff6c` (2026-06-11), not just the README:

- **No broker execution at all.** `service/requirements.txt` has no broker SDK
  (no ccxt, ib_insync, alpaca, Tradovate or Rithmic). "binance" and "coinbase"
  appear only as aliases for the `crypto` market (`service/server/routes_shared.py`).
- **Markets in code:** only `us-stock`, `crypto` and `polymarket`
  (`service/server/price_fetcher.py`, `routes_shared.py`). "Futures" appears only
  in one README marketing line. The code has no futures, Tradovate, Rithmic,
  TradingView, webhook or CME symbols.
- **What "trade sync" and "copy trading" do:** your agent registers at
  `api.ai4trade.ai` and uploads positions and trades so followers can see them.
  Copy trading mirrors **paper** positions inside that platform.
- **What it's good for:** joining an AI-agent trading community, $100K paper
  trading on stocks and crypto, and seeing what other agents post. It can't
  trade a Lucid account. Its signals are for stocks and crypto, not NQ/ES futures.

### Code check: charliedream1/ai_quant_trade

Commit `b612402` (2026-10-05). This is a learning collection of examples
(`egs_*` folders), not a trading app.

- "Live trading" means a **simulation** through Wind, a Chinese market
  terminal (`egs_trade/paper_trade/wind`), and JoinQuant strategies for
  Chinese A-shares (`egs_online_platform/聚宽_JoinQuant`).
- The futures example (`egs_data/期货/futures`) loads **Chinese** contracts
  (rebar, copper, PVC) from AKShare/Sina. No CME futures.
- No Tradovate, Rithmic, NinjaTrader or TradingView code anywhere.

### Code check: jarrodwatts/jev-trader

Commit `b587759` (2026-09-16). A TypeScript bot that places a post-only limit
order on the Kuru MON-USDC order book (Monad blockchain) **every ~300 ms
block**. It's a crypto market-making / high-frequency bot. That style is
**banned** at Lucid (HFT and microscalping), and it can't connect to futures.

### The three compared

| Goal | HKUDS/AI-Trader | ai_quant_trade | jev-trader |
|------|-----------------|----------------|------------|
| Trades your Lucid account | ❌ No broker code | ❌ China markets, simulation | ❌ Crypto DEX, and HFT is banned at Lucid |
| Reads your screenshots and suggests what to do | ❌ | ❌ | ❌ |
| Paper-trade practice | ✅ $100K paper (stocks/crypto) | ⚠️ Wind sim (China) | ⚠️ Dry run (crypto) |
| Learning material | ⚠️ | ✅ Large tutorial set | ⚠️ Clean code example |

None of the three accept screenshots. A text search for vision/image features
found only false positives.

**Verdict:** None of them fit. If you had to pick one, AI-Trader comes closest:
you can plug an AI agent into it and paper-trade for free. But it uses stocks
and crypto, not your futures account. For either goal, use what's below instead.

- **"Send it screenshots, it tells me what to do":** Claude already does this.
  Send a TradingView screenshot in the Claude app with your timeframe, account
  rules and setup criteria, and ask for a structured read: trend, key levels,
  setup yes/no, entry, stop, target, invalidation. Treat it as a second opinion.
  A single screenshot doesn't show live price, order flow or news.
- **"Trade for me":** a Pine Script strategy → TradingView webhook alert →
  TradersPost / PickMyTrade / CrossTrade → Lucid (see the setup above).

### Futures/Tradovate-specific repos (separate search)

| Repo | Stars | Note |
|------|------:|------|
| [dearvn/tradovate-trading-bot](https://github.com/dearvn/tradovate-trading-bot) | 63 | Uses the Tradovate API, so it **won't reach a Lucid prop account** (no API access). |
| [prashanthaitha24/nq-strategy-b-bot](https://github.com/prashanthaitha24/nq-strategy-b-bot) | 10 | NQ ICT/FVG strategy. Useful as a strategy and backtest reference. Same API limit. |
| [jayadevrana/tradovate-tradingview-webhook-bridge](https://github.com/jayadevrana/tradovate-tradingview-webhook-bridge) | 0 | DIY TradingView→Tradovate bridge. Same API limit. |

## Lucid rules a bot must respect

Source: third-party summaries of Lucid's help center, as of 2026. Confirm on
support.lucidtrading.com before going live.

- Automated strategies and trade copiers are **allowed** on evaluation and
  funded accounts.
- **HFT is banned.** **Hedging across accounts is banned.**
- **Microscalping is flagged** when more than 50% of profit comes from trades held
  **5 seconds or less**. Penalties go from a warning to forfeited profits and a ban.
- **You are responsible** for any bot error or malfunction.
- Consistency rules depend on the plan: LucidPro ≤40%, LucidDirect 20%, LucidFlex
  funded has no consistency rule.

## Recommended setup

1. **Strategy:** Write it in Pine Script v6 on TradingView. Have AI (Claude)
   draft and refine it. Backtest in TradingView's Strategy Tester with real
   commission and 1–2 ticks of slippage. You can also re-test offline with
   `whchien/ai-trader` (Claude can drive it through its MCP server).
2. **Built-in guardrails:** a daily loss stop below Lucid's limit, max contracts
   within the plan limit, flat before close and around major news, a minimum
   hold time well over 5s, and one account per direction.
3. **Execution:** TradingView alert → TradersPost or PickMyTrade (Tradovate
   route) or CrossTrade (NinjaTrader route) → Lucid account.
4. **Prove it first:** Run it on a Lucid evaluation or sim account for several
   weeks before a funded account.

GitHub stars measure popularity, not profitability. None of these repos has a
verified live futures track record.

## Sources

- https://github.com/search?q=Ai+trader&type=repositories&s=stars&o=desc
- https://crosstrade.io/learn/prop-firm-ai-trading/lucid-trading
- https://proptradingvibes.com/blog/lucid-trading-tradingview
- https://proptradingvibes.com/blog/lucid-trading-prohibited-trading-strategies
- https://daytradingz.com/lucid-trading-review/
- https://proea.app/en/blog/lucid-trading-rules
- https://pickmytrade.trade/debugging/prop-accounts-block-api-automation/
- https://community.tradovate.com/t/api-access-for-propfirm-accounts/10348
- https://blog.traderspost.io/article/lucid-trading-review-2025
