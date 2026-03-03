# Understanding: Command Construction

## Phase: EXPLORING

## Validated Understanding

**Core Architecture**:
- Static command templates for common AT commands
- Dynamic command formatting via `at_fill_generic_cmd()` (printf-style)
- Initialization sequences split into modem and SIM phases

**Command Templates** (static const strings):
- `cmd_at` = "AT\r"
- `cmd_chld1x` = "AT+CHLD=1%d\r" (hold/release call by index)
- `cmd_ddsetex2` = "AT^DDSETEX=2\r" (data service settings)
- `cmd17` = "AT^CVOICE?\r" (voice mode query)
- `cmd21` = "AT+CSCS=\"UCS2\"\r" (text encoding)
- `cmd81` = "AT+CMGF=0\r\r" (PDU mode for SMS)

**Initialization Sequences**:

1. **Modem Init** (`at_enque_initialization_modem`):
   - AT (ping)
   - ATE0 (disable echo)
   - AT+CGMI (manufacturer info)
   - AT+CGMM (model name)
   - AT+CGMR (software version)
   - AT+CMEE=0 (error reporting)
   - AT+CGSN (IMEI)
   - AT+CFUN? (functionality check)

2. **SIM Init** (`at_enque_initialization_sim_e` / `sim_mb`):
   - AT^SN (serial number)
   - AT^ICCID? (SIM ICCID)
   - AT^SPN=0 (operator name)
   - AT+CIMI (IMSI)
   - AT+CREG=2 / AT+CREG? (registration status)
   - AT+CNUM (subscriber number)
   - AT+CSSN=1,1 (supplementary service notifications)
   - AT+CMGF=0 (SMS PDU mode)
   - AT+CSCS="UCS2" (UCS-2 encoding)
   - AT+CNMI=1,1,0,1,0 (new SMS notifications)
   - AT+CSQ (signal quality)

**Device-Specific Paths**:
- MULTIBAND devices use `at_enque_initialization_sim_mb` (simplified sequence)
- Other devices use `at_enque_initialization_sim_e` (full sequence)

**User Command Execution** (`at_enque_cmd_proc`):
- Takes raw command string from user
- Appends \r terminator
- Executes as CMD_USER type

## Sources Analyzed
- `at_command.c` (1214 lines) - Command construction implementation

## Children (Discovered)
| Child | Status |
|-------|--------|
| initialization-sequence | COMPLETE (covered here) |
| dial-sms-ussd | PENDING |

## Flow Recommendation
**Type**: SDD
**Confidence**: high

## Bubble Up
- Static command templates
- Two-phase initialization (modem → SIM)
- Device-specific init paths (MULTIBAND vs standard)
- UCS-2 encoding for SMS/USSD
