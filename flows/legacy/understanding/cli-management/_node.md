# Understanding: CLI Management

## Phase: EXPLORING

## Validated Understanding

**Core Purpose**: Asterisk CLI interface for device management and control.

**CLI Commands Registered** (23 commands in `cli.c` - 1643 lines):

**Show Commands**:
1. `dongle show devices` - List all dongles (ID, Group, State, RSSI, Provider, IMEI, IMSI, Balance)
2. `dongle show devicesl` - List with limits info
3. `dongle show devicesd` - Detailed device list
4. `dongle show devicesi` - Info list
5. `dongle show device settings` - Show specific device settings
6. `dongle show device state` - Show device state
7. `dongle show device statistics` - Show device statistics
8. `dongle show version` - Show driver version

**Control Commands**:
9. `dongle cmd <device> <at_command>` - Execute raw AT command
10. `dongle diagmode <device>` - Enter diagnostic mode
11. `dongle changeimei <device> <imei>` - Change IMEI
12. `dongle update <device>` - Firmware update
13. `dongle setgroup <device> <group>` - Set device group
14. `dongle setgroupimsi <imsi> <group>` - Set group by IMSI
15. `dongle ussd <device> <code>` - Send USSD
16. `dongle sms <device> <number> <message>` - Send SMS
17. `dongle pdu <device> <pdu>` - Send PDU
18. `dongle callwaiting <device>` - Toggle call waiting
19. `dongle reset <device>` - Reset device
20. `dongle start <device>` - Start device
21. `dongle reload` - Reload configuration
22. `dongle discovery` - Device discovery

**Key Features**:
- Tab completion for device names (`complete_device()`)
- Device locking during operations
- State persistence via `readpvtinfo()`, `readpvtlimits()`
- Integration with firmware update (`ttyprog_svistok.c`)

**Implementation Pattern**:
```c
static char* cli_<name>(struct ast_cli_entry* e, int cmd, struct ast_cli_args* a)
{
    switch (cmd) {
        case CLI_INIT:
            e->command = "dongle ...";
            e->usage = "Usage: ...";
            return NULL;
        case CLI_GENERATE:
            return NULL;
    }
    // Command implementation
}
```

## Sources Analyzed
- `cli.c` (1643 lines) - Full CLI implementation
- `cli.h` - CLI registration API

## Children (Discovered)
| Child | Status |
|-------|--------|
| command-registration | COMPLETE |
| device-commands | COMPLETE |
| at-command-passthrough | COMPLETE |

## Flow Recommendation
**Type**: SDD
**Confidence**: high
**Rationale**: CLI interface with precise command parsing and dispatch

## Bubble Up
- 23 CLI commands for device management
- Tab completion support
- Raw AT command passthrough
- Firmware update integration
- SMS/USSD sending from CLI

