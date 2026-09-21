# Roadmap

## Phase 0 — Project skeleton

- [x] Define repository architecture
- [x] Define module boundaries
- [x] Create empty RTL, testbench, host, script, and constraint placeholders
- [ ] Select the first FPGA board and video connector

## Phase 1 — Video timing

- [ ] Implement 640×480 @ 60 Hz timing counters
- [ ] Generate horizontal sync, vertical sync, blanking, and active coordinates
- [ ] Produce simulation-only colour bars
- [ ] Add self-checking timing tests

Exit criterion: every line, frame, porch, and sync interval matches the selected timing specification.

## Phase 2 — Framebuffer scanout

- [ ] Infer the 320×240 dual-port framebuffer
- [ ] Implement 2× nearest-neighbour scaling
- [ ] Implement the 256-entry RGB palette
- [ ] Render a deterministic simulated frame

Exit criterion: a known framebuffer pattern produces the expected 640×480 frame.

## Phase 3 — Basic drawing engine

- [ ] Implement clear-screen command
- [ ] Implement bounds-checked pixel writes
- [ ] Implement filled rectangles
- [ ] Add unit and integration tests

Exit criterion: commands produce the same pixels as the Python reference model.

## Phase 4 — Line rasterizer

- [ ] Implement integer Bresenham line drawing
- [ ] Support every octant
- [ ] Add clipping or safe rejection at framebuffer boundaries
- [ ] Compare random lines with a software reference

Exit criterion: all in-range reference pixels match and no command writes outside framebuffer memory.

## Phase 5 — Host command link

- [ ] Implement UART receiver
- [ ] Implement framed command decoder and FIFO
- [ ] Implement Python command utility
- [ ] Add demo-scene generator and status reporting

Exit criterion: a simulated UART command stream renders a complete reference scene.

## Phase 6 — Hardware demonstration

- [ ] Add board-specific clock generation and constraints
- [ ] Build or connect the VGA resistor DAC/interface
- [ ] Verify image stability on a real monitor
- [ ] Measure timing closure, BRAM use, LUT use, and command throughput
- [ ] Add photos, video, utilization, timing, and measured limits

Exit criterion: reproducible monitor output with documented hardware and measurements.

## Stretch goals

- [ ] Filled triangles
- [ ] Sprite blitter and transparent colour key
- [ ] Bitmap font and text console
- [ ] Double buffering
- [ ] External SDRAM framebuffer
- [ ] DVI/HDMI TMDS output on a supported board
- [ ] Fixed-point transforms and a simple 3D wireframe pipeline

