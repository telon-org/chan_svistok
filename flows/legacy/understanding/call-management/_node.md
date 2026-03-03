# Understanding: Call Management

## Phase: EXPLORING

## Validated Understanding

**Core Architecture**:
- Asterisk channel driver interface (`channel_tech`)
- Call state machine with 8 states
- Per-call private data (`cpvt_t` - Call Private Data)

**Call States** (`call_state_t`):
1. `CALL_STATE_ACTIVE` - Active call (from CLCC)
2. `CALL_STATE_ONHOLD` - Held call
3. `CALL_STATE_DIALING` - Dialing
4. `CALL_STATE_ALERTING` - Ringing (alerting)
5. `CALL_STATE_INCOMING` - Incoming call
6. `CALL_STATE_WAITING` - Call waiting
7. `CALL_STATE_RELEASED` - Call ended
8. `CALL_STATE_INIT` - Call being initialized

**Call Flags** (`call_flag_t`):
- `CALL_FLAG_HOLD_OTHER` - Hold other calls when dialing
- `CALL_FLAG_NEED_HANGUP` - Must issue hangup command
- `CALL_FLAG_ACTIVATED` - FD attached to channel
- `CALL_FLAG_ALIVE` - Still in CLCC list
- `CALL_FLAG_CONFERENCE` - Begin conference after activate
- `CALL_FLAG_MASTER` - Master channel (owns audio FD)
- `CALL_FLAG_BRIDGE_LOOP` - Detected bridge loop
- `CALL_FLAG_MULTIPARTY` - Part of conference call

**Call Structure** (`cpvt_t`):
- Linked list entry
- Pointer to Asterisk channel
- Pointer to device (`pvt`)
- Call index (0-31)
- State and flags
- Direction (incoming/outgoing)
- Mix buffer for audio mixing
- Audio read/write buffers

**Key Operations** (`channel.c` - 1571 lines):
1. `channel_request()` - Create new channel from dialplan
   - Parse dial string: `Dongle/device:number`
   - Find available device
   - Allocate channel in `CALL_STATE_INIT`

2. `channel_call()` - Execute outbound call
   - Validate channel state
   - Check device readiness (`ready4voice_call()`)
   - Queue dial command (`ATD<number>`)

3. Dial string parsing:
   - Format: `device/options:number`
   - Options: `holdother`, `conference`
   - Validation: only `0123456789*#+ABC` allowed

**Integration Points**:
- Asterisk channel tech interface
- AT command queue for device control
- Mix buffer for multi-party calls
- Manager events for call state changes

## Sources Analyzed
- `channel.c` (1571 lines) - Channel driver implementation
- `channel.h` - Channel API
- `cpvt.h` - Call private data structures

## Children (Discovered)
| Child | Status |
|-------|--------|
| call-states | COMPLETE |
| dial-string-parsing | COMPLETE |
| hold-conference | PENDING |
| bridge-detection | PENDING |

## Flow Recommendation
**Type**: SDD
**Confidence**: high
**Rationale**: Channel driver with precise state machine and call handling logic

## Bubble Up
- 8-state call machine
- 12 call flags for tracking
- Dial string: `device/options:number`
- Conference via hold+merge
- Bridge loop detection
