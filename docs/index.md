---
title: Home
layout: default
nav_order: 1
---

# Stablecoin Payment Orchestrator

A custodial payment orchestrator that accepts merchant requests in USD and routes USDC transfers across Ethereum and Solana using cost, latency, and reliability scores.

[Get Started](getting-started.md) · [API Reference](api-reference.md) · [Repository README](https://github.com/vaibhavkapur/Stablecoin-Payment-Orchestrator/blob/main/README.md)

## Documentation

- [Getting Started](getting-started.md)
- [Architecture](architecture.md)
- [API Reference](api-reference.md)
- [Configuration Reference](configuration.md)
- [Database Schema](database.md)
- [Testing](testing.md)
- [Deployment Guide](deployment.md)
- [Routing Engine](routing-engine.md)
- [Chain Adapters](chain-adapters.md)
- [Ledger System](ledger.md)
- [Webhooks](webhooks.md)
- [Workers](workers.md)

## Overview

The Stablecoin Payment Orchestrator is a backend implementation that enables merchants to accept USDC payments across multiple blockchains through a single API. The orchestrator handles chain selection, transaction execution, confirmation monitoring, and webhook delivery.

## Key Features

- **Multi-chain routing** --- Automatically selects the optimal blockchain (Ethereum or Solana) based on real-time metrics
- **Priority-based scoring** --- Supports `low_fee`, `fast`, and `reliable` routing priorities with configurable weight profiles
- **Custodial treasury management** --- Manages treasury wallets with double-entry ledger accounting
- **Idempotent payments** --- Built-in idempotency keys prevent duplicate payment execution
- **Async execution** --- BullMQ job queue with retry logic and exponential backoff
- **Real-time health monitoring** --- Continuous chain health collection drives routing decisions
- **Webhook delivery** --- HMAC-signed webhook notifications with automatic retries
- **Admin dashboard API** --- Analytics endpoints for payment stats, route distribution, and failure analysis

## Architecture at a Glance

```
Merchant API Request
        |
   [ API Service ]  ---- POST /quotes ---- [ Routing Engine ]
        |                                        |
   POST /payment_intents                  Score & select chain
        |                                        |
   [ BullMQ Queue ]                      [ Chain Health DB ]
        |
   [ Worker Service ]
        |
   +----+----+----+
   |    |    |    |
  Exec Confirm Webhook Metrics
   |    |    |    |
  Chain Adapters   |
  (ETH / SOL)     DB
```

## Tech Stack and Scope

TypeScript / Fastify / BullMQ, with PostgreSQL and Redis. Ethereum and Solana adapters submit transactions when configured with RPC endpoints and funded treasury keys; fee estimates include fixed price assumptions and fallbacks.

| Component | Technology |
|:----------|:-----------|
| Language | TypeScript (Node.js) |
| API Framework | Fastify |
| Job Queue | BullMQ |
| Database | PostgreSQL 16 |
| Cache / Queue | Redis 7 |
| Ethereum | ethers.js |
| Solana | @solana/web3.js |
| Monorepo | Node.js workspaces (npm commands) |

## Project Structure

```
Stablecoin-Payment-Orchestrator/
├── packages/
│   ├── common/            # Shared types, DB/Redis clients, utilities
│   ├── routing-engine/    # Route selection & scoring algorithm
│   ├── chain-adapters/    # Ethereum & Solana blockchain adapters
│   └── ledger/            # Double-entry balance tracking
├── apps/
│   ├── api-service/       # Fastify REST API (port 3000)
│   └── worker-service/    # Background workers & metrics collector
├── infra/
│   ├── migrations/        # PostgreSQL schema & seed data
│   └── docker/            # Docker Compose (Postgres + Redis)
└── .env.example           # Environment variable template
```

## Related projects

These are independent companion repositories. The links describe related work, not implemented runtime integrations:

- [Agent Authorization Wallet + Merchant Trust Gateway](https://github.com/vaibhavkapur/Agent-Authorization-Wallet-Merchant-Trust-Gateway): purchase authorization, merchant verification, and execution evidence.
- [Agent Services Marketplace](https://github.com/vaibhavkapur/Agent-Services-Marketplace): service discovery, quotes, and agent purchase workflows.
- [Agentic Commerce Protocol Test Lab](https://github.com/vaibhavkapur/Agentic-Commerce-Protocol-Test-Lab): protocol fixtures, scenarios, and conformance checks.
- [Autonomous Price Watch Buyer](https://github.com/vaibhavkapur/Autonomous-Price-Watch-Buyer): price monitoring and bounded purchase decisions.
- [Cross-Merchant Procurement Agent](https://github.com/vaibhavkapur/Cross-Merchant-Procurement-Agent): merchant comparison and procurement planning.
