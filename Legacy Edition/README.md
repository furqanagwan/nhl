<p align="center"><img src="https://raw.githubusercontent.com/xenia-manager/x360db/main/titles/454109EC/artwork/banner.png" alt="EA SPORTS NHL Legacy Edition marketplace banner"></p>

# EA SPORTS NHL Legacy Edition

The 2015 NHL game for Xbox 360, statically recompiled from its executable
into a native Windows PC game with
[ReXGlue](https://github.com/furqanagwan/rexglue-sdk).

The last NHL release on Xbox 360: it brings together the most popular modes
and gameplay features from nine years of EA's NHL series, with the 2015-16
rosters and schedules.

**Status: Investigating.** It recompiles and boots to its first-run screen;
menus beyond it and gameplay haven't been tried yet.

## The game

| | |
| --- | --- |
| Released | 15 September 2015 |
| Developer | EA Canada |
| Publisher | Electronic Arts |
| Genre | Sports (ice hockey) |
| Players | Up to 4 locally; online play needed EA's servers, now closed |
| Rating | ESRB E10+ (Everyone 10+) |

## The release this is built from

| | |
| --- | --- |
| Disc (Redump) | `NHL Legacy Edition (USA, Europe) (En,Fr,De,Sv,Fi,Ru,Cs)`, one disc |
| Region | USA and Europe |
| Languages | English, French, German, Swedish, Finnish, Russian, Czech |
| Title ID | `454109EC` |
| Media ID | `3CF2A23F` |
| Executable | `default.xex` v0.0.0.4 (built 2015-07-17) |
| XEX SHA-256 | `d818eb15775c1fee34b057b0fc494b211535d07123230ee45bd7d40991c55624` |
| Title update | None applied. Title update 1 exists for this disc (on Xbox Unity, 64 MB, 2016). |

Check that your disc's title and media IDs match: the recompiled code is only
valid for this executable.

## What works

Tested on an NVIDIA RTX 5080 Laptop GPU ([RG-NHL-001](../docs/RG-NHL-001.md)).

- **Recompiles without title hints.** The SDK needed its USB camera exports
  built in for it to link.
- **Boots to the first-run screen** ("Welcome to EA SPORTS NHL Legacy
  Edition", the gamer-type choice) at 4K, showing the signed-in gamertag, in
  a 60-second GDK run. The log's only error lines are the `cache:` and
  `update:` drives not being mounted.

**Not validated yet:** the menus past the first-run screen, a match,
saving and loading, the title update, AMD and Intel GPUs, and the standard
(non-GDK) build.

## Configuration

[`nhllegacy.toml`](nhllegacy.toml) is this game's codegen configuration. It
needs no title-specific settings yet, so it only identifies the executable.
Building is described in [docs/building.md](../docs/building.md).

## Sources

- [LaunchBox Games Database](https://gamesdb.launchbox-app.com/games/details/31492-nhl-legacy-edition):
  release date, players, rating. The entry is filed under PlayStation 3; its
  overview covers both consoles.
- [x360db](https://github.com/xenia-manager/x360db/tree/main/titles/454109EC)
  (Xenia Manager's database): developer, marketplace description and the
  banner above.
- The disc and its `default.xex` headers: everything under "The release this
  is built from".
