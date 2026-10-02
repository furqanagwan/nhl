# Building the games

Until releases are published, each game is built from source against the
ReXGlue SDK. Everything you create here stays in the game's folder, which git
ignores apart from its `README.md` and `.toml`.

## Requirements

- **Your own copy of the game:** the disc listed on its page. Check that its
  title and media IDs match.
- **Windows 11 x64 and a Direct3D 12 GPU.** Only NVIDIA has been tested.
- **The build tools:** Visual Studio 2026 with LLVM Clang, CMake and Ninja, as
  listed in the [SDK README](https://github.com/furqanagwan/rexglue-sdk#requirements).
- **The ReXGlue SDK** from [furqanagwan/rexglue-sdk](https://github.com/furqanagwan/rexglue-sdk),
  built and installed from `main` (`d3e2846` or later: it has the USB camera
  exports this game imports and the title cvar defaults below).
- **Optional:** Microsoft GDK 260404, for the GDK build.
- **Optional:** your console's dashboard system update (`$SystemUpdate`), so
  the Xbox guide is built in; see the SDK's
  [Xbox guide](https://github.com/furqanagwan/rexglue-sdk/blob/main/docs/xbox-guide.md#using-it).

| Game | Folder | Project name | Executable | Configuration |
| --- | --- | --- | --- | --- |
| NHL Legacy Edition | `Legacy Edition/` | `nhllegacy` | `default.xex` | `nhllegacy.toml` |

## 1. Extract the files

Extract your disc image into `Legacy Edition/game/`, for example with
[extract-xiso](https://github.com/XboxDev/extract-xiso). The disc's
`$SystemUpdate` folder is a console update, not game code; it can stay.

## 2. Create the project

```powershell
rexglue init --project-name nhllegacy --xex-path "Legacy Edition/game/default.xex" --game-root "Legacy Edition/game" --project-root "Legacy Edition/recompiled"
```

## 3. Add the game's configuration

In `Legacy Edition/recompiled/nhllegacy_manifest.toml`:

```toml
[entrypoint]
includes = ["../nhllegacy.toml"]
```

In `Legacy Edition/recompiled/CMakeLists.txt`, give the game its default
render target path (its matches draw black on the default RTV path,
[RG-NHL-003](RG-NHL-003.md)):

```cmake
rexglue_setup_target(nhllegacy GPU_PLUGINS xenos
    CVAR_DEFAULTS "render_target_path_d3d12=rov")
```

## 4. Generate and build

```powershell
cd "Legacy Edition/recompiled"
rexglue codegen
cmake -S . -B out/build/release -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_PREFIX_PATH=<ReXGlue install prefix>
cmake --build out/build/release
```

Codegen takes about nine minutes for this executable. For the GDK build, use
the SDK's GDK install prefix (`out/install/win-amd64-gdk`).

## Run it

```powershell
.\out\build\release\nhllegacy.exe --game_data_root="<...>/Legacy Edition/game" --gpu_plugin=xenos
```

Saves go to the per-user location the SDK documents
([data locations](https://github.com/furqanagwan/rexglue-sdk/blob/main/docs/data-locations.md));
`--user_data_root` puts them somewhere else.
