---
title: Testing
layout: default
nav_order: 20
---

# Testing

[Documentation home](index.md)

## Build check

From the repository root:

```bash
npm install
npm run build
```

The build compiles the TypeScript workspaces. The repository currently has no automated test suite or `npm test` script.

## Local service checks

Follow [Getting Started](getting-started.md) to configure the environment, start PostgreSQL and Redis, run migrations, and start the API. Then check:

```bash
curl http://localhost:3000/health
curl http://localhost:3000/admin/overview
```

Use the quote example in [API Reference](api-reference.md) to inspect candidate routes and the recommended chain. Quote generation alone does not submit a payment. Replace example addresses and quote IDs before testing the payment-intent flow.

## Execution checks

End-to-end execution also requires the worker, reachable RPC endpoints, configured token contracts or mints, and funded treasury accounts. Use matching development-network settings from [Configuration](configuration.md). Creating a payment intent queues execution through real chain adapters, so service health alone does not demonstrate successful settlement.

Inspect the payment status, ledger entries, and webhook deliveries after execution. See [Chain Adapters](chain-adapters.md), [Workers](workers.md), and [Webhooks](webhooks.md) for the individual stages.
