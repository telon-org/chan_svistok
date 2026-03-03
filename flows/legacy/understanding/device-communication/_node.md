# Understanding: Device Communication

## Phase: EXPLORING

## Validated Understanding

**Core Components**:

1. **TTY Layer** (`tty_v2.c` - 221 lines):
   - `opentty()` - Opens serial device with configuration:
     - B115200 baud rate
     - 8-bit characters (CS8)
     - Hardware flow control (CRTSCTS)
     - Raw mode (no input/output processing)
     - VMIN=1, VTIME=0 (blocking read, 1 char min)
   - `lock_try()` / `lock_create()` - PID-based device locking via `/var/lock/LOCK..{device}`
   - `closetty()` - Closes device, removes lock

2. **Ring Buffer** (`ringbuffer.h`):
   - Circular buffer for async data streaming
   - Operations: `rb_read_iov`, `rb_write_iov` (scatter/gather I/O)
   - `rb_read_until_char_iov` - Read until delimiter (for line-based protocols)
   - `rb_memcmp` - Compare buffer content
   - Used with `struct iovec` for efficient vector I/O

3. **I/O Multiplexing** (`select.h`):
   - `pvt_select` structure for device selection
   - `call_request` structure for call routing decisions
   - `pvt_select_stat()` - Statistics for device selection

**Key Patterns**:
- Device locking prevents concurrent access
- Ring buffer handles async data from serial port
- Vector I/O (iovec) for efficient buffer operations
- Blocking reads with VMIN=1

## Sources Analyzed
- `tty_v2.c` (221 lines) - Serial port operations
- `ringbuffer.h` - Circular buffer API
- `select.h` - Device selection structures

## Children (Discovered)
| Child | Status |
|-------|--------|
| serial-config | COMPLETE |
| io-multiplexing | PENDING |
| buffering | COMPLETE |

## Flow Recommendation
**Type**: SDD
**Confidence**: high

## Bubble Up
- 115200 baud, 8N1, hardware flow control
- PID-based device locking
- Ring buffer with vector I/O
- Blocking reads (VMIN=1)
