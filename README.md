## Arb Protocol: V2

![GitHub last commit](https://img.shields.io/github/last-commit/ARBProtocol/ARB-V2) ![GitHub issues](https://img.shields.io/github/issues/ARBProtocol/ARB-V2) ![GitHub number of milestones](https://img.shields.io/github/milestones/all/ARBProtocol/ARB-V2) ![GitHub stars](https://img.shields.io/github/stars/ARBProtocol/ARB-V2?style=social)
[![Twitter Follow](https://img.shields.io/twitter/follow/ArbProtocol?style=social)](https://twitter.com/YourTwitterHandle)
[![Discord](https://img.shields.io/discord/985095351293845514?logo=discord&logoColor=white&style=flat-square)](https://discord.gg/wcxYzfKNaE)

ARB Protocol V2 is an advanced, open-source arbitrage bot designed to operate on the Solana blockchain. It monitors multiple decentralized exchanges (DEXs) on Solana to identify and execute profitable arbitrage opportunities. This bot runs locally on your machine, ensuring full control over your trading strategy and data.

Use of this bot/script is at your own risk. Arbitrage trading can lead to significant losses if not properly configured or monitored. Please exercise proper due diligence and DYOR before using.

## Table of Contents

- [Features](#features-)
- [Installation](#installation-)
- [Usage](#usage-)
- [Setup](#setup-%EF%B8%8F)
- [Env.json](#envjson-)
- [Optimization Tips](#optimization-tips-)
- [Extra Information](#extra-information)

## Features ✨

- **Multi-DEX Monitoring:** Tracks price differences across major Solana DEXs like Meteora, Raydium, and Orca.
- **Risk Reduction** Through our On-Chain HARBR Program, we help mitigate losses. This program is used to double check your arbitrage trade, and fails if you were to make a loss, protecting your funds.
- **Local Operation:** Runs on your own machine or VPS for enhanced security and control.
- **Customizable Strategies:** Set your own parameters for minimum profit thresholds, trade sizes, and risk management.
- **Lightning Fast Execution:** Written in Rust. Iterating through options/combinations hundreds of times per second.
- **Detailed Logging:** Keeps comprehensive logs of all trades, profits, and errors for analysis and optimization.
- **HARBR:** [https://solscan.io/account/HARBRqBp3GL6BzN5CoSFnKVQMpGah4mkBCDFLxigGARB](https://solscan.io/account/HARBRqBp3GL6BzN5CoSFnKVQMpGah4mkBCDFLxigGARB)

## Installation 🔧
Navigate to this Github page and select the most recent version for your operating system:
https://github.com/ARBProtocol/ARB-V2/releases

It should come bundled with all the necessary files you need inside.

## Usage 🚀

-  **Initial Setup:**  Once downloaded, you'll be greeted with ```config.json``` and ```env.json``` files.
- **config.json**: This is where the main bulk of settings/parameters are. Here is where you will tweak for performance.
- **env.json**: This is where your personal data is kept. *Keep this private, and NEVER share it!* (Private Key, RPC connection etc)

## Setup ⚙️

<details>
<summary>Settings Explainer</summary>

```bash
{
  "meta": {
    "price_fetch_delay": 0,
    "after_order_delay": 0,
    "simulate": false,
    "jito_tip_percent": 50,
    "max_jito_tip_lamports": 5000000,
    "slippage": {
      "mode": "exact",
      "bps": 2500,
      "inner_bps": 500
    },
    "wrap_mode": "all"
  },
  "tokens": [
    {
      "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
      "name": "USDC",
      "send_mode": "both",
      "decimals": 6,
      "max_accounts": 25,
      "sizes": [
        {
          "min_amt": 400,
          "max_amt": 400,
          "trade_size_decimals": 0,
          "min_profit_onchain": 0,
          "min_profit_to_send": 3
        }
      ],
      "prio_fee": 30000,
      "cu_limit": 250000
    }
  ],
  "mids": {
        "top_amt": 0 ,
        "prune_percentage": -50,
        "prune_readd_amt": 2,
        "custom": [
            {
              "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
              "name": "USDC",
              "decimals": 6
            },
            {
              "mint": "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
              "name": "USDT",
              "decimals": 6
            },
            {
              "mint": "3psH1Mj1f7yUfaD5gh6Zj7epE8hhrMkMETgv5TshQA4o",
              "name": "boden",
              "decimals": 9
            }
        ]
    }
}
```
## Fields

-   `meta`  (object): Meta settings for the bot.
    -   `price_fetch_delay`  (number): Delay between price fetches in milliseconds.
    -   `after_order_delay`  (number): Delay after an order is placed in milliseconds.
    -   `simulate`  (boolean): Whether to simulate trades.
    -   `slippage`  (object): Slippage settings.
        -   `mode`  (string): Slippage mode (One of “exact” or “profitbps” - exact is an exact slippage, where profitbps is a bps amount of the pair’s profit, ex: 50bps of a profit of 10USDC would be 0.05 USDC slippage).
        -   `bps`  (number): Base slippage in basis points.
        -   `inner_bps`  (number): Inner slippage in basis points - used in requests when fetching quotes.
    -   `jito_tip_percent`  (object): The amount of profit to tip as a percentage to Jito (only when using them, of course).
    -   `max_jito_tip_lamports`  (number): The maximum amount of lamports to tip Jito. If the tip is higher than this, it will be capped at this amount.
    -   `wrap_mode`  (string): Wrap mode (One of “all” or “none” - all wraps and unwraps your SOL, and none does not wrap or unwrap, using pre-wrapped WSOL).
-   `tokens`  (array): Array of token setting objects.
    -   `enable`  (boolean, optional): Whether to enable the token. Defaults to true.
    -   `mint`  (string): Mint address of the token.
    -   `send_mode`  (string): One of “rpc”, “jito”, “both”.
    -   `name`  (string): Name of the token.
    -   `decimals`  (number): Decimals of the token.
    -   `max_accounts`  (number): Max accounts to trade with on each side of the ARB. 25 is reccomended, as Solana has a max accounts per tx limit of 60 - 25*2=50 - leaving 10 accounts of slack for the profit checker, etc.
    -   `sizes`  (array): Array of size objects.
        -   `min_amt`  (number): Minimum amount to trade.
        -   `max_amt`  (number): Maximum amount to trade.
        -   `trade_size_decimals`  (number): Decimals of the trade size (ex: min 1, max 2, decimals 1: 1, 1.1, 1.2 … 1.9, 2. decimals 2: 1, 1.01, 1.02 … 1.99, 2. etc…)
        -   `min_profit_onchain`  (number, optional): Minimum  **BPS ONLY**  profit to trade on-chain. This reverts trades that do not make  _enough_  profit on-chain. Set this to 0 or do not specify it to disable, and take any profit (Reccomended!).
        -   `min_profit_to_send`  (number): Minimum profit to send. Use a whole number value for BPS, and a decimal value for token amounts (ex: 3 for 3bps minimum, 0.1 for 0.1 tokens).
    -   `prio_fee`  (number): Priority fee in lamports.
    -   `cu_limit`  (number): Compute unit limit on the transaction.
-   `mids`  (object): Configuration for the mids (token B in the A -> B -> A system)
    -   `top_amt`  (number): DEPRECATED!!! Set to 0. Will be removed next update.
    -   `prune_percentage`  (number): Any route who quotes a loss of more than this percentage gets pruned.
    -   `prune_readd_amt`  (number): Every “cycle” (1/(route count * 2) chance every route check), this many pruned routes get added back into the mix
    -   `custom`  (array): Array of custom token mids, deduped with the ones added by to_amt - reccomended to put high volatility tokens you know of with multiple pools (the list is autodeduped if you use the mid getting too)
        -   `mint`  (string): Mint address of the token.
        -   `name`  (string): Name of the token.
        -   `decimals`  (number): Decimals of the token.
</details>


## Env.json 📝

<details>
<summary>Env Explainer</summary>

env.json
```
{
"keypair": "",
"jup": "",
"rpc": "",
"send_rpcs": [""]
}
```

## Fields

-   `keypair`  (string): The private key of the wallet you want to use.
-   `jup`  (string): A Jupiter API URL.
-   `rpc`  (string): A Solana RPC URL.
-   `send_rpcs`  (list of strings): The URLs of the RPCs you want to send transactions to.

## Extra fields (Only use if you know what you’re doing!)

-   `block_engines`  (list of strings, optional): The URL of the Jito block engine you want to use, with the bundles path. Defaults to  `https://mainnet.block-engine.jito.wtf/api/v1/bundles`
-   `grpc`  (string, optional): A Solana gRPC URL. Defaults to nothing, and only used in conjunction with the  `docker`  option.
-   `docker`  (string, optional): The docker endpoint you want to use. Defaults to nothing. Find it with  `docker context list`  - it’s probably something like  `unix:///var/run/docker.sock`. Untested on Windows!

</details>

## Optimization Tips ⚡

<Details>
<summary> Tips </summary>

As detailed in the  [arbitrage reference](https://arb.intragon.org/home/introductions/arbitrage), Arbitrage relies on speed to capture inefficiencies between markets. This guide will help you optimize your bot for maximum efficiency.

As always - NFA, DYOR!

## Connection

There are two main components to the bot’s connection:

1.  The Solana RPC
2.  The Jupiter API

### Solana RPC

The RPC (Remote Procedural Call server) is how the bot connects to Solana. It’s important to have a fast, reliable RPC to ensure the bot can send transactions quickly.

To combat speed issues with default Solana RPCs, solutions that grab the leader information and send transactions directly to them have been developed. Two popular options include Helius’  [Atlas](https://arb.intragon.org/home/introductions/atlas)  and Blockworks’  [Lite-RPC](https://arb.intragon.org/home/introductions/lite-rpc).

To speed these solutions up even further, you can use a Geyser GRPC to enable streaming data directly to these nodes, instead of relying on yet another RPC server, which only moves the bottleneck further up the line. Generally, this requires setting up your own RPC (guide coming soon, stay tuned!), but some providers, and even members of the community, offer a standalone GRPC service.

### Jupiter API

The Jupiter API is responsible for the bot’s market data. Oftentimes, this will be the bottleneck when considering speed of finding arbs. Public instances do exist, but they are not recommended for anything other than testing. For production, it’s recommended to  [set up your own](https://arb.intragon.org/home/introductions/jupiter).

Once again, a GRPC can be used to massively speed up the API’s price updating speed. You can also filter out tokens you don’t care about, to reduce the amount of data you need to process. We reccomend running your Jupiter API on the same machine you run your bot on to reduce latency - in our testing, 2gb of RAM seems to be the minimum for a performant Jupiter API - although less is possible.

## Configuration

The bot’s configuration is key to its performance. The  [config reference](https://arb.intragon.org/v2/config)  details all the options available to you, but here are a few things you should look out for:

### Simulation

Simulating transactions increases the time-to-chain of each transaction by basically requiring the RPC server to process it twice. Unless you’re  _really_  paranoid, or you have an extremely good reason to keep it on, we reccomend turning it off. Our  [onchain program](https://arb.intragon.org/home/introductions/harbr)  catches any non-profitable arbs onchain and reverts them for you, enabling a brute-force approach to finding profitable trades while keeping peace of mind.

### Price ranges

Setting a large price range, or high trade size decimals, can slow down the bot by forcing it to check a very similar route twice or more. Keep tight, high ranges to cycle through different routes as fast as possible.

### Profit BPS

Setting a high BPS will ultimately make the bot send a lower amount of profitable txs, as it becomes very picky about what it considers for profit. This can be good if you’re looking for high-profit trades, but it also lowers the total trade volume. A good range to start testing would be 5-10bps, with a high trade price.

## Some extra tips

-   Regular Updates: Keep your bot and its components up-to-date with the latest versions and patches to ensure optimal performance and security.
-   Network Monitoring: Continuously monitor network performance and latency to identify and address any issues promptly.
-   Community Engagement: Engage with the community for shared insights, troubleshooting tips, and access to additional resources like GRPC services.

By implementing these optimization strategies, you can enhance your arbitrage bot’s efficiency and maximize its profitability. Happy trading!

</details>

## Extra Information

- **Full Documentation:** https://arbsolana.gitbook.io

- **Discord:** https://discord.com/invite/M4F8RKqgce

- **Program:** https://solscan.io/account/HARBRqBp3GL6BzN5CoSFnKVQMpGah4mkBCDFLxigGARB

## Disclaimer

- **Use a Burner Wallet:** We strongly recommend using a burner wallet when interacting with ARB Protocol. This ensures your primary funds remain secure.
- **No Control Over User Funds:** ARB Protocol does not have access to or control over any user funds. All transactions are executed directly by the user.
- **No Expectation of Profit:** There is no guarantee of profit when using ARB Protocol. Users should be aware of the risks associated with arbitrage trading.
