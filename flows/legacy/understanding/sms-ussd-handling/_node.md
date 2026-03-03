# Understanding: SMS/USSD Handling

## Phase: EXPLORING

## Validated Understanding

**SMS Architecture**:

1. **PDU Mode** (`pdu.c/h`):
   - `pdu_build()` - Builds SMS PDU with SCA, destination, message, validity
   - `pdu_parse()` - Parses received PDU to extract sender and message
   - `pdu_parse_sca()` - Extracts Service Center Address
   - PDU format: hex-encoded binary SMS

2. **Character Encoding** (`char_conv.c/h`):
   - `STR_ENCODING_7BIT_HEX` - GSM 7-bit default alphabet (hex encoded)
   - `STR_ENCODING_8BIT_HEX` - 8-bit data (hex encoded)
   - `STR_ENCODING_UCS2_HEX` - UCS-2 for international characters (hex encoded)
   - `STR_ENCODING_7BIT` - Plain 7-bit (no UTF-8 conversion needed)
   - `str_recode()` - Bidirectional conversion (encode/decode)

3. **SMS Sending** (`at_enque_sms`, `at_enque_pdu`):
   - Two-stage command sequence:
     1. `AT+CMGS=<length>` - Wait for SMS prompt (`>`)
     2. PDU data + Ctrl-Z (0x1A) - Wait for OK
   - Timeout: 2s for command, 40s for message send
   - Validity period: default 3 days (4320 minutes)
   - Delivery report support (`report_req`)

4. **USSD Handling** (`at_enque_ussd`):
   - Command: `AT+CUSD=1,"<code>",15`
   - Encoding: 7-bit hex or UCS-2 hex based on device config
   - Special handling: codes starting with `=` sent raw
   - State tracking: `pvt->outgoing_ussd` flag

**Key Data Structures**:
- PDU buffer: 2048 bytes max
- SMS service center (`pvt->sms_scenter`)
- Encoding detection: `get_encoding()`

**Integration Points**:
- `helpers.c` - `send_sms()` function queues USSD/SMS
- State persistence via `putfilei("sim/state", ...)`

## Sources Analyzed
- `pdu.h` - PDU API
- `char_conv.h` - Character encoding API
- `at_command.c` - SMS/USSD enqueue implementations

## Children (Discovered)
| Child | Status |
|-------|--------|
| pdu-encoding | COMPLETE |
| character-conversion | COMPLETE |
| sms-notification | PENDING |
| ussd-handling | COMPLETE |

## Flow Recommendation
**Type**: SDD
**Confidence**: high
**Rationale**: Protocol implementation with precise PDU formatting and encoding

## Bubble Up
- PDU mode SMS (binary format)
- UCS-2/7-bit encoding conversion
- Two-stage send (prompt + data)
- Delivery report support
