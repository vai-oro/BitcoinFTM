# BitcoinFTM

A Bitcoin arbitrage trading platform built in PHP that aggregates real-time data from multiple cryptocurrency exchanges, identifies arbitrage opportunities, and provides visualization tools for trading analysis.

> **Note:** This project was created in 2013 and last updated in 2014. While historically interesting, many of the exchanges it connects to (MtGox, BTCE, Campbx) are no longer operational.

## Overview

BitcoinFTM monitors Bitcoin prices across multiple exchanges in real-time, calculates arbitrage opportunities, and provides tools for manual and automated trading execution. The platform features:

- Real-time price aggregation from 7+ exchanges
- Arbitrage opportunity detection and ranking
- Interactive charts and orderbook visualization
- Transaction history tracking
- Leaderboard for top-performing strategies

## Architecture

### Core Components

```
BitcoinFTM/
├── btcftm/                 # Main application directory
│   ├── ajax/               # AJAX endpoints for real-time updates
│   ├── core/               # Core trading logic
│   ├── css/                # Stylesheets
│   ├── docs/               # Documentation
│   ├── images/             # Static assets
│   ├── js/                 # JavaScript utilities
│   ├── json/               # Cached API responses
│   ├── partials/           # Reusable UI components
│   ├── index.php           # Main application entry
│   ├── controls.php        # Trading controls dashboard
│   ├── ajax-arbitrage.php  # Arbitrage API endpoint
│   ├── bitcoinchart.php    # Chart rendering endpoint
│   ├── full-matrix.php     # Exchange comparison view
│   └── ...
├── classes/                # Exchange adapter classes
│   ├── orderbook.php       # Base orderbook class
│   ├── ticker.php          # Base ticker class
│   ├── bitfinex*.php       # Bitfinex exchange adapter
│   ├── bitstamp*.php       # Bitstamp exchange adapter
│   ├── btce*.php           # BTCE exchange adapter (defunct)
│   ├── campbx*.php         # Campbx exchange adapter (defunct)
│   ├── kraken*.php         # Kraken exchange adapter
│   ├── mtgox*.php          # MtGox exchange adapter (defunct)
│   └── cryptotrade*.php    # CryptoTrade exchange adapter
├── cron/                   # Scheduled data collection tasks
│   ├── add-live-data.php   # Fetch ticker data
│   ├── add-orderbook-data.php  # Fetch orderbook snapshots
│   ├── resample-live-data.php  # Data aggregation/resampling
│   └── tasks/              # Cron job configurations
├── utils/                  # Utility classes
│   ├── database_util.php   # Database connection helpers
│   └── ExchangeDbUtil.php  # Exchange data persistence
├── archive/                # Historical data snapshots
└── phpinfo.php             # PHP configuration viewer
```

### Supported Exchanges

| Exchange | Status | Notes |
|----------|--------|-------|
| **MtGox** | ❌ Defunct | Collapsed 2014 |
| **BTCE** | ❌ Defunct | Shut down 2016 |
| **Campbx** | ❌ Defunct | Ceased operations 2014 |
| **CryptoTrade** | ❌ Defunct | Exited market |
| **Bitfinex** | ✅ Active | Still trading |
| **Bitstamp** | ✅ Active | Still trading |
| **Kraken** | ✅ Active | Still trading |

## Features

### Real-Time Monitoring
- Live ticker data from all connected exchanges
- Orderbook depth tracking
- Automatic price updates via AJAX

### Arbitrage Detection
- `ajax-arbitrage.php` - Returns current arbitrage opportunities
- `best-ops.php` - Ranks exchanges by spread
- Leaderboard tracking top arbitrageurs

### Visualization
- `bitcoinchart.php` - Interactive price charts
- Orderbook visualization
- Transaction history graphs
- Historical data plotting

### Trading Controls
- `controls.php` - Main dashboard
- `ajax-market-buy.php` / `ajax-market-buysell.php` - Market order execution
- `ajax-transfer.php` - Internal fund transfers
- `run-arbitrage.php` - Automated arbitrage execution

### Data Management
- `cron/add-live-data.php` - Fetches ticker data every interval
- `cron/add-orderbook-data.php` - Captures orderbook snapshots
- `cron/resample-live-data.php` - Resamples data for different timeframes
- JSON cache storage for performance

