# coingecko-bot
A bot that utilizes coingecko's API to display information such as trending tokens to top performing sectors. 
# CoinGecko Bot

![Discord](https://img.shields.io/badge/Discord-bot-blue) ![Python](https://img.shields.io/badge/Python-requests-green)

A simple Python application that fetches CoinGecko’s top trending coins and posts them to your Discord channel via a webhook on a schedule (e.g., GitHub Actions).

---

## Table of Contents

1. [Features](#features)
2. [Prerequisites](#prerequisites)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Usage](#usage)
6. [Scheduling](#scheduling)
7. [Customization](#customization)
8. [Contributing](#contributing)
9. [License](#license)

---

## Features

* Fetches the **top 7 trending** coins from CoinGecko’s `/search/trending` endpoint
* Builds a Discord embed listing each coin’s name, symbol, market cap rank, and link
* Posts the embed to a Discord channel via a **webhook URL**
* Easily schedulable via cron, GitHub Actions, AWS Lambda, Heroku Scheduler, etc.

---

## Prerequisites

* Python 3.7+
* A Discord server with **Webhook** permissions
* (Optional) GitHub account if you plan to use **GitHub Actions**

---

## Installation

1. **Clone** this repository:

   ```bash
   git clone https://github.com/<YOUR_USERNAME>/discord-gecko-trending.git
   cd discord-gecko-trending
   ```
2. **Create** (or activate) a virtual environment:

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # on Windows: venv\Scripts\activate
   ```
3. **Install** dependencies:

   ```bash
   pip install -r requirements.txt
   ```

---

## Configuration

1. Go to your Discord server settings → **Integrations** → **Webhooks** → **New Webhook**
2. Choose a channel, give it a name (e.g. `gecko-trending-bot`), and **Copy Webhook URL**
3. Export the URL as an environment variable:

   ```bash
   export DISCORD_WEBHOOK="https://discord.com/api/webhooks/XXX/YYY"
   ```

---

## Usage

Run the script manually:

```bash
python gecko_trending.py
```

You should see a confirmation in your Discord channel within a few seconds.

---

## Scheduling

### GitHub Actions

Use the provided workflow in `.github/workflows/trending.yml`:

```yaml
on:
  schedule:
    - cron: '0 * * * *'  # runs every hour
  workflow_dispatch:     # allows manual triggers
jobs:
  post-trending:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      - run: pip install -r requirements.txt
      - run: python gecko_trending.py
        env:
          DISCORD_WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
```

> Don’t forget to add your `DISCORD_WEBHOOK` as a **GitHub Actions secret**!

### Other Options

* **Cron job** on a Linux server
* **Heroku Scheduler** add-on\ n- **AWS Lambda** + **EventBridge**
* **Google Cloud Functions** + **Scheduler**

---

## Customization

* **Embed color**: change the `color` field in `build_embed` (hex).
* **Number of coins**: slice the returned list (e.g. `coins[:5]` for top 5).
* **Additional fields**: include `price_btc`, `thumb`, or `market_cap` from the API response.

---

## Contributing

Contributions are welcome! Please:

1. Fork this repo
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add awesome feature"`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Contact

Created by [NullNoob](https://github.com/NullNoob). Questions or feedback? Open an issue or ping me on Discord.
