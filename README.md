# 🔐 SeaLog - Tamper-Evident Log Integrity System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node.js-20+-green)](https://nodejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-blue)](https://www.typescriptlang.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-336791)](https://www.postgresql.org)

SeaLog is a production-ready cryptographic integrity layer that provides **tamper-evident logging** using Merkle trees and Ethereum blockchain anchoring. It adds cryptographic guarantees to your existing logging infrastructure without requiring storage replacement.

## 🎯 Overview

SeaLog bridges the gap between traditional logging systems and cryptographic security. It works alongside your existing logging infrastructure (ELK, CloudWatch, Splunk, etc.) to provide:

- **Cryptographically enforceable immutability** through append-only constraints
- **Efficient proof generation** using position-preserving binary Merkle trees
- **Cross-batch consistency verification** via cryptographic hash chaining
- **Blockchain anchoring** for external immutable timestamping
- **Zero-trust verification** architecture - independent auditors can verify without trusting SeaLog
- **Real-time tamper detection** through visual dashboard

## ⚡ Quick Start

### Prerequisites

```bash
- Node.js 20 or higher
- npm or yarn
- PostgreSQL 15+
- Docker (optional, for containerized development)
```

### 5-Minute Setup

```bash
# 1. Clone the repository
git clone https://github.com/Yash-172003/SeaLog.git
cd SeaLog

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env
# Edit .env with your configuration (see Configuration section)

# 4. Start PostgreSQL
docker-compose up -d postgres

# 5. Run database migrations
npm run db:migrate

# 6. Generate Prisma client
npm run db:generate

# 7. Start the development server
npm run dev
```

The API will be available at `http://localhost:3000`

---

## 📁 Project Structure

```

SeaLog/
│
├── benchmarks/
│   ├── benchmark.ts                    # Performance benchmark suite
│   ├── generate_graphs.ts              # Benchmark visualization
│   └── research_benchmark.ts           # Research-grade benchmarking
│
├── contracts/
│   ├── scripts/
│   │   └── deploy.js                   # Smart contract deployment
│   └── SeaLogAnchor.sol                # Blockchain anchor contract
│
├── dashboard/
│   ├── public/
│   │   ├── favicon.svg
│   │   └── icons.svg
│   │
│   ├── src/
│   │   ├── api/
│   │   │   └── client.ts
│   │   │
│   │   ├── assets/
│   │   │   ├── hero.png
│   │   │   ├── react.svg
│   │   │   └── vite.svg
│   │   │
│   │   ├── types/
│   │   │   └── api.ts
│   │   │
│   │   ├── utils/
│   │   │   └── tree.ts
│   │   │
│   │   ├── App.tsx
│   │   ├── App.css
│   │   ├── index.css
│   │   └── main.tsx
│   │
│   ├── README.md
│   ├── eslint.config.js
│   ├── index.html
│   ├── package.json
│   ├── package-lock.json
│   ├── tsconfig.app.json
│   ├── tsconfig.json
│   ├── tsconfig.node.json
│   └── vite.config.ts
│
├── docs/
│   ├── architecture/
│   │   ├── gantt_chart.png
│   │   └── sequence_diagram.png
│   │
│   ├── CRYPTOGRAPHIC_VERIFICATION.md
│   ├── INVARIANTS.md
│   ├── LOG_RETENTION_POLICY.md
│   ├── STUDY_GUIDE.md
│   └── TESTING_GUIDE.md
│
├── prisma/
│   ├── migrations/
│   │   ├── 20260125000932_init/
│   │   │   └── migration.sql
│   │   │
│   │   ├── 20260420000000_batch_performance_metrics/
│   │   │   └── migration.sql
│   │   │
│   │   ├── append_only_enforcement.sql
│   │   └── migration_lock.toml
│   │
│   └── schema.prisma
│
├── scripts/
│   └── e2e_blockchain.ts               # End-to-end blockchain validation
│
├── src/
│   ├── __tests__/
│   │   ├── chain.test.ts
│   │   ├── integration.test.ts
│   │   ├── integrity.test.ts
│   │   └── verification.concrete.test.ts
│   │
│   ├── anchoring/
│   │   ├── implementation.ts
│   │   └── index.ts
│   │
│   ├── batching/
│   │   ├── implementation.ts
│   │   └── index.ts
│   │
│   ├── ingestion/
│   │   ├── implementation.ts
│   │   └── index.ts
│   │
│   ├── merkle/
│   │   ├── implementation.ts
│   │   └── index.ts
│   │
│   ├── storage/
│   │   ├── implementation.ts
│   │   └── index.ts
│   │
│   ├── types/
│   │   └── index.ts
│   │
│   ├── verification/
│   │   ├── implementation.ts
│   │   └── index.ts
│   │
│   ├── api.ts
│   └── index.ts
│
├── LICENSE
├── README.md
├── docker-compose.yml
├── hardhat.config.js
├── jest.config.js
├── package.json
├── package-lock.json
├── test_hash_debug.ts
├── test_leaf3.ts
├── test_tree_actual.ts
└── tsconfig.json

```