## Setup & Configuration

### Prerequisites
- PHP 5.x (likely 5.3+ based on 2013 timeframe)
- MySQL database
- Apache/Nginx web server
- Cron job support
- jQuery (included in `/btcftm/jquery/`)

### Configuration
1. Copy `btcftm/config.php` and configure database credentials
2. Set up MySQL database for storing ticker and orderbook data
3. Configure cron jobs to run data collection scripts
4. Ensure web server can access the application directory

### Cron Jobs Example
```bash
# Fetch ticker data every 30 seconds
*/30 * * * * php /path/to/BitcoinFTM/cron/add-live-data.php

# Fetch orderbook data every minute
* * * * * php /path/to/BitcoinFTM/cron/add-orderbook-data.php

# Resample and aggregate data hourly
0 * * * * php /path/to/BitcoinFTM/cron/resample-live-data.php
```

## API Endpoints

### AJAX Endpoints
- `GET /btcftm/ajax-arbitrage.php` - Returns arbitrage opportunities
- `GET /btcftm/ajax-market-buy.php` - Execute market buy order
- `GET /btcftm/ajax-orderbooks.php` - Current orderbooks
- `GET /btcftm/ajax-transfer.php` - Transfer funds between accounts

### Data Endpoints
- `GET /btcftm/full-markets.php` - Full market overview
- `GET /btcftm/show-leaderboard.php` - Top arbitrageurs
- `GET /btcftm/show-transactions.php` - Transaction history
- `GET /btcftm/master-json.php` - Raw JSON data export

### Utility Endpoints
- `GET /btcftm/test-arbitrage.php` - Test arbitrage calculation
- `GET /btcftm/test-chart.php` - Chart data test
- `GET /btcftm/test-sma.php` - SMA calculation test
- `GET /btcftm/dump.php` - Debug data dump

## Data Storage

The application stores:
- **Ticker data**: Bid/ask prices, volume, timestamp
- **Orderbook snapshots**: Full depth snapshots for analysis
- **Transaction history**: All executed trades
- **User rankings**: Arbitrage profitability tracking

Database schema likely includes:
- `btcftm_ticker` - Price feed data
- `btcftm_orderbook` - Orderbook snapshots
- `btcftm_transactions` - Trade history
- `btcftm_users` - User accounts
- `btcftm_leaderboard` - Rankings

## Development Notes

### Code Style
- Object-oriented with base classes for exchanges
- jQuery for frontend interactions
- AJAX for real-time updates
- MySQL for persistence

### Key Design Patterns
- **Adapter Pattern**: Exchange classes implement common interface
- **Repository Pattern**: `ExchangeDbUtil.php` handles persistence
- **Observer Pattern**: Real-time updates via AJAX polling

### Extensibility
To add a new exchange:
1. Create class in `/classes/` extending `ticker.php` and `orderbook.php`
2. Implement exchange-specific API calls
3. Add cron job for data collection
4. Update `index.php` to include new exchange

## Historical Context

This project was built during Bitcoin's "gold rush" era (2013-2014):
- Multiple arbitrage opportunities existed due to market fragmentation
- MtGox dominated volume (~70% of global Bitcoin trading)
- Many small exchanges with limited liquidity
- No major regulation, minimal KYC requirements

### Why It Matters
- Early example of algorithmic crypto trading
- Shows the state of crypto infrastructure in 2013
- Demonstrates arbitrage as a fundamental market mechanism
- Historical record of defunct exchanges

## Current Relevance

While the project is largely historical now:
- **Educational Value**: Excellent reference for building exchange aggregators
- **Architecture**: Clean separation of concerns, modular design
- **Legacy**: Many concepts still used in modern trading platforms
- **Exchanges**: Bitfinex, Bitstamp, and Kraken remain active

## License

Unknown - original license not specified in repository.

## Acknowledgments

Built for the early Bitcoin ecosystem. Many exchanges have since consolidated, but the core concepts of arbitrage trading and price discovery remain unchanged.

---

**Repository Stats:**
- Created: October 27, 2013
- Last pushed: March 23, 2014
- Language: JavaScript (68%), PHP (32%)
- Size: ~21 MB
- Stars: 1
- Forks: 0
# Additional Notes
