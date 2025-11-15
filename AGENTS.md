# Repository Guidelines

## Project Structure & Module Organization
- Root components live under `src/` (core `game/`, `server/`, `shared/`, `realmd/`) with shared assets in `cmake/`, `dep/`, and `contrib/`.
- Module libraries such as `PlayerBots/` reside under `src/modules/PlayerBots`; enable them via `BUILD_PLAYERBOTS` in the top-level `CMakeLists.txt`.
- Keep configuration files (`sql/`, `.clang-format`, etc.) in place; avoid merging unrelated directories when touching core modules.

## Build, Test, and Development Commands
- Configure: `cmake -S . -B build -DBUILD_PLAYERBOTS=ON -DCMAKE_INSTALL_PREFIX=/your/install` to generate the build tree with the playerbot module.
- Build: `cmake --build build --target mangosd` (or `game`/`realmd` as needed); builds `libgame.a` and the server binaries.
- Install: `cmake --build build --target install` to deploy executables & configs to the install prefix.
- Verification: `ctest --test-dir build` runs any enabled CTest suites after `cmake --build` completes; add tests to `src/tests/` and expose them via `CMakeLists.txt` if needed.

## Coding Style & Naming Conventions
- Follow existing conventions: tabs for indentation, opening brace on the same line, descriptive `PascalCase` class names, and `camelCase` for member functions where already used.
- Keep headers/definitions in their respective folders (e.g., `src/game/` for engine code, `src/modules/` for optional modules); include `PlayerBot` headers only when `BUILD_PLAYERBOTS` is enabled.
- Prefer `const` references and `auto` for iterator acquisition; mirror the surrounding file’s spacing and alignment when modifying.

## Testing Guidelines
- No dedicated unit suite yet, so extend `CTEST_ADD_TEST` in `src/tests/CMakeLists.txt` when adding new checks.
- Name tests descriptively (e.g., `PlayerbotInvite.TestInvitesMaster`) and run via `ctest --test-dir build --output-on-failure` before submitting.

## Commit & Pull Request Guidelines
- Write present-tense, descriptive commit messages (e.g., `Fix bot invite logging before packet dispatch`).
- Reference the issue/bug ID if available and briefly note the affected subsystem in the summary line.
- PRs should explain the change, list commands run (build/test), and mention any manual validation (e.g., `mangosd` + playerbot invite workflow). Include config tweaks if required for testers.