### Directory Descriptions

| Directory | Purpose | Key Files |
|-----------|---------|-----------|
| `src/` | Core application logic | `index.ts` in each subdirectory |
| `prisma/` | Database schema and ORM | `schema.prisma`, `migrations/` |
| `contracts/` | Smart contracts for blockchain | `*.sol` files |
| `dashboard/` | React frontend application | `app/`, `components/` |
| `docs/` | Comprehensive documentation | `INVARIANTS.md` (read first!) |
| `benchmarks/` | Performance analysis | `benchmark.ts`, graph generation |
| `scripts/` | Automation & deployment | `deploy.js`, `e2e_blockchain.ts` |

---

## 🔧 Configuration

### Environment Variables

Create a `.env` file based on `.env.example`:

```bash
# Database Configuration
DATABASE_URL="postgresql://sealog_user:password@localhost:5432/sealog"
DATABASE_POOL_SIZE=20

# Server Configuration
NODE_ENV=development
PORT=3000
API_BASE_URL="http://localhost:3000"

# Ethereum Configuration
ETHEREUM_RPC_URL="http://localhost:8545"  # or Infura/Alchemy URL
ETHEREUM_PRIVATE_KEY="your_private_key_here"
ETHEREUM_NETWORK="hardhat"  # or "sepolia", "mainnet"
ANCHOR_CONTRACT_ADDRESS="0x..."

# Logging
LOG_LEVEL="info"
LOG_FORMAT="json"

# Optional: External Services
SENTRY_DSN=""
DATADOG_API_KEY=""
```

### Database Setup

```bash
# Start PostgreSQL container
docker-compose up -d postgres

# Create database and user
psql -U postgres -c "CREATE DATABASE sealog;"
psql -U postgres -c "CREATE USER sealog_user WITH PASSWORD 'password';"
psql -U postgres -c "ALTER ROLE sealog_user CREATEDB;"

# Run migrations (includes trigger creation)
npm run db:migrate

# Critical: Revoke dangerous permissions (run as superuser)
psql -U postgres -d sealog << EOF
REVOKE UPDATE, DELETE, TRUNCATE ON logs FROM sealog_user;
REVOKE UPDATE, DELETE, TRUNCATE ON batches FROM sealog_user;
EOF
```

---

## 🚀 Development

### Available Scripts

```bash
# Development
npm run dev              # Start dev server with hot reload

# Building
npm run build            # Build for production
npm run preview          # Preview production build

# Testing
npm test                 # Run all tests
npm test -- integrity    # Run specific test suite
npm run test:watch       # Watch mode for TDD

# Code Quality
npm run lint             # Run ESLint
npm run typecheck        # Run TypeScript type checking
npm run format           # Format code with Prettier

# Database
npm run db:migrate       # Run pending migrations
npm run db:reset         # Reset database (⚠️ destructive)
npm run db:generate      # Generate Prisma client

# Benchmarking
npm run benchmark        # Run performance benchmarks
npm run benchmark:graph  # Generate O(log N) complexity graphs

# Blockchain
npm run contracts:build  # Compile Solidity contracts
npm run contracts:test   # Test smart contracts
npm run contracts:deploy # Deploy to blockchain
```

### Development Workflow

1. **Create feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Write tests first (TDD)**
   ```bash
   npm run test:watch
   ```

3. **Implement feature**
   ```bash
   # Edit src/ files
   # TypeScript will catch type errors
   ```

4. **Format and lint**
   ```bash
   npm run format && npm run lint
   ```

5. **Verify tests pass**
   ```bash
   npm test
   ```

6. **Commit and push**
   ```bash
   git add .
   git commit -m "feat: add feature description"
   git push origin feature/your-feature-name
   ```

---

## 📡 API Documentation

### Authentication
Currently, SeaLog uses no authentication in development mode. Production deployments should implement API key or OAuth2.

### Endpoints

#### 1. Ingest Logs

**Endpoint:** `POST /api/v1/logs/ingest`

