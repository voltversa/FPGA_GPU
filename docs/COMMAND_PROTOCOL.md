# Proposed Command Protocol

This protocol is a design target and is not implemented yet.

## Frame

| Field | Size | Description |
|---|---:|---|
| Start | 1 byte | `0xA5` |
| Opcode | 1 byte | Drawing or control operation |
| Length | 2 bytes | Little-endian payload length |
| Payload | Variable | Coordinates, dimensions, and colour data |
| CRC | 1 byte | CRC-8 over opcode, length, and payload |

## Planned operations

| Operation | Payload concept | Purpose |
|---|---|---|
| `GET_INFO` | None | Read resolution, version, and supported operations |
| `CLEAR` | Colour | Fill the full framebuffer |
| `PIXEL` | X, Y, colour | Draw one bounded pixel |
| `RECT` | X, Y, width, height, colour | Draw a filled rectangle |
| `LINE` | X0, Y0, X1, Y1, colour | Draw a Bresenham line |
| `SET_PALETTE` | Index, RGB | Update one palette entry |
| `GET_STATUS` | None | Read FIFO, busy, and error state |

Numeric opcodes and field widths will be frozen alongside executable decoder and host reference tests.

