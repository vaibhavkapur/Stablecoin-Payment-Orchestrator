# Stablecoin Payment Orchestrator

A custodial payment orchestrator that accepts merchant requests in USD and routes USDC transfers across Ethereum and Solana using cost, latency, and reliability scores.

> **[Read the full documentation](docs/index.md)**

TypeScript / Fastify / BullMQ, with PostgreSQL and Redis. Ethereum and Solana adapters submit transactions when configured with RPC endpoints and funded treasury keys; fee estimates include fixed price assumptions and fallbacks.

## Getting Started

See the [Getting Started guide](docs/getting-started.md) for prerequisites and configuration.

```bash
# Configure environment (RPC endpoints and treasury keys for execution)
cp .env.example .env

# Start Postgres & Redis
docker compose -f infra/docker/docker-compose.yml up -d

# Install & build
npm install
npm run build

# Run migrations
npm run migrate

# Start services (in separate terminals)
npm run dev:api
npm run dev:worker
```

## Quick Example

Replace destination addresses with recipients on your configured networks and `quo_...` with the returned quote ID. Creating a payment intent queues execution through the chain adapters.

```bash
# Create a quote
curl -X POST http://localhost:3000/quotes \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: test-api-key" \
  -d '{
    "amount_usd": 100.00,
    "destination": {
      "ethereum_address": "0x742d35Cc6634C0532925a3b844Bc9e7595f2bD18",
      "solana_address": "9WzDXwBbmkg8ZTbNMqUxvQRAyrZzDsGYdLVL9zYtAWWM"
    },
    "priority": "low_fee"
  }'

# Create a payment intent from the quote
curl -X POST http://localhost:3000/payment_intents \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: test-api-key" \
  -d '{ "quote_id": "quo_...", "idempotency_key": "order-123" }'
```