**Request:**
```json
{
  "source_service": "auth-service",
  "log_level": "INFO",
  "message": "User login successful",
  "metadata": {
    "user_id": "12345",
    "ip_address": "192.168.1.1"
  }
}
```

**Response (201 Created):**
```json
{
  "log_id": "550e8400-e29b-41d4-a716-446655440001",
  "sequence_number": 1001,
  "acknowledged_at": "2024-01-15T10:30:01.500Z",
  "batch_status": "pending"
}
```

**Error (400 Bad Request):**
```json
{
  "error": "Invalid log format",
  "details": "message field is required"
}
```

---

#### 2. Verify Single Log

**Endpoint:** `GET /api/v1/verify/log/:log_id`

**Response (200 OK):**
```json
{
  "log_id": "550e8400-e29b-41d4-a716-446655440001",
  "verification_status": "verified",
  "merkle_proof": {
    "leaf_hash": "0xabc123...",
    "siblings": ["0xdef456...", "0xghi789..."],
    "path": [0, 1, 0],
    "root": "0xjkl012..."
  },
  "batch_info": {
    "batch_id": "batch_001",
    "batch_number": 5,
    "size": 256,
    "created_at": "2024-01-15T10:25:00.000Z"
  },
  "blockchain_anchor": {
    "tx_hash": "0x1234...",
    "block_number": 19542103,
    "timestamp": "2024-01-15T10:30:00.000Z",
    "network": "mainnet"
  },
  "integrity_metrics": {
    "timestamp_skew": 2,
    "integrity_score": 0.99,
    "proof_source": "DERIVED"
  }
}
```

---

#### 3. Verify Batch

**Endpoint:** `GET /api/v1/verify/batch/:batch_id`

**Response (200 OK):**
```json
{
  "batch_id": "batch_001",
  "batch_number": 5,
  "verification_status": "verified",
  "merkle_root": "0xjkl012...",
  "blockchain_anchor": {
    "tx_hash": "0x1234...",
    "block_number": 19542103,
    "timestamp": "2024-01-15T10:30:00.000Z"
  },
  "logs_count": 256,
  "logs": [
    {
      "log_id": "550e8400-e29b-41d4-a716-446655440001",
      "verification_status": "verified"
    }
    // ... more logs
  ]
}
```

---

#### 4. Verify Cross-Batch Chain

**Endpoint:** `GET /api/v1/verify/chain`

Detects batch deletion and validates cryptographic continuity.

**Response (200 OK):**
```json
{
  "chain_status": "verified",
  "total_batches": 42,
  "gaps_detected": 0,
  "batches": [
    {
      "batch_number": 40,
      "hash": "0xabc123...",
      "previous_hash": "0xdef456...",
      "hash_match": true,
      "timestamp": "2024-01-15T10:25:00.000Z"
    }
    // ... batch chain
  ]
}
```

---

#### 5. Generate Audit Bundle

**Endpoint:** `GET /api/v1/audit/:log_id`

Generates a standalone, offline-verifiable audit package.

**Response (200 OK):**
```json
{
  "audit_bundle": {
    "log_id": "550e8400-e29b-41d4-a716-446655440001",
    "log_data": { /* ... */ },
    "merkle_proof": { /* ... */ },
    "batch_data": { /* ... */ },
    "blockchain_proof": { /* ... */ },
    "verification_instructions": "Use the standalone verifier tool...",
    "generated_at": "2024-01-15T10:30:05.000Z"
  }
}
```

---

#### 6. Manual Batch Trigger

**Endpoint:** `POST /api/v1/admin/batch/trigger`

Manually creates a batch (normally automatic).

**Request:**
```json
{
  "batch_name": "manual_audit_batch",
  "trigger_reason": "compliance_audit"
}
```

**Response (200 OK):**
```json
{
  "batch_id": "batch_manual_001",
  "status": "created",
  "logs_batched": 245,
  "merkle_root": "0xabc123...",
  "timestamp": "2024-01-15T10:35:00.000Z"
}
```

---

#### 7. Get Batch Status

**Endpoint:** `GET /api/v1/batch/:batch_id`

**Response (200 OK):**
```json
{
  "batch_id": "batch_001",
  "batch_number": 5,
  "status": "anchored",
  "logs_count": 256,
  "created_at": "2024-01-15T10:25:00.000Z",
  "anchored_at": "2024-01-15T10:30:00.000Z",
  "merkle_root": "0xjkl012...",
  "blockchain_tx": "0x1234..."
}
```

---

#### 8. Health Check

**Endpoint:** `GET /health`

