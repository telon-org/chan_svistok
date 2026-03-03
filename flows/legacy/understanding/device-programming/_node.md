# Understanding: Device Programming

## Phase: EXPLORING

## Validated Understanding

**Core Purpose**: Firmware flashing tool for Huawei dongles via diagnostic mode.

**Architecture**:

1. **Diagnostic Mode Entry** (`ttyprog_core.c`):
   - Sends `AT$QCDMG` to enter Qualcomm diagnostic mode
   - Receives `$QCDMG...OK` response
   - Sends `AT^DISLOG=255` to enable diagnostic logging
   - Binary protocol with HDLC-like framing (0x7E delimiters)

2. **Programming Sequence** (`ttyprog_programmator.c`):
   ```
   1. Open TTY port (opentty)
   2. Enter diagnostic mode (ttyprog_set_diagmode)
   3. Wait 25 seconds for mode switch
   4. Send firmware file (ttyprog_sendfile)
   5. Close port
   ```

3. **State Persistence**:
   - Saves state to `/var/simnode/{device}.{param}`
   - States: `init` → `diag` → `wait` → `update` → `done`
   - Progress: 0 → 1 → 10 → 20 → 100 (percentage)

4. **Binary Protocol** (from `ttyprog_core.c`):
   - `cmd0/res0` - Enter diagnostic mode (`AT$QCDMG`)
   - `cmd1/res1` - Enable diag logging (`AT^DISLOG=255`)
   - `cmd5/res5` - Query firmware version (returns `E1550FCD6ATCPU Ver.A`)
   - `cmd8/res8` - Enter programming mode (restart)
   - Frame format: `7E ... 7E` (HDLC-style)

5. **File Structure**:
   - `ttyprog_core.c` (305 lines) - Protocol sequences
   - `ttyprog_programmator.c` - Standalone programmer
   - `ttyprog_svistok.c` - Asterisk-integrated version
   - `ttyprog_test.c` - Testing
   - `programmator/` - Standalone binary + scripts

**Key Patterns**:
- HDLC framing with 0x7E delimiters
- State machine for firmware update
- Progress tracking via file system
- Two-stage: diagnostic → programming mode

## Sources Analyzed
- `ttyprog_core.c` (305 lines) - Protocol implementation
- `ttyprog_programmator.c` - Standalone programmer
- `ttyprog_svistok.c` - Asterisk integration

## Children (Discovered)
| Child | Status |
|-------|--------|
| diagnostic-mode | COMPLETE |
| firmware-transfer | PENDING |
| progress-tracking | COMPLETE |

## Flow Recommendation
**Type**: SDD
**Confidence**: high
**Rationale**: Device programming with precise binary protocol

## Bubble Up
- Qualcomm diagnostic mode entry
- HDLC-framed binary protocol
- State machine: init→diag→update→done
- Progress persistence to filesystem
