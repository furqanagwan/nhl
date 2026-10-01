# RG-NHL-001: First recompile of NHL Legacy Edition

| | |
| --- | --- |
| Disc | `NHL Legacy Edition (USA, Europe) (En,Fr,De,Sv,Fi,Ru,Cs)` |
| Title / media | `454109EC` / `3CF2A23F` |
| Executable | `default.xex` v0.0.0.4, built 2015-07-17 04:32:57 UTC |
| XEX SHA-256 | `d818eb15775c1fee34b057b0fc494b211535d07123230ee45bd7d40991c55624` |
| SDK | furqanagwan/rexglue-sdk `main` `9b4485a`, GDK Release |
| Hardware | NVIDIA RTX 5080 Laptop GPU, Windows 11 |
| Date | 2026-10-01 |

- **Extraction:** the image's game partition is at byte `0x2080000` (XGD3);
  171 files, 6,450,973,987 bytes. The only executable is `default.xex`; the
  `$SystemUpdate` folder on the disc is a console update.
- **Codegen:** `rexglue init` and `rexglue codegen` with no title hints, 515
  seconds, 725 files. The CRT's `setjmp` (`0x830801C0`) and `longjmp`
  (`0x8307E1B0`) were recognised. One function, `sub_8301F830`, generates
  2.26 MB of C++ and gets a file of its own (a normal prologue and about
  1,900 labels; not a misdetected range).
- **Link:** failed on `XUsbcamSetConfig` and `XUsbcamGetState`: the SDK had
  never built its USB camera exports. Fixed in rexglue-sdk#160.
- **Run:** a 60-second GDK run with a throwaway user folder reached the
  first-run screen ("Welcome to EA SPORTS NHL Legacy Edition", the gamer-type
  choice) at 3840 x 2160, with the signed-in gamertag. The log has 7 error
  lines, all `cache:` or `update:` not mounted; the GPU warns repeatedly about
  a texture fetch constant of type "invalid".

Not checked: input on the first-run screen, the menus past it, a match,
saves, the title update (version 1 on Xbox Unity), AMD and Intel GPUs.
