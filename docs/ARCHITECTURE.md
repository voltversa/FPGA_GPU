# Architecture

## Objective

Build a compact 2D FPGA graphics processor that accepts drawing commands, rasterizes them into block RAM, and continuously scans the framebuffer to a monitor.

## Planned pipeline

```mermaid
flowchart LR
    H[Host commands] --> Q[Command FIFO]
    Q --> R[Raster engine]
    R --> F[Dual-port framebuffer]
    F --> P[Palette and scaler]
    P --> V[Video output]
```

The write side and scanout side share the framebuffer through separate RAM ports. This keeps display timing independent from command execution.

## Module responsibilities

| Module | Responsibility |
|---|---|
| `gpu_pkg` | Shared types, opcodes, coordinates, colours, and timing constants |
| `clock_reset` | Board-wrapper boundary for system and pixel clocks |
| `video_timing` | Horizontal/vertical counters, sync pulses, blanking, and active coordinates |
| `pixel_scaler` | Maps each 320×240 logical pixel to a 2×2 output area |
| `palette` | Converts an 8-bit colour index into RGB output |
| `framebuffer_dp` | Infers true dual-port block RAM for write and scanout access |
| `pixel_writer` | Performs bounds-checked single-pixel writes |
| `line_rasterizer` | Generates pixels using an integer Bresenham algorithm |
| `rect_rasterizer` | Generates filled axis-aligned rectangles |
| `triangle_rasterizer` | Stretch-goal filled-triangle engine after the 2D baseline is stable |
| `command_fifo` | Buffers decoded host commands |
| `command_processor` | Dispatches drawing operations and reports busy/ready state |
| `uart_rx` | Receives command bytes from the host |
| `fpga_gpu_top` | Integrates board-neutral graphics logic |

## Clock-domain plan

- The initial design uses a system clock and a pixel clock.
- A board-specific PLL/MMCM wrapper will generate the pixel clock when hardware is selected.
- Command data crosses into the raster domain through a FIFO if the two clocks differ.
- Framebuffer port A serves the raster engine; port B serves video scanout.
- Resets are released synchronously in each clock domain.

## Framebuffer plan

- Logical resolution: 320×240 pixels.
- Pixel storage: one 8-bit palette index per pixel.
- Required memory: 614,400 bits (76,800 bytes), before implementation overhead.
- Output scaling: nearest-neighbour 2× scaling to 640×480.
- Initial version: single buffering with write/read arbitration handled by dual-port BRAM.
- Stretch goal: optional double buffering on devices with enough block RAM or external memory.

## Command execution

1. The host sends a framed command over UART.
2. The decoder verifies the frame and pushes a command into the FIFO.
3. The command processor waits until the selected raster unit is available.
4. The raster unit emits bounded pixel writes.
5. The command processor returns completion or error status.

## Verification strategy

- Verify exact VGA counter and sync-pulse boundaries.
- Test framebuffer read/write behaviour and collision assumptions.
- Compare line pixels against a software Bresenham reference.
- Test rectangles at normal, zero-size, clipped, and out-of-range coordinates.
- Render deterministic simulation frames and compare them with reference images.
- Test sustained host commands and FIFO back-pressure.

## Hardware boundary

No clock pin, RGB pin, I/O voltage, PLL primitive, or connector is assumed yet. The board wrapper and constraints will be added only when the exact FPGA board and display interface are known. HDMI/DVI output is a later option, not part of the initial VGA milestone.

