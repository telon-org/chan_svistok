# Legacy Analysis Review

## Items for Human Review

### No Conflicts Detected

All analysis completed without contradictions to existing documentation (no existing flows found).

### Recommendations for Flow Creation

The following flows are recommended for creation based on code analysis:

#### 1. SDD: AT Command Protocol
**Purpose**: Document the AT command protocol implementation
**Key Specifications**:
- 60+ AT commands enumerated
- Queue-based execution with timeout handling (1s-40s)
- Response matching per command
- Initialization sequences (modem → SIM)

**Files**: `at_command.c/h`, `at_response.c/h`, `at_queue.c/h`

#### 2. SDD: Call Management
**Purpose**: Document the call state machine and channel driver
**Key Specifications**:
- 8 call states (ACTIVE, ONHOLD, DIALING, ALERTING, INCOMING, WAITING, RELEASED, INIT)
- 12 call flags for tracking
- Dial string parsing: `device/options:number`
- Conference via hold+merge (AT+CHLD commands)

**Files**: `channel.c`, `cpvt.c/h`, `channel.h`

#### 3. SDD: Device Communication
**Purpose**: Document serial communication layer
**Key Specifications**:
- 115200 baud, 8N1, hardware flow control
- PID-based device locking (`/var/lock/LOCK..{device}`)
- Ring buffer with vector I/O
- Blocking reads (VMIN=1)

**Files**: `tty_v2.c`, `ringbuffer.c/h`, `select.h`

#### 4. SDD: SMS/USSD Handling
**Purpose**: Document SMS and USSD implementation
**Key Specifications**:
- PDU mode binary SMS format
- UCS-2/7-bit character encoding
- Two-stage send (prompt + data)
- Delivery report support

**Files**: `pdu.c/h`, `char_conv.c/h`, `at_command.c` (SMS functions)

#### 5. SDD: CLI Management
**Purpose**: Document Asterisk CLI interface
**Key Specifications**:
- 23 CLI commands
- Tab completion support
- Raw AT command passthrough
- Firmware update integration

**Files**: `cli.c/h`

#### 6. SDD: Device Programming
**Purpose**: Document firmware flashing mechanism
**Key Specifications**:
- Qualcomm diagnostic mode entry (`AT$QCDMG`)
- HDLC-framed binary protocol (0x7E delimiters)
- State machine: init→diag→update→done
- Progress persistence to filesystem

**Files**: `ttyprog_core.c`, `ttyprog_programmator.c`, `ttyprog_svistok.c`

#### 7. SDD: State Persistence
**Purpose**: Document state storage mechanism
**Key Specifications**:
- File-based KV storage: `/var/svistok/{type}/{item}.{ext}`
- Device state, limits, errors, balance
- Mutex-protected access

**Files**: `share.c`, `share_mysql.c`, `share.h`

### ADR Topics for Discussion

1. **AT Command Queue Architecture** - Why queue-based with timeout matching?
2. **PDU Mode vs Text Mode SMS** - Why PDU mode chosen?
3. **File-Based State Persistence** - Why files over database?
4. **Qualcomm DIAG Protocol** - Why this protocol for firmware?
5. **115200 Baud Serial** - Why this baud rate?
6. **8-State Call Machine** - Why this state design?

## Next Steps

1. Review recommended flows above
2. Approve flow creation priorities
3. Create flows using `/sdd start [name]` command
4. Create ADRs using `/adr start [name]` command

## Review Status

- [ ] Flows reviewed
- [ ] Flow priorities approved
- [ ] ADR topics approved
- [ ] Ready for flow generation
