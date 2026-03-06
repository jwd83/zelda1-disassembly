# Repository Guidelines

## Project Structure & Module Organization
`src/` contains the disassembly source. Banked code lives in `src/Z_00.asm` through `src/Z_07.asm`, linker configuration is in `src/Z.cfg`, shared symbols/macros are in `*.inc`, and extracted binary assets live under `src/dat/`. Top-level scripts handle the build: `build.ps1` assembles and links the ROM, and `util.ps1` provides file join/compare helpers. Generated outputs belong in ignored directories such as `bin/`, `obj/`, and `ext/`.

## Build, Test, and Development Commands
Use PowerShell from the repository root.

```powershell
./build.ps1
```

Builds `bin/Z.nes`, extracting required `.dat` assets from `ext/Original.nes`, assembling all banks with `ca65`, linking with `ld65`, and verifying the final ROM matches the original.

```powershell
./build.ps1 -NoVerify
```

Skips final ROM comparison when you only need a build artifact.

```powershell
./build.ps1 -NoExtract
```

Reuses existing extracted assets in `bin/` instead of regenerating them.

Place `ca65.exe`, `ld65.exe`, and the verified `(U) (PRG0) [!]` `Original.nes` in `ext/` before building.

## Coding Style & Naming Conventions
Match the existing ca65 style exactly. Keep directives uppercase (`.SEGMENT`, `.BYTE`, `.INCLUDE`), use uppercase hex literals (`$BF50`), and indent statements under labels consistently. Preserve bank-oriented filenames (`Z_00.asm`) and descriptive PascalCase labels such as `SongHeaderOverworld0`. New extracted asset references should follow the existing `src/dat/*.dat` and `*Addrs.inc` naming patterns.

## Testing Guidelines
There is no separate unit test suite; `./build.ps1` is the main validation path. Treat a clean verified build as the minimum acceptance bar. If you change extracted data or linker layout, run the full verified build instead of `-NoVerify` and note the result in your PR.

## Commit & Pull Request Guidelines
Recent commits use short, imperative subjects, often with a scope prefix, for example `Format: Indent .INCLUDE and .INCBIN...`. Follow that pattern and keep each commit focused on one logical change. Pull requests should describe the affected bank/files, explain any ROM-matching impact, and include the exact verification command you ran. Add screenshots only when the change is validated in an emulator such as Mesen.

## Configuration Tips
Do not commit generated ROMs, object files, or extracted binaries; `.gitignore` already excludes them. Keep `ext/` local, and avoid replacing `OriginalNesHeader.bin` unless you are intentionally updating build inputs.
