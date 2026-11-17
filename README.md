# CS50 Finance — Stock Trading Simulator

A complete stock-trading web app: create an account, get **$10,000** in virtual cash, look up **real-time stock prices**, and buy and sell shares — with a live-valued portfolio and a full transaction history.

Built as the capstone web problem of **Harvard CS50x Week 9 (Flask)**, where it passed all **21/21** automated checks, and deployed here as a live web service.

## Features

- **Accounts** — register / log in / log out with hashed passwords; every portfolio is private to its owner.
- **Real quotes** — live prices by ticker symbol (via CS50's quote service, `finance.cs50.io`).
- **Buy & sell** — validated trades (real symbols, positive whole shares, sufficient cash/shares) executed against your cash balance.
- **Portfolio** — your holdings re-valued at current prices on every page load, with cash and grand total.
- **History** — every transaction (buys, sells) with price and timestamp.
- **Add cash** — a personal-touch extra beyond the course spec: top up your virtual balance.

## Tech stack

| Layer | Choice |
| --- | --- |
| Language | Python 3 |
| Framework | Flask 3 + Flask-Session (filesystem sessions) |
| Database | SQLite via the CS50 `SQL` library |
| Market data | `finance.cs50.io` quote API |
| Server | Gunicorn (deployment) |

## Run it locally

```bash
git clone https://github.com/hamza-474/cs50-finance.git
cd cs50-finance
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
flask --app app run
```

Open http://127.0.0.1:5000 and register. On first run the app creates `finance.db` and its tables automatically. Set a `SECRET_KEY` environment variable outside local development.

## Data model

- **users** — `id`, unique `username`, password `hash`, `cash` (starts at 10,000.00)
- **transactions** — `id`, `user_id` (FK), `symbol`, `shares` (+ buy / − sell), `price`, `type`, `transacted`

A portfolio is a view, not a table: holdings are computed from the signed sum of a user's transactions, valued at the latest quote.

## Origin & deployment notes

The original submission lives in the [cs50x-projects](https://github.com/hamza-474/cs50x-projects) repo (`week9/finance/`). This deployment copy differs only in operations, never in behaviour: `gunicorn` was added, the `users` table (which the course distribution shipped inside its database file) is created on first run, and the database file itself is created if missing — so the app boots cleanly on a fresh host such as Render.

## License

MIT — free to use, learn from, and build on.
