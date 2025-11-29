Automated Crypto Options Trading Bot – Delta Exchange
Overview
This repository contains a Python-based automated trading bot that integrates with the Delta Exchange REST API to trade crypto options, with a primary focus on Bitcoin 
 0DTE (zero days to expiry) options. The bot systematically monitors option premiums and manages positions according to rule-based intraday strategies.​

Key features:

Live market data fetch for BTC options from Delta Exchange.​

0DTE options selection logic for intraday strategies.​

Rule-based leg management when option premiums move by configurable thresholds (e.g., ±30%).

Basic risk and position management.​

Detailed trade logging and performance tracking (P&L, drawdowns, execution quality).​

Strategy Logic
At a high level, the bot:

Fetches the latest BTC options chain and filters for 0DTE contracts based on expiry.​

Enters an initial position (e.g., short strangle/straddle or directional spread, depending on configuration).​

Continuously monitors option premiums.

When a call or put premium moves beyond a configured percentage threshold (e.g., +30% from entry), the bot:

Exits the affected leg.

Re-enters the opposite leg according to strategy rules.

Logs all trades and key metrics for later analysis of P&L distribution, drawdowns, and overall strategy performance.​

The exact entry/exit rules, instruments, and thresholds are configurable via a settings file or environment variables.

Project Structure
config/ – API keys, environment settings, and strategy parameters (thresholds, product symbols, position sizes).​

src/data/ – Utilities for fetching live market data and option chains from the Delta Exchange API.​

src/strategy/ – Core strategy logic (0DTE selection, premium-based triggers, leg management).​

src/execution/ – Order placement, modification, and cancellation wrappers around the REST API.​

src/risk/ – Basic risk checks (max position size, max daily loss, max number of roll/adjustments).​

src/logs/ – Trade logs, error logs, and performance snapshots.​

notebooks/ – Optional research/backtesting notebooks for analyzing historical performance.​

(Adapt these folder names to match your actual repository.)

Requirements
Python 3.9+

Packages (typical):

requests or httpx for REST calls

pandas, numpy for data handling

python-dotenv or similar for config and secrets management

loguru or logging for structured logs​

Install dependencies (example):

bash
pip install -r requirements.txt
Setup
Clone the repository:

bash
git clone https://github.com/<your-username>/crypto-options-bot-delta.git
cd crypto-options-bot-delta
Create a .env or config file with your Delta Exchange API credentials:

text
DELTA_BASE_URL=https://api.delta.exchange
DELTA_API_KEY=your_api_key
DELTA_API_SECRET=your_api_secret
SYMBOL=BTC
PREMIUM_THRESHOLD=0.30
POSITION_SIZE=1
MAX_DAILY_LOSS=0.02
(Replace values with your own settings and preferred risk parameters.)​

Verify API connectivity:

bash
python -m src.data.health_check
This should print basic account and market data if the keys are correct.​

Running the Bot
To run the bot in paper-trading/simulation mode (recommended first):

bash
python -m src.main --mode paper
To run in live mode (real orders):

bash
python -m src.main --mode live
Command-line arguments (example):

--mode: paper or live

--config: path to a custom config file

--log-level: logging verbosity (INFO/DEBUG/WARN)​

Logs and Performance Analysis
The bot writes:

Trade logs (timestamp, instrument, side, size, price, P&L).

Strategy events (premium threshold hits, leg exits/entries, risk triggers).

Session summary (total P&L, max drawdown, number of adjustments).​

You can load these logs into a notebook or analytics script to:

Plot equity curves and drawdowns.

Analyze distribution of trade returns and win rate.

Compare different threshold parameters or position sizes.​

Risk Disclaimer
This project is for educational and research purposes only and does not constitute financial advice or a recommendation to trade. Crypto derivatives, including options on Bitcoin , are highly volatile and can result in substantial losses. Use at your own risk and test thoroughly in simulation before enabling live trading.