**Response (200 OK):**
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:05.000Z",
  "services": {
    "database": "connected",
    "blockchain": "connected",
    "cache": "healthy"
  }
}
```

---

## 🧪 Testing

### Test Structure

```
src/__tests__/
├── chain.test.ts                 # Cross-batch hash chaining
├── integrity.test.ts             # Append-only & immutability
├── verification.concrete.test.ts # Real-world verification scenarios
└── integration.test.ts           # End-to-end audit pipeline validation
                   
```

### Running Tests

```bash
# Run all tests
npm test

# Run specific test file
npm test -- integrity.test.ts

# Run with coverage report
npm test -- --coverage

# Watch mode (for TDD)
npm run test:watch

# Run tests matching pattern
npm test -- --testNamePattern="Merkle"
```

### Test Phases & Results

#### Phase 1: Determinism & Integrity ✅
- ✅ Deterministic Merkle roots
- ✅ Append-only enforcement
- ✅ Timestamp manipulation detection
- ✅ Position-preserving hashing
- ✅ Independent offline verification

**Result:** 8/8 tests passing

#### Phase 2: Research Enhancements ✅
- ✅ Genesis chain hash stability
- ✅ Cross-batch hash chaining
- ✅ Batch deletion detection
- ✅ Batch tampering detection
- ✅ Zero-trust validation

**Result:** 20/20 tests passing

#### Phase 3: Empirical Benchmarking ✅
- ✅ O(log N) proof size complexity (N=500)
- ✅ Ingestion throughput (~4,300 logs/sec)
- ✅ End-to-end pipeline latency

---

## 🔐 Security Architecture

## System Invariants

SeaLog is built on **11 immutable security and cryptographic invariants** that guarantee auditability, integrity, and tamper-evidence.

### Key Invariants

1. **Append-Only Logs** – Logs can never be modified or deleted.
2. **Global Sequence Ordering** – Every log has a unique deterministic order.
3. **Dual Timestamps** – Event and ingestion timestamps are cryptographically committed.
4. **Position-Preserving Merkle Trees** – Merkle proofs retain left/right ordering.
5. **Deterministic Verification** – Independent parties always derive identical results.
6. **On-Chain Commitments Only** – Only Merkle roots are anchored to the blockchain.
7. **Derived Proofs** – Verification never trusts cached database proofs.
8. **Permission Separation** – Database controls enforce immutability.
9. **Immutable Batches** – Batch contents and roots cannot change after creation.
10. **Blockchain Source of Truth** – On-chain anchors are the final authority.
11. **Cross-Batch Hash Chaining** – Batch deletion or reordering is detectable.

📖 See **`docs/INVARIANTS.md`** for the complete specification.

### Security Features

```
┌─────────────────────────────────────────┐
│         SeaLog Security Stack           │
├─────────────────────────────────────────┤
│  Application Layer                      │
│  ├─ Zero-trust verification            │
│  └─ Proof derivation (never cached)     │
├─────────────────────────────────────────┤
│  Cryptographic Layer                    │
│  ├─ Keccak-256 hashing                 │
│  ├─ Position-preserving Merkle         │
│  └─ Bitcoin-style odd-node duplication │
├─────────────────────────────────────────┤
│  Database Layer                         │
│  ├─ Append-only enforcement             │
│  ├─ Dual timestamp model                │
│  └─ Cross-batch hash chaining           │
├─────────────────────────────────────────┤
│  Blockchain Layer                       │
│  ├─ Immutable anchoring (Ethereum)     │
│  └─ External timestamp source           │
└─────────────────────────────────────────┘
```

---

## 📊 Benchmarking & Performance

### Run Benchmarks

```bash
# Run performance benchmarks
npm run benchmark

# Generate complexity analysis graphs
npm run benchmark:graph
```

### Performance Characteristics

| Metric | Value | Notes |
|--------|-------|-------|
| Proof Size Complexity | O(log N) | Linear in tree height |
| Max Tree Size | 500+ | Empirically validated |
| Ingestion Throughput | ~4,300 logs/sec | Single-thread baseline |
| Average Batch Time | ~5 seconds | Auto-trigger on 256 logs |
| Verification Latency | <100ms | For single log proof |

### Scaling Analysis

The `benchmarks/generate_graphs.ts` script produces empirical O(log N) graphs proving linear scaling with log count.

---

## 🌐 Blockchain Integration

### Smart Contracts

Located in `contracts/scripts/`:

- **SeaLogAnchor.sol** - Main anchoring contract
- **deployment.js** - Contract deployment script

### Deployment

```bash
# Compile contracts
npm run contracts:build

