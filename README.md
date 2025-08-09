# Neura Testnet - Avalanche Node Setup

This repository contains Docker Compose configurations for running an Avalanche node that tracks the Neura testnet subnet.

## Overview

The Neura testnet is an Avalanche subnet running on the Fuji network. This setup allows you to run a local Avalanche node that synchronizes with and tracks the Neura subnet for development and testing purposes.

## Prerequisites

- Docker
- Docker Compose
- At least 4GB of available RAM
- Stable internet connection

## Quick Start

### Option 1: Local Development (Recommended)

```bash
docker-compose up -d
```

### Option 2: Public RPC Access

```bash
docker-compose -f docker-compose-rpc.yaml up -d
```

## Configuration Details

### Network Configuration

- **Network**: Fuji (Avalanche testnet)
- **Tracked Subnet**: `KoRyWyin2vdGScA5xUtkc83UWjtPLuDbqBr6tUkyiBfe8QGgt` (Neura subnet)
- **Image**: `avaplatform/subnet-evm_avalanchego:v0.7.8_v1.13.4`

### Port Mappings

| Port | Purpose           | Access                                                                             |
| ---- | ----------------- | ---------------------------------------------------------------------------------- |
| 9650 | HTTP RPC API      | Local: `http://localhost:9650`<br/>Public: `http://your-ip:9650` (RPC config only) |
| 9651 | P2P Communication | Node-to-node networking                                                            |

### Key Features

- **Partial Sync**: Enabled for faster initial synchronization
- **Subnet Tracking**: Specifically configured to track the Neura subnet
- **Persistent Storage**: Node data is stored in `~/.avalanchego` on the host
- **Auto-restart**: Container automatically restarts unless manually stopped

## Validator Setup and L1 Conversion

### Step 1: Bootstrap the Validator

After your node is running and synchronized, you need to bootstrap your validator to join the Neura subnet network.

### Step 2: Convert Subnet to L1

Once your validator is bootstrapped and ready, you must use the official Avalanche L1 toolbox to convert your subnet to an L1:

**🔧 [Avalanche L1 Toolbox - Convert to L1](https://build.avax.network/tools/l1-toolbox#convertToL1)**

This tool is essential for:

- Converting your subnet configuration to L1 format
- Generating the necessary L1 genesis and configuration files
- Ensuring proper L1 network setup and validation

> **Important**: This conversion step is mandatory after validator bootstrapping and before the L1 can be fully operational.

## Monitoring

Check the container logs to monitor the node's synchronization progress:

```bash
docker-compose logs -f avago
```

## Data Persistence

The node's blockchain data and configuration are persisted in your home directory at `~/.avalanchego`. This ensures that your node's state is maintained between container restarts.

## Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 9650 and 9651 are not in use by other services
2. **Insufficient disk space**: The blockchain data can grow large over time
3. **Slow sync**: Initial synchronization may take several hours depending on your internet connection

### Useful Commands

```bash
# Stop the node
docker-compose down

# View real-time logs
docker-compose logs -f

# Check container status
docker-compose ps

# Remove all data and start fresh
docker-compose down -v
rm -rf ~/.avalanchego
docker-compose up -d
```

## Security Notes

- The RPC configuration (`docker-compose-rpc.yaml`) exposes the node publicly - use only in trusted environments
- Consider implementing proper firewall rules if running with public access
- Regular backups of `~/.avalanchego` are recommended for production use

## Support

For issues related to:

- **Avalanche**: Visit [Avalanche Documentation](https://docs.avax.network/)
- **Neura Subnet**: Contact the Neura team or check their documentation
- **This Setup**: Open an issue in this repository
