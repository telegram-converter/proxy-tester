<div align="center">


# PRCH — Proxy Pool Tester

**A free Windows desktop toolkit for in-depth quality testing of proxy pools: IP uniqueness, subnet diversity, residential/datacenter classification, success rate against real targets, and more — all behind a clean multi-tab GUI.**

![Platform](https://img.shields.io/badge/platform-Windows-0078D4?style=for-the-badge&logo=windows)
![Interface](https://img.shields.io/badge/interface-PyQt5-41CD52?style=for-the-badge&logo=qt&logoColor=white)
![Languages](https://img.shields.io/badge/languages-Python-5865F2?style=for-the-badge)
![License](https://img.shields.io/badge/license-free-orange?style=for-the-badge)

---
<img width="1024" alt="PRCH — Proxy Pool Tester" src="https://github.com/user-attachments/assets/2de91e63-ee99-48c5-b54a-1bd463061362" />
</div>

---

## Table of Contents
- [Overview](#overview)
- [Key Features](#key-features)
- [Proxy Tester](#proxy-tester)
- [IP Compare](#ip-compare)
- [IP API Check](#ip-api-check)
- [Success Rate](#success-rate)
- [Typical Workflow](#typical-workflow)
- [Download](#download)
- [Screenshots](#screenshots)
- [Contacts](#contacts)
- [Donations](#donations)

---

## Overview

Most "proxy checkers" only tell you whether a proxy is alive. **PRCH (Proxy Pool Tester)** answers the questions that actually matter when you are paying for a proxy pool:

- **How diverse is the exit pool?** How many unique IPs and unique Class C subnets appear across thousands of requests?
- **Is it really residential?** Each exit IP is classified as residential or datacenter/proxy.
- **Does it actually work against real websites?** Per-target success rate including captcha challenges, blocks and rate limits.
- **How stable is it under load?** Hundreds of concurrent threads with automatic proxy rotation and failure handling.

Everything runs locally on your machine — no accounts, no limits, free of charge.

## Key Features

| Feature                       | Description                                                                                             |
| ----------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Free, no limits**           | Full functionality, free updates.                                                                       |
| **HTTP & SOCKS5**             | `address:port:login:password` format — one or many proxies per pool.                                    |
| **Automatic rotation**        | Every request picks a random proxy from the pool; faulty proxies are retried, then rotated out.         |
| **High concurrency**          | Hundreds of simultaneous threads with tunable request count, delay and timeout.                         |
| **Real exit-IP verification** | Apparent IP is checked through 25+ independent IP echo services.                                        |
| **IP intelligence**           | Residential / datacenter classification with batched API calls and rate-limit handling.                 |
| **Success rate testing**      | Measure how targets treat your proxy traffic: OK / captcha / forbidden / blocked.                       |
| **Persistent settings**       | All options (including the proxy list) are saved automatically; one-click restore via `LAST RUN SET`.   |
| **Comfortable UI**            | Dark / light theme, colored real-time console log, portable single-file EXE — no installation required. |

## Proxy Tester

The core module — stress-tests your proxy pool with a configurable number of requests and records the real exit IP of every response.

**Controls & options**

| Setting         | Description                                                                                                                                                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requests        | Total number of requests for the task.                                                                                                                                                                                        |
| Threads         | Number of concurrent workers.                                                                                                                                                                                                 |
| Delay (sec)     | Pause between requests.                                                                                                                                                                                                       |
| Timeout (sec)   | Per-request network timeout.                                                                                                                                                                                                  |
| Max proxy fails | How many failures a proxy tolerates before it is rotated out.                                                                                                                                                                 |
| Bad proxy log   | Log every proxy that exhausted its retry loop.                                                                                                                                                                                |
| Raw IP export   | Save every collected exit IP to a file for further analysis.                                                                                                                                                                  |
| IP Type Check   | Classify sampled exit IPs as *residential* or *datacenter/proxy* via the ip-api.com batch API — configurable sample limit, batch delay and batch attempts, with the option to route API calls through the task's own proxies. |

**Run statistics**

| Metric                             | Description                                             |
| ---------------------------------- | ------------------------------------------------------- |
| Requests done                      | Live progress counter in the status bar.                |
| Requests made / completed / failed | Task totals at a glance.                                |
| Unique IPs (%)                     | Distinct exit IPs returned, with uniqueness percentage. |
| Unique subnets (Class C)           | Diversity of the exit pool at the subnet level.         |
| Elapsed time                       | Total run duration.                                     |

**One-click tools**

| Tool         | Description                                                                           |
| ------------ | ------------------------------------------------------------------------------------- |
| Check Proxy  | Instant single-proxy verification with retry logic and 407 (auth required) detection. |
| LAST RUN SET | Restore all settings of the previous run, including the proxy list.                   |

## IP Compare

| Feature                      | Description                                                                              |
| ---------------------------- | ---------------------------------------------------------------------------------------- |
| Intersection of two IP lists | Load two IP files and instantly see the matching exit IPs.                               |
| Pool diffing                 | Compare two runs, two providers, or before/after a proxy plan change to see the overlap. |

## IP API Check

Bulk IP intelligence for any IP list (e.g. one exported by the Proxy Tester) via the ip-api.com batch endpoint — including ISP, organization and AS-name details.

**Classification**

| Result             | Meaning                                                   |
| ------------------ | --------------------------------------------------------- |
| Residential        | No hosting/proxy flags and no datacenter ASN.             |
| Datacenter / proxy | `hosting` or `proxy` flag set, or a known datacenter ASN. |
| Unknown            | API failure or unresolved record for this IP.             |

**Controls & options**

| Setting                 | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| Batch size              | IPs per batch request (default 100).                         |
| Delay (sec)             | Pause between batches.                                       |
| Attempts                | Retries per batch (0 = unlimited).                           |
| Timeout (sec)           | Per-request timeout.                                         |
| Use task proxy          | Route API requests through your own proxies.                 |
| Save report             | Detailed per-IP report file in `ip_api_reports`.             |
| Rate-limit handling     | Automatic backoff on 429 / `X-Rl` / `X-Ttl` API limits.      |
| Progress bar & live log | Per-batch progress and detailed status messages.             |
| Summary panel           | Residential / datacenter / unknown totals for the whole run. |

## Success Rate

Measures how reliably a proxy pool reaches real-world targets: point it at any list of URLs (one per line) — default presets (Google, Cloudflare, etc.) load with one click.

**Request classification**

| Result                     | Meaning                                                                   |
| -------------------------- | ------------------------------------------------------------------------- |
| **OK**                     | Target answered with HTTP 200.                                            |
| **Captcha / challenge**    | Anti-bot page detected (reCAPTCHA, hCaptcha, Cloudflare challenge, etc.). |
| **Forbidden**              | HTTP 403 — access denied.                                                 |
| **Blocked / rate-limited** | HTTP 429 or 503.                                                          |
| **HTTP error**             | Any other 4xx/5xx status code.                                            |
| **Timeout**                | Request exceeded the configured timeout.                                  |
| **Proxy error**            | The proxy itself failed (connection or authentication).                   |

**Controls & options**

| Setting                      | Description                                               |
| ---------------------------- | --------------------------------------------------------- |
| Requests                     | Total number of requests across all targets.              |
| Threads                      | Concurrent workers.                                       |
| Delay (sec)                  | Pause between requests.                                   |
| Timeout (sec)                | Per-request timeout.                                      |
| Max proxy fails              | Retries before rotating to another proxy.                 |
| Follow redirects             | Enable or disable redirect following.                     |
| Save report                  | Detailed per-request report file.                         |
| Progress bar & live counters | Per-category progress in real time, plus a final summary. |

## Typical Workflow

1. Paste your proxy pool (`address:port:login:password` per line) and run the **Proxy Tester** — collect thousands of exit IPs.
2. Enable **Raw IP Export**, then feed the exported list into **IP API Check** for a full residential/datacenter breakdown.
3. Point **Success Rate** at the sites you actually work with to see captcha/block rates.
4. Use **IP Compare** to diff exit pools across providers or runs.

## Download

- [Always Latest Release](https://github.com/telegram-converter/proxy-tester/releases/latest)

Portable Windows executable — just download and run.


## Screenshots

<img width="512" halt="PRCH — Proxy Pool Tester" src="https://github.com/user-attachments/assets/e44a4ec5-a928-46e8-a449-a044581b6a1a" />
<img width="512" halt="PRCH — Proxy Pool Tester" src="https://github.com/user-attachments/assets/bee6c9e2-a6ae-4b44-9283-751950e455af" />
<img width="512" halt="PRCH — Proxy Pool Tester" src="https://github.com/user-attachments/assets/27d2d4ab-e713-4730-b5e2-716bdc026435" />
<img width="512" halt="PRCH — Proxy Pool Tester" src="https://github.com/user-attachments/assets/e3abbab6-1f6d-4107-a11f-b22acaaed038" />

## Contacts

- Telegram: [Send message](https://telegram-converter.com/telegram-contact)
- Discord:  [Send message](https://telegram-converter.com/discord-contact)
- Matrix:   [Send message](https://telegram-converter.com/icq-contact)
- Partner Products: [WebSite](https://telegram-converter.com/partners/)

## Donations

* [Buy us a coffee ☕️](https://nowpayments.io/donation/mcv)
* Thank you!
