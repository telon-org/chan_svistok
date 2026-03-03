# Understanding: chan_dongle - Asterisk Channel Driver

## Phase: COMPLETE

## Final Validated Understanding

**Project**: chan_dongle - Asterisk channel driver for Huawei UMTS 3G dongles

**Core Functionality**:
1. **Voice Calls** - Full duplex audio via 3G dongles
2. **SMS** - Send/receive with PDU mode and UCS-2 encoding
3. **USSD** - Interactive service codes
4. **Device Management** - Multi-device support with groups, load balancing
5. **Firmware Flashing** - Qualcomm diagnostic mode programming

**Architecture Overview**:
```
┌─────────────────────────────────────────────────────────────┐
│                    Asterisk PBX Core                        │
├─────────────────────────────────────────────────────────────┤
│ chan_dongle Channel Driver                                  │
│  ┌─────────────┐ ┌──────────────┐ ┌──────────────┐         │
│  │ Channel Mgmt│ │ CLI Interface│ │ Manager API  │         │
│  │ (channel.c) │ │   (cli.c)    │ │ (manager.c)  │         │
│  └─────────────┘ └──────────────┘ └──────────────┘         │
│  ┌──────────────────────────────────────────────┐           │
│  │           AT Command Protocol Layer          │           │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────────┐ │           │
│  │  │ Commands │ │ Responses│ │    Queue     │ │           │
│  │  │(at_cmd.c)│ │(at_resp.c)│ │ (at_queue.c) │ │           │
│  │  └──────────┘ └──────────┘ └──────────────┘ │           │
│  └──────────────────────────────────────────────┘           │
│  ┌──────────────────┐ ┌────────────────────────────┐        │
│  │ Device Comm (TTY)│ │ State Persistence (Files)  │        │
│  │  (tty_v2.c)      │ │      (share.c)             │        │
│  └──────────────────┘ └────────────────────────────┘        │
├─────────────────────────────────────────────────────────────┤
│  Firmware Flasher (ttyprog_*.c) - Qualcomm DIAG protocol    │
└─────────────────────────────────────────────────────────────┘
                          │
                    ┌─────┴─────┐
                    │ Huawei    │
                    │ UMTS      │
                    │ Dongle    │
                    └───────────┘
```

**Domain Breakdown**:

| Domain | Files | Purpose |
|--------|-------|---------|
| AT Command Protocol | at_command.c, at_response.c, at_queue.c | 60+ AT commands, queue with timeouts |
| Device Communication | tty_v2.c, ringbuffer.h | 115200 baud serial, HDLC framing |
| Call Management | channel.c, cpvt.h | 8-state call machine, Asterisk channel |
| SMS/USSD Handling | pdu.c, char_conv.c | PDU encoding, UCS-2/7-bit conversion |
| Device Programming | ttyprog_*.c | Qualcomm DIAG mode firmware flashing |
| CLI Management | cli.c | 23 Asterisk CLI commands |
| State Persistence | share.c, share_mysql.c | File-based KV storage |

**Key Technical Details**:
- Serial: 115200 baud, 8N1, hardware flow control (CRTSCTS)
- SMS: PDU mode, UCS-2 encoding, 140-octet limit
- Calls: Multi-party support, hold/conference via AT+CHLD
- Queue: Command timeouts 1s-40s, response matching
- Firmware: HDLC-framed binary protocol (0x7E delimiters)

## Flow Recommendations

| Domain | Type | Confidence | Rationale |
|--------|------|------------|-----------|
| AT Command Protocol | SDD | high | Internal protocol with precise specs |
| Device Communication | SDD | high | Serial protocol implementation |
| Call Management | SDD | high | Channel driver state machine |
| SMS/USSD Handling | SDD | high | PDU formatting, encoding specs |
| Device Programming | SDD | high | Binary protocol specifications |
| CLI Management | SDD | high | Command parsing, dispatch logic |
| State Persistence | SDD | high | File format specifications |

## ADR Candidates

| Decision | Type | Status |
|----------|------|--------|
| AT command queue architecture | enabling | draft |
| PDU mode vs text mode SMS | constraining | draft |
| File-based state persistence | enabling | draft |
| Qualcomm DIAG protocol for flashing | enabling | draft |

## Sources Analyzed
- 30+ C source files (~15,000 lines total)
- Header files defining data structures
- README.txt - Feature documentation

## Children (Explored)
| Child | Status |
|-------|--------|
| at-command-protocol | COMPLETE |
| device-communication | COMPLETE |
| call-management | COMPLETE |
| sms-ussd-handling | COMPLETE |
| device-programming | COMPLETE |
| cli-management | COMPLETE |
| database-integration | COMPLETE |

## Bubble Up
- Asterisk channel driver for Huawei 3G dongles
- Voice, SMS, USSD via AT command protocol
- Queue-based command execution with timeouts
- File-based state persistence
- Firmware flashing via Qualcomm DIAG protocol
