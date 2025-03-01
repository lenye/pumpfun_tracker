<p> <a href="README.md"> English <a/> |  中文 </p>

# 监控 Solana 区块链 pump.fun 实时交易动向，Webhook 实时推送交易信息。

使用高性能的 Solana 节点，实时监听 Solana 区块链上与 pump.fun 平台相关的交易，并解析交易数据，提取买入/卖出信息，然后以 JSON
格式推送到用户的 Webhook URL。

* **实时推送:** 实时推送 pump.fun 平台上的买入/卖出交易信息。
* **新代币监控:** 能够自动检测和监控 pump.fun 上新创建的代币。
* **监控 Raydium 迁移:** 监控代币从 pump.fun 到 Raydium DEX 的实时迁移。
* **数据格式:** 以 JSON 格式推送数据，方便用户解析和使用。
* **安全性:** 提供 Webhook 签名验证，确保数据安全可靠。

## 使用说明

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

试用版本速率限制：Solana RPC endpoint，每秒最大请求数：5

## 数据格式

HTTP header

```text
Header            Value
Content-Type      application/json; charset=utf-8
X-Trace-Id        01JMFG86VZZSQFWJ210T33V8N5
X-Attempt-Count   2
```

* sol_amount 单位为 lamports
* price 单位为 SOL

#### 创建

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

#### 买入

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

#### 卖出

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

#### raydium 迁移

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

## 测试环境

这里列出了一些可以快速搭建的测试环境，用来接收 Webhook 事件：

* [Typedwebhook.tools - A webhook testing tool for checking payloads](https://typedwebhook.tools)
* [Webhook.site - Test, process and transform emails and HTTP requests](https://webhook.site)

# 免责声明

本项目仅供学习和研究之用，加密币及交易存在风险，使用此软件的风险由您自行承担，我们对使用该软件时产生的任何损失概不负责。