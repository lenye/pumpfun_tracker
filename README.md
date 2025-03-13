<p> English | <a href="README_CN.md"> 中文 <a/></p>

# Monitor Solana blockchain pump.fun real-time transaction activity with Webhook-based real-time transaction information push.

Utilize high-performance Solana nodes to listen in real-time to transactions related to the pump.fun platform on the
Solana blockchain, parse the transaction data, extract buy/sell information, and then push the information in JSON
format to the user's Webhook URL.

* **Real-time Push:** Real-time push of buy/sell transaction information on the pump.fun platform.
* **New Token Monitoring:** Ability to automatically detect and monitor newly created tokens on pump.fun.
* **Monitoring Raydium migrations:** monitor real-time migrations of tokens from pump.fun to Raydium DEX.
* **Data Format:** Push data in JSON format for easy parsing and use by users.
* **Security:** Provide Webhook signature verification to ensure data security and reliability.

## Usage

```shell
$ pumpfun_tracker serve -h

Track pump.fun's live trading movements and receive real-time transaction updates via Webhooks

Usage:
  pumpfun_tracker serve [flags]

Flags:
  -w, --websocket_endpoint string           solana RPC Websocket endpoint (required)
  -r, --rpc_endpoint string                 solana RPC endpoint (required)
      --rpc_ratelimit int                   maximum requests per second (RPS) rate limit for solana RPC endpoint (default 5)
  -k, --webhook_endpoint string             Webhook URL. This could be a lambda, a custom API endpoint, etc (required)
      --webhook_timeout duration            Webhook request timeout, Maximum 30 seconds (default 5s)
      --webhook_auth_header_secret string   HTTP authentication header secret to pass into the POST requests to webhook
      --webhook_auth_header_key string      HTTP authentication header key to pass into the POST requests to webhook (default "Authorization")
      --webhook_retry_attempts uint         Webhook retry attempts, Maximum 5 times (default 0, no retries)
      --webhook_retry_interval duration     Webhook retry interval, Maximum 30 seconds (default 500ms)
  -t, --trade_log                           enable trade logging
      --worker_error_log                    enable worker function error logging
  -f, --logfile                             enable writing to a log file
  -l, --level string                        log level: info, warn, error (default "info")
  -h, --help                                help for serve
```

Using Helius RPC node example:

```shell
$ pumpfun_tracker serve --websocket_endpoint wss://mainnet.helius-rpc.com/?api-key=xxx --rpc_endpoint https://mainnet.helius-rpc.com/?api-key=xxx --rpc_ratelimit 3 --webhook_endpoint https://typedwebhook.tools/webhook/fdc7a346-01b2-48b5-8eae-1b577d2f47f2
```

Free trial rate limit: Solana RPC endpoint, maximum requests per second: 5.

## Release

