# Sample Skill (Example)

> This is an example skill to demonstrate the expected format. Do not use this skill directly.

## Overview

This sample skill demonstrates the structure and format expected for skills in this repository. Use this as a reference when creating your own skills.

## Prerequisites

- Node.js 18+ installed
- MetaMask extension installed
- Access to an Ethereum RPC endpoint

## Instructions

### Step 1: Verify Environment

Check that the required tools are available:

```bash
node --version
npm --version
```

### Step 2: Connect to Network

Ensure the user has configured their network settings appropriately.

### Step 3: Execute the Task

Perform the main action of the skill with proper error handling.

## Examples

### Example 1: Basic Usage

```
User: "Help me with the sample task"
Agent: I'll help you with the sample task. First, let me verify your environment...
```

## Configuration

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `network` | string | No | `mainnet` | Target network |
| `timeout` | number | No | `30000` | Request timeout in ms |

## Troubleshooting

### Connection Issues

**Problem:** Unable to connect to the network.

**Solution:** Verify your RPC endpoint is accessible and your network settings are correct.

## Security Considerations

- This skill does not handle private keys
- All transactions require user confirmation via MetaMask
- No sensitive data is stored or transmitted

## References

- [MetaMask Documentation](https://docs.metamask.io/)
- [Ethereum JSON-RPC API](https://ethereum.org/en/developers/docs/apis/json-rpc/)
