# Understanding: Database Integration

## Phase: EXPLORING

## Validated Understanding

**Core Purpose**: State persistence layer using FILE-based storage (NOT actual MySQL despite filename).

**Architecture**:

1. **File-Based Storage** (`share.c`, `share_mysql.c`):
   - Path: `/var/svistok/{devtype}/{fileitem}.{filetype}`
   - Example: `/var/svistok/imsi/250000000.balance`
   - Operations: `getfilei`, `putfilei`, `getfiles`, `putfiles` (integer/string)

2. **Stored Data Types**:
   - **Device Info** (`readpvtinfo`, `writepvtinfo`):
     - IMSI, IMEI, model, firmware
     - Provider name, balance
     - Signal quality (RSSI)
   
   - **Device Limits** (`readpvtlimits`, `writepvtlimits`):
     - Call limits per device
     - Balance thresholds
   
   - **Device State** (`writepvtstate`):
     - Current call state
     - AT queue state
   
   - **Device Errors** (`readpvterrors`, `writepvterrors`):
     - Error counters
     - Last error info

3. **State Persistence API** (`share.h`):
   ```c
   int getfilei(char* devtype, char* fileitem, char* filetype, int *value);
   int putfilei(char* devtype, char* fileitem, char* filetype, int value);
   int getfiles(char* devtype, char* fileitem, char* filetype, char *value);
   int putfiles(char* devtype, char* fileitem, char* filetype, char* value);
   ```

4. **Mutex Management** (`share.h`):
   - `ast_mutex_lock_pvt()` - Lock device private data
   - `ast_mutex_unlock_pvt()` - Unlock
   - `mutex_trylock_pvt_e()` - Try lock with debug info

**Key Patterns**:
- File-based key-value storage
- Device identified by IMSI or device name
- Automatic file creation on write
- Text format for all data types

**Note**: Despite `share_mysql.c` filename and MySQL includes, actual MySQL queries are NOT used - it's file I/O wrapper.

## Sources Analyzed
- `share.h` - Storage API
- `share_mysql.c` (590 lines) - File-based implementation
- `share.c` - Core storage functions

## Children (Discovered)
| Child | Status |
|-------|--------|
| file-storage | COMPLETE |
| state-persistence | COMPLETE |
| mutex-management | COMPLETE |

## Flow Recommendation
**Type**: SDD
**Confidence**: high
**Rationale**: State persistence with precise file format specifications

## Bubble Up
- File-based KV storage: `/var/svistok/{type}/{item}.{ext}`
- Device state persistence
- Balance/limits tracking
- Mutex-protected access
