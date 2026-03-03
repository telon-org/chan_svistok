# Understanding: chan_dongle - Asterisk Channel Driver

## Phase: EXPLORING

## Validated Understanding

**Core Purpose**: Channel driver for Asterisk PBX that enables Huawei UMTS dongles (3G modems) to function as telephony channels.

**Key Capabilities**:
- Voice calls (place/terminate) via 3G dongles
- SMS send/receive
- USSD commands/messages
- Multiple dongle management (groups, round-robin, specific device selection)

**Architecture**:
- Asterisk channel driver module (`chan_dongle.c`)
- AT command protocol parser (`at_command.c`, `at_response.c`, `at_parse.c`)
- Device communication layer (`tty_v2.c`, `dserial.c`)
- CLI interface for management (`cli.c`)
- Device programming tools (`ttyprog_*`)

**Integration Points**:
- Asterisk PBX core (channel driver API)
- MySQL database (`share_mysql.c`)
- Serial/TTY device communication
- SIP dialplan integration

## Sources Analyzed
- `README.txt` - Feature documentation, dialplan examples
- `chan_dongle.c` (1967 lines) - Main channel driver implementation
- `app.c` - Asterisk application integration
- `at_command.*`, `at_response.*`, `at_parse.*` - AT command protocol
- `ttyprog_*` - Device programming utilities

## Children (Discovered Domains)
| Child | Status |
|-------|--------|
| at-command-protocol | PENDING |
| device-communication | PENDING |
| call-management | PENDING |
| sms-ussd-handling | PENDING |
| device-programming | PENDING |
| cli-management | PENDING |
| database-integration | PENDING |

## Flow Recommendation
**Type**: SDD (Spec-Driven Development)
**Confidence**: high
**Rationale**: Internal service logic - channel driver implementing hardware protocol. Requires precise specifications for AT commands, call state machines, and device communication.

## Bubble Up
- Asterisk channel driver for Huawei 3G dongles
- AT command-based protocol
- Voice, SMS, USSD support
- Multiple device management
- Device programming capability
