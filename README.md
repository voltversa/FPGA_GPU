# FPGA GPU

An educational, simulation-first 2D graphics processor implemented in VHDL-2008.

> Status: repository skeleton and design plan only. The RTL files are intentionally empty until each milestone is implemented and verified.

This project targets a small command-driven raster engine rather than claiming to be a modern general-purpose GPU. The first hardware goal is a 320×240, 8-bit indexed framebuffer scaled to 640×480 video output.

## Planned first release

- 640×480 @ 60 Hz VGA timing
- 320×240 indexed-colour framebuffer with 2× scaling
- Palette-based RGB output
- Clear-screen, pixel, line, and filled-rectangle commands
- UART command link and Python demo utility
- Dual-port BRAM framebuffer
- Self-checking GHDL testbenches
- Board-specific clock and pin wrappers added only after a board is selected

## Repository layout

```text
rtl/          Synthesizable VHDL graphics pipeline
tb/           Self-checking VHDL testbenches
host/         Python command utility and demo scenes
constraints/  Board-specific constraints added after board selection
docs/         Architecture, command format, and roadmap
scripts/      Local simulation helpers
```

## Design documents

- [Architecture](docs/ARCHITECTURE.md)
- [Roadmap](docs/ROADMAP.md)
- [Command protocol](docs/COMMAND_PROTOCOL.md)

## Proposed defaults

| Parameter | Initial value |
|---|---:|
| Display timing | 640×480 @ 60 Hz |
| Logical framebuffer | 320×240 |
| Pixel format | 8-bit palette index |
| Framebuffer size | 76,800 bytes |
| HDL standard | VHDL-2008 |
| Host link | UART |
| Simulation | GHDL |

The final clock generator, RGB pinout, voltage standard, and achievable command throughput will be documented after the target FPGA board is selected and tested.

