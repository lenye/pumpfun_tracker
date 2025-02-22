<p> English | <a href="README_CN.md"> 中文 <a/></p>

# Monitor Solana blockchain pump.fun real-time transaction activity with Webhook-based real-time transaction information push.

Utilize high-performance Solana nodes to listen in real-time to transactions related to the pump.fun platform on the
Solana blockchain, parse the transaction data, extract buy/sell information, and then push the information in JSON
format to the user's Webhook URL.

* **Real-time Push:** Real-time push of buy/sell transaction information on the pump.fun platform.
* **New Token Monitoring:** Ability to automatically detect and monitor newly created tokens on pump.fun.
* **Data Format:** Push data in JSON format for easy parsing and use by users.
* **Security:** Provide Webhook signature verification to ensure data security and reliability.

## Usage

```shell
pumpfun_tracker serve -h
Track pump.fun's live trading movements and receive real-time transaction updates via Webhooks

Usage:
  pumpfun_tracker serve [flags]

Flags:
      --websocket_endpoint string           solana RPC Websocket endpoint (required)
      --rpc_endpoint string                 solana RPC endpoint (required)
      --rpc_ratelimit int                   maximum requests per second (RPS) rate limit for solana RPC endpoint (default 5)
      --webhook_endpoint string             webhook URL. This could be a lambda, a custom API endpoint, etc (required)
      --webhook_timeout duration            webhook timeout, Maximum 30 seconds (default 5s)
      --webhook_auth_header_secret string   HTTP authentication header secret to pass into the POST requests to webhook
      --webhook_auth_header_key string      HTTP authentication header key to pass into the POST requests to webhook (default "Authorization")
      --webhook_retry_attempts uint         webhook retry attempts, Maximum 5 times (default 0, no retries)
      --webhook_retry_interval duration     webhook retry interval, Maximum 30 seconds (default 500ms)
      --trade_log_enable                    enable trade logging
      --worker_error_log_enable             worker function error logging
  -h, --help                                help for serve

Global Flags:
  -l, --level string   log level: info, warn, error (default "info")
  -f, --logfile        writing to a log file
```

## Data Format

* We will send data in JSON format (UTF-8 encoded) to the webhook endpoint via an HTTP POST request.
* sol_amount (in lamports)
* price (in SOL)

#### create

```json
{
    "market": "pump.fun",
    "instructions": [
        {
            "action": "create",
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

#### buy

```json
{
    "market": "pump.fun",
    "instructions": [
        {
            "action": "buy",
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

#### sell

```json
{
    "market": "pump.fun",
    "instructions": [
        {
            "action": "sell",
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

## Testing Environments

Here are some testing environments that are quick to set up for posting webhook events:

* [Typedwebhook.tools - A webhook testing tool for checking payloads](https://typedwebhook.tools)
* [Webhook.site - Test, process and transform emails and HTTP requests](https://webhook.site)

# Disclaimer

This project is for learning and research purposes only. Cryptocurrencies and trading involve risk, and you assume all
risks associated with using this software. We are not responsible for any losses incurred while using this software.