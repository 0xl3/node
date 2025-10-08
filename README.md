# 0xL3 Blockchain Node

This project is a Docker Compose setup for running a 0xL3 blockchain node, consisting of OP-Reth (Execution Layer) and OP-Node (Consensus Layer).

## Network Information

- **Chain ID**: 7117
- **L1 Network**: Optimism (Chain ID: 10)
- **Block Time**: 2 seconds
- **RPC Endpoint**: http://localhost:9545
- **WebSocket Endpoint**: ws://localhost:9546

## System Requirements

- Docker and Docker Compose
- OpenSSL (for generating JWT secret)
- At least 50GB free disk space for blockchain data
- At least 8GB RAM

## Installation and Usage

### 1. Generate JWT Secret

Before starting, you need to generate a JWT secret for authentication between OP-Node and OP-Reth:

```bash
openssl rand -hex 32 > config/jwt.txt
```

### 2. Verify Configuration

Check the configuration files in the `config/` directory:

- **`.env`**: Environment variables for L1 connection and peer nodes
- **`genesis.json`**: Genesis block configuration for L2 chain
- **`rollup.json`**: Rollup configuration and system parameters

### 3. Start the Node

```bash
cd node
docker-compose up -d
```

### 4. Check Status

```bash
# View OP-Reth logs
docker-compose logs -f op-reth

# View OP-Node logs
docker-compose logs -f op-node

# Check overall status
docker-compose ps
```

### 5. Stop the Node

```bash
docker-compose down
```

## Connectivity

### RPC Endpoints

- **HTTP RPC**: http://localhost:9545
- **WebSocket**: ws://localhost:9546
- **OP-Node RPC**: http://localhost:8547 (internal)

### Usage Examples

```bash
# Check chain ID
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":1}' \
  http://localhost:9545

# Check latest block number
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
  http://localhost:9545
```

## File Structure

```
.
├── config/
│   ├── .env              # Environment variables
│   ├── genesis.json      # Genesis block configuration
│   ├── rollup.json       # Rollup configuration
│   └── jwt.txt           # JWT secret (generated with openssl command)
└── node/
    ├── docker-compose.yml    # Docker Compose configuration
    ├── op-node-entrypoint    # OP-Node startup script
    ├── op-reth-entrypoint    # OP-Reth startup script
    └── data/                 # Blockchain data directory (auto-created)
```

## Features

### OP-Reth (Execution Layer)
- Supports HTTP and WebSocket RPC
- CORS enabled for cross-origin requests
- All APIs enabled (eth, net, web3, debug, etc.)
- Connected to sequencer and peer nodes

### OP-Node (Consensus Layer)
- Syncs data from L1 (Optimism)
- P2P networking support
- Alternative Data Availability (AltDA) enabled
- Automatic public IP detection

## Troubleshooting

### Common Issues

1. **JWT Secret Not Found**
   ```
   Error: JWT secret not found
   ```
   **Solution**: Generate JWT secret with `openssl rand -hex 32 > config/jwt.txt`

2. **Port Already in Use**
   ```
   Error: Port 9545 is already in use
   ```
   **Solution**: Stop the service using that port or change the port in docker-compose.yml

3. **Cannot Connect to L1**
   ```
   Error: Failed to connect to L1
   ```
   **Solution**: Check `L1_RPC_URL` in config/.env

### Viewing Logs

```bash
# View real-time logs
docker-compose logs -f

# View logs for specific service
docker-compose logs -f op-reth
docker-compose logs -f op-node

# View historical logs
docker-compose logs --tail=100 op-reth
```

## Updates

To update the node to the latest version:

```bash
# Stop the node
docker-compose down

# Pull new images
docker-compose pull

# Start again
docker-compose up -d
```

## Additional Information

- **Official Documentation**: [OP Stack Documentation](https://docs.optimism.io/)
- **OP-Reth Repository**: [paradigmxyz/op-reth](https://github.com/paradigmxyz/op-reth)
- **Optimism Repository**: [ethereum-optimism/optimism](https://github.com/ethereum-optimism/optimism)

## Support

If you encounter issues or have questions, please check the logs and configuration files first, or contact the development team for assistance.