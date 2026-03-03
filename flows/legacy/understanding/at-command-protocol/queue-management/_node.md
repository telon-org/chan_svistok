# Understanding: Queue Management

## Phase: EXPLORING

## Validated Understanding

**Core Architecture**:
- Linked list queue (`AST_LIST`) of tasks
- Each task contains multiple commands (`at_queue_cmd_t cmds[0]`)
- Commands executed sequentially, task removed when all commands complete

**Key Operations**:

1. **Add Task** (`at_queue_add`):
   - Allocates task + commands array
   - Copies commands to task
   - Inserts at tail (or after head if priority)
   - Tracks statistics (at_tasks, at_cmds)

2. **Execute** (`at_queue_run`):
   - Gets head command from queue
   - Writes to device FD via `at_write()`
   - Sets timeout: `cmd->timeout = ast_tvadd(ast_tvnow(), cmd->timeout)`
   - Frees command data after write (marks as "in flight")
   - Error handling: removes command on write failure

3. **Complete Command** (`at_queue_remove_cmd`):
   - Called when response received
   - Increments command index within task
   - Removes task if: all commands done OR response mismatch (unless IGNORE flag)

4. **Memory Management**:
   - `at_queue_free_data`: Only frees if not STATIC flag
   - `at_queue_free`: Frees entire task
   - `at_queue_remove`: Removes and frees task from queue

**Timeout Handling**:
- Timeout set AFTER write: `cmd->timeout = ast_tvadd(ast_tvnow(), cmd->timeout)`
- Timeout values: 1s, 2s, 5s, 10s, 15s, 40s (from header)
- Expired commands logged and removed (code commented out in current version)

**Flags**:
- `STATIC` (0x01): Data is static, don't free
- `IGNORE` (0x02): Ignore response mismatch

## Sources Analyzed
- `at_queue.c` (412 lines) - Full implementation

## Children (Discovered)
| Child | Status |
|-------|--------|
| timeout-handling | PENDING |
| response-matching | PENDING |
| memory-management | PENDING |

## Flow Recommendation
**Type**: SDD
**Confidence**: high
**Rationale**: Queue management with precise timeout/retry semantics

## Bubble Up
- Task-based command grouping
- Sequential execution with response matching
- Timeout tracking per command
- Memory management via flags
