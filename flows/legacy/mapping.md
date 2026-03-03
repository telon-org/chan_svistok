# Legacy Analysis Mapping

## Node to Flow Mapping

| Understanding Node | Recommended Flow | Status | Notes |
|-------------------|------------------|--------|-------|
| / (root) | - | N/A | Architecture overview |
| at-command-protocol | sdd-at-command-protocol | DRAFT | 60+ commands, queue management |
| device-communication | sdd-device-communication | DRAFT | Serial protocol, 115200 baud |
| call-management | sdd-call-management | DRAFT | 8-state call machine |
| sms-ussd-handling | sdd-sms-ussd-handling | DRAFT | PDU encoding, UCS-2 |
| device-programming | sdd-device-programming | DRAFT | Qualcomm DIAG protocol |
| cli-management | sdd-cli-management | DRAFT | 23 CLI commands |
| database-integration | sdd-state-persistence | DRAFT | File-based KV storage |

## ADR Mapping

| Decision | Understanding Node | Type | Status |
|----------|-------------------|------|--------|
| AT command queue with timeout matching | at-command-protocol | enabling | DRAFT |
| PDU mode for SMS (vs text mode) | sms-ussd-handling | constraining | DRAFT |
| File-based state persistence | database-integration | enabling | DRAFT |
| Qualcomm DIAG protocol for firmware | device-programming | enabling | DRAFT |
| 115200 baud serial communication | device-communication | constraining | DRAFT |
| 8-state call machine | call-management | enabling | DRAFT |

## Existing Flows Check

No existing SDD/DDD/TDD/VDD/ADR flows found in project.
All recommended flows are new creations.

## Flow Generation Priority

1. **sdd-at-command-protocol** - Core protocol layer
2. **sdd-call-management** - Primary functionality
3. **sdd-device-communication** - Hardware interface
4. **sdd-sms-ussd-handling** - Secondary functionality
5. **sdd-cli-management** - Management interface
6. **sdd-device-programming** - Firmware update
7. **sdd-state-persistence** - Supporting infrastructure

## Statistics

- **Total Nodes**: 8
- **Flows Recommended**: 7 (all SDD)
- **ADRs Recommended**: 6
- **Conflicts**: 0
- **Existing Flows Preserved**: 0

## Generated

- **Date**: 2026-03-02
- **Mode**: BFS (Breadth-First Search)
- **Target**: chan_svistok/