# Test contracts
npm run contracts:test

# Deploy to blockchain
npm run contracts:deploy

# Deploy to testnet (Sepolia)
ETHEREUM_NETWORK=sepolia npm run contracts:deploy
```

### Contract Interaction

Merkle roots are anchored to Ethereum at regular intervals:

```solidity
// Contract stores batch anchors
event MerkleRootAnchored(
  bytes32 indexed batchId,
  bytes32 merkleRoot,
  uint256 timestamp
);
```

---

## 📚 Documentation

### Essential Reading (In Order)

1. **[docs/INVARIANTS.md](./docs/INVARIANTS.md)** - Core design principles (⭐ START HERE)
2. **[docs/STUDY_GUIDE.md](./docs/STUDY_GUIDE.md)** - Proof correctness
3. **[docs/CRYPTOGRAPHIC_VERIFICATION.md](./docs/CRYPTOGRAPHIC_VERIFICATION.md)** - Proof correctness
4. **[docs/LOG_RETENTION_POLICY.md](./docs/LOG_RETENTION_POLICY.md)** - Proof correctness
5. **[docs/TESTING_GUIDE.md](./docs/TESTING_GUIDE.md)** - Testing methodology


---

## 🚢 Deployment

### Prerequisites

- Production PostgreSQL instance
- Ethereum mainnet or L2 connection (Infura, Alchemy, or self-hosted)
- Node.js 20+ runtime
- TLS certificates for HTTPS

### Production Checklist

See `docs/PRE_DEPLOYMENT_SANITY_CHECK.md` for complete pre-deployment verification.

Key steps:

1. Run all tests: `npm test`
2. Build for production: `npm run build`
3. Verify type safety: `npm run typecheck`
4. Review environment: `npm run env:validate`
5. Test blockchain connection
6. Perform database sanity checks
7. Run end-to-end tests

### Deployment via Docker

```bash
# Build Docker image
docker build -t sealog:latest .

# Run container
docker run -p 3000:3000 \
  --env-file .env.production \
  sealog:latest

# With Docker Compose (for full stack)
docker-compose -f docker-compose.prod.yml up -d
```

---

## 🐛 Troubleshooting

### Common Issues

**Issue: Database connection refused**
```bash
# Check PostgreSQL is running
docker-compose ps

# Verify DATABASE_URL in .env
# Format: postgresql://user:password@host:port/database
```

**Issue: Blockchain connection timeout**
```bash
# Test Ethereum RPC endpoint
curl -X POST https://your-rpc-url \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","id":1}'
```

**Issue: Type errors in TypeScript**
```bash
# Regenerate Prisma types
npm run db:generate

# Type-check entire project
npm run typecheck
```

**Issue: Tests failing**
```bash
# Reset test database
npm run db:reset

# Run single test for debugging
npm test -- verification.concrete.test.ts
```

### Debug Mode

Enable detailed logging:

```bash
# Start with debug logging
DEBUG=sealog:* npm run dev

# Or set in .env
LOG_LEVEL=debug
```

---

## 🤝 Contributing

We welcome contributions! Please follow the development workflow:

1. **Fork** the repository
2. **Create** a feature branch (`feature/your-feature`)
3. **Write tests** for your changes
4. **Submit** a pull request with clear description

### Code Standards

- **TypeScript** - Strict mode enabled
- **ESLint** - No warnings
- **Prettier** - Code formatting required
- **Tests** - 100% of new code must be tested
- **Types** - No `any` types allowed

---

## 📄 License

MIT License - See `LICENSE` file for details.

---

## 🎓 Academic Context

SeaLog was developed as a capstone project demonstrating:

- Advanced cryptographic engineering
- Blockchain integration patterns
- Production-grade system design
- Security-first development practices
- Research-grade benchmarking methodology

---

## 📞 Support & Contact

For issues, questions, or collaboration inquiries:

- **GitHub Issues**: [Report bugs or request features](https://github.com/Yash-172003/SeaLog/issues)
- **Documentation**: See `docs/` directory
- **Code Examples**: Check `src/_tests_/` for usage patterns

---

## 🙏 Acknowledgments

Built with:
- TypeScript & Node.js
- PostgreSQL & Prisma ORM
- Ethereum & Hardhat
- React & Next.js 15
- Cryptographic libraries (Keccak-256, ECDSA)

---

**Last Updated:** January 2026  
**Version:** 1.0.0-crypto-locked  
**Status:** ✅ MVP Complete & Production-Ready