Static binaries are available in [release section](https://github.com/lenye/pumpfun_tracker/releases)

## Data Format

HTTP header

```text
Header            Value
Content-Type      application/json; charset=utf-8
X-Block-Time      2025-02-26T16:53:17Z
X-Trace-Id        01JMFG86VZZSQFWJ210T33V8N5
X-Attempt-Count   2
```

* sol_amount (in lamports)
* price (in SOL)

#### Create

```json
{
    "instructions": [
        {
            "market": "pumpfun",
            "method": "create",
            "accounts": {
                "mint": "Kz2MfJf6ZNChZLK7Wofzma5BsnnebnPPZwN775sirCE",
                "bonding_curve": "31PV2wqSYVBp9NtjRQRnpUjJAuaBqbwY9oDqBtkj4ciV",
                "associated_bonding_curve": "DxFdVu8H4VuU9J3D2HKBShxt76bN9ns6BwqGGHuoXz1j",
                "user": "8wZiLEMF7sV6q4JbFyFNTyaBDPm5LXToKiE1n6moqDV5"
            },
            "data": {
                "name": "BENI",
                "symbol": "beni",
                "uri": "https://ipfs.io/ipfs/bafkreigegxjfgfb25vx64awsylnxc2rckbt4c766pg2ubpirj24otbcqjy"
            }
        }
    ],
    "tx_hash": "2KH3bWHYsvzz8AJjkyMugGWgaAjGRVjjnfvGixV8PS5FbR7LobTWkx7SaVgpiDbS4x7GT5M7VxsAQjkXL2WjKSvq",
    "block_time": 1727058754,
    "slot": 291446204
}
```

#### Buy

```json
{
    "instructions": [
        {
            "market": "pumpfun",
            "method": "buy",
            "accounts": {
                "mint": "FV1U42g3mvtKhWZgKf3s76sLTcXm1bGHTEwiAVHtpump",
                "bonding_curve": "Bzp9zS9KoQDRYTie7QmYZBGhQDN4ChDeGpNce9mn296g",
                "associated_bonding_curve": "J3Wmp7WSkeBAAaJhwJBTiGGWh2CB1b9FTYX1PUEzbLQA",
                "associated_user": "Fusy5dSHJwDjVnSp3mx9eRruWEME4RcQ8QhRs94F6XSs",
                "user": "devoM3KtGhwBbwHz8TQPaKRyRxMPp6t4j6pVMJepHFn"
            },
            "data": {
                "sol_amount": 361379303,
                "token_amount": 9479553659186,
                "timestamp": 1737732751,
                "price": "0.0000000385"
            }
        }
    ],
    "tx_hash": "4f35TmwfzGzfLFrEu8RukLA5ButTytEeZ5WcXkWkSpvCFDmVdJXCWjJrF91jXr7EvqzXkMPiHxhA3hRnqD7hFpm8",
    "block_time": 1737732751,
    "slot": 316088340
}
```

#### Sell

```json
{
    "instructions": [
        {
            "market": "pumpfun",
            "method": "sell",
            "accounts": {
                "mint": "4HSvu2598JRFhyfqVi6kXcsb3Y3AXepzE1iBJhGupump",
                "bonding_curve": "HkNRxBLdDcSRbC6aMb7LmGb29s5ZRg4RbC9QU4E1vjk",
                "associated_bonding_curve": "8uuptDvgwTV6LBPTXx4mstFG87k3Ytc8ziHWzxch94kC",
                "associated_user": "4MXdXg4uQRTN8pmKtWGMms2BfQ8Dq7vZDJ86PauLcD3g",
                "user": "FAThSTn33fq5qGAA7LkZNJ3xu7rroe4JtvRuoZzdthN9"
            },
            "data": {
                "sol_amount": 21003473,
                "token_amount": 168469183892,
                "timestamp": 1726935585,
                "price": "0.0000001246"
            }
        }
    ],
    "tx_hash": "5PD2KbbsRy2WUNfjimKdSq9xvqBGtaDBWXoSWn8TPkmhj5TGBPuGkWUJnjovFECGhtXwemuChRTX1hie7zyDDQkr",
    "block_time": 1726935585,
    "slot": 291165075
}
```

#### Raydium Migration

```json
{
    "instructions": [
        {
            "market": "raydium_liquidity_pool_v4",
            "method": "pumpfun_raydium_migration",
            "accounts": {
                "amm": "99xV1kc1nv8QYsoswMLKnGyh2y5BLAdqgdsRsQT35wR4",
                "coin_mint": "So11111111111111111111111111111111111111112",
                "pc_mint": "HzyjrtxxsrR7H3iUPsdMc8EqiyiCmACraVYQY12npump"
            },
            "data": {
                "open_time": 0,
                "init_pc_amount": 206900000000000,
                "init_coin_amount": 79005360080
            }
        }
    ],
    "tx_hash": "2uEGYnCtYP8LVqZaB6vS3fTQDuzvo6o34CQHTGV4FY7ESiXb2ioRQZYTNV13BPmYch6EqTaJ8yTXhRprBzhUR5Hg",
    "block_time": 1739130385,
    "slot": 319573989
}
```

## Testing Environments

Here are some testing environments that are quick to set up for posting webhook events:

* [Typedwebhook.tools - A webhook testing tool for checking payloads](https://typedwebhook.tools)
* [Webhook.site - Test, process and transform emails and HTTP requests](https://webhook.site)

## Support

If you find this project helpful, please consider giving it a ⭐️ on GitHub! If you have any further questions or need
assistance, check the [issues page](https://github.com/lenye/pumpfun_tracker/issues), feel free to reach out to me
anytime!

# Disclaimer

This project is for learning and research purposes only. Cryptocurrencies and trading involve risk, and you assume all
risks associated with using this software. We are not responsible for any losses incurred while using this software.