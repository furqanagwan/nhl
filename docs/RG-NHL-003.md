# RG-NHL-003: Matches draw black

| | |
| --- | --- |
| Executable | `default.xex` v0.0.0.4, SHA-256 `d818eb15775c1fee34b057b0fc494b211535d07123230ee45bd7d40991c55624` |
| SDK | furqanagwan/rexglue-sdk `main` `d3e2846`, GDK Release |
| Hardware | NVIDIA RTX 5080 Laptop GPU, Windows 11 |
| Date | 2026-10-02 |

**Problem:** the menus drew, but a match was black apart from the HUD (the
scoreboard and clock ran). Reproduced from a fresh profile: Start, then A
through the first-run screens into a match.

**What was tried:**
- The log had 26,635 "texture fetch constant has invalid type" warnings in
  four minutes, each skipping a draw. The SDK now allows them by default, as
  Xenia Canary and Edge do. The warnings are gone; the match was still black.
- The game asks for the console's `cache:` partition. The SDK now mounts it
  (as Canary does), and the game keeps `809284.ver` and `highlights.sav` there.
  The match was still black.
- **On the ROV render target path the match draws fully**: rink, players,
  crowd and HUD. With the default RTV path it stays black whatever else is
  changed. Tried on RTV: occlusion queries faked, 2x MSAA off, float24 depth
  conversion and rounding, gamma as unorm16 off, stencil value output off.

**Fix here:** the project's `CMakeLists.txt` passes
`CVAR_DEFAULTS "render_target_path_d3d12=rov"` to `rexglue_setup_target`
(see [building.md](building.md)). That is the game's default; a config file or
`--render_target_path_d3d12` still overrides it. The RTV cause is
rexglue-sdk#182.

**Result:** a match (Montreal at Toronto) draws fully and holds 60 fps once
loaded at 3840 x 2160; loading reaches 30-50 fps.

Not checked: a full match, saves, the RTV fix itself, AMD and Intel GPUs.
