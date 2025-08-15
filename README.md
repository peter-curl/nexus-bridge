# Nexus Bitcoin Bridge Protocol

A revolutionary cross-chain infrastructure enabling seamless Bitcoin integration with the Stacks ecosystem through secure tokenization and institutional-grade asset management.

## Overview

The Nexus Bridge represents a cutting-edge solution for Bitcoin liquidity mobility across blockchain networks. This protocol facilitates trustless Bitcoin deposits through advanced oracle networks, minting equivalent wrapped tokens while maintaining full collateralization.

## Key Features

- **Multi-signature Oracle Validation**: Robust oracle network for transaction verification
- **Emergency Pause Mechanisms**: Administrative controls for protocol safety
- **Dynamic Fee Structures**: Configurable bridge fees for optimal economics
- **Recipient Whitelisting**: Enhanced security through recipient validation
- **Institutional-grade Security**: Comprehensive access controls and validation
- **Full Collateralization**: 1:1 Bitcoin backing for wrapped tokens

## Architecture

### Core Components

1. **Wrapped Bitcoin Token**: SIP-10 compliant fungible token representing locked Bitcoin
2. **Oracle Network**: Authorized validators for Bitcoin transaction verification
3. **Whitelist System**: Controlled access for deposit recipients
4. **Bridge Controls**: Administrative functions for protocol management

### State Variables

- `bridge-owner`: Protocol administrator principal
- `is-bridge-paused`: Emergency pause state
- `total-locked-bitcoin`: Total Bitcoin locked in the protocol
- `bridge-fee-percentage`: Current bridge fee (basis points)
- `max-deposit-amount`: Maximum deposit per transaction

## Smart Contract Functions

### Administrative Functions

#### Oracle Management

```clarity
(add-oracle (oracle principal))
(remove-oracle (oracle principal))
```

Manage authorized oracle validators for Bitcoin transaction verification.

#### Whitelist Management

```clarity
(add-to-whitelist (recipient principal))
(remove-from-whitelist (recipient principal))
```

Control recipient access for bridge deposits.

#### Bridge Controls

```clarity
(pause-bridge)
(unpause-bridge)
(update-bridge-fee (new-fee uint))
(update-max-deposit (new-max uint))
```

Emergency controls and parameter updates.

### Core Bridge Function

#### Deposit Bitcoin

```clarity
(deposit-bitcoin (btc-tx-hash (string-ascii 64)) (amount uint) (recipient principal))
```

Processes Bitcoin deposits and mints wrapped tokens:

- Validates Bitcoin transaction hash
- Verifies oracle authorization
- Checks recipient whitelist status
- Calculates fees and mints net amount
- Updates protocol state

### Read-Only Functions

- `get-total-locked-bitcoin`: Returns total Bitcoin locked
- `get-user-balance`: Gets user's wrapped Bitcoin balance
- `is-oracle-authorized`: Checks oracle authorization status
- `get-bridge-fee-percentage`: Current bridge fee rate
- `get-max-deposit-amount`: Maximum deposit limit
- `get-bridge-status`: Bridge pause state
- `is-transaction-processed`: Transaction processing status
- `is-recipient-whitelisted`: Recipient whitelist status

## Security Features

### Access Control

- Owner-only administrative functions
- Oracle authorization validation
- Recipient whitelisting system

### Validation Mechanisms

- Transaction hash format validation
- Amount bounds checking
- Duplicate transaction prevention
- Principal address validation

### Emergency Measures

- Bridge pause functionality
- Configurable deposit limits
- Fee adjustment capabilities

## Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| u1 | ERR-NOT-AUTHORIZED | Caller not authorized for operation |
| u2 | ERR-INVALID-AMOUNT | Invalid amount specified |
| u3 | ERR-INSUFFICIENT-BALANCE | Insufficient balance for operation |
| u4 | ERR-BRIDGE-PAUSED | Bridge is currently paused |
| u5 | ERR-TRANSACTION-PROCESSED | Transaction already processed |
| u6 | ERR-ORACLE-VALIDATION-FAILED | Oracle validation failed |
| u7 | ERR-INVALID-RECIPIENT | Invalid recipient address |
| u8 | ERR-MAX-DEPOSIT-EXCEEDED | Deposit exceeds maximum limit |
| u9 | ERR-INVALID-TX-HASH | Invalid transaction hash format |

## Usage Example

### Depositing Bitcoin

1. **Oracle validates Bitcoin transaction**
2. **Call deposit function:**

   ```clarity
   (contract-call? .nexus-bridge deposit-bitcoin 
     "a1b2c3d4e5f6..." 
     u1000000  ;; 0.01 BTC in satoshis
     'SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7)
   ```

### Administrative Setup

1. **Add authorized oracles:**

   ```clarity
   (contract-call? .nexus-bridge add-oracle 'SP1ORACLE...)
   ```

2. **Whitelist recipients:**

   ```clarity
   (contract-call? .nexus-bridge add-to-whitelist 'SP1RECIPIENT...)
   ```

## Deployment Considerations

- Initialize with appropriate oracle network
- Configure initial fee parameters
- Set reasonable deposit limits
- Establish recipient whitelist
- Implement monitoring for oracle health

## Security Auditing

This contract should undergo comprehensive security auditing before mainnet deployment, focusing on:

- Oracle validation logic
- Fee calculation accuracy
- Access control mechanisms
- State management consistency
- Edge case handling

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

Please read CONTRIBUTING.md for details on code of conduct and the process for submitting pull requests.

## Support

For support and questions, please open an issue in the GitHub repository or contact the development team.
