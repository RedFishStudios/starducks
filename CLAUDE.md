# Starducks — Roblox game

Roblox experience built with AI help. Code lives in this repo as Luau files. **Lync** syncs it into Roblox Studio.

## Layout
- `src/server/` → `ServerScriptService.Server` (server scripts and modules)
- `src/client/` → `StarterPlayer.StarterPlayerScripts.Client`
- `src/shared/` → `ReplicatedStorage.Shared` (modules both sides use)
- `default.project.json` maps files to instances (Lync format; needs `port`, `experienceId`, `placeId`, `tree`).
- The Studio place file is `../Place1.rbxl`, outside the repo. It is not tracked.

## File naming (Lync)
- All code files are `Name.luau`. Lync picks the script type from a directive on line 1, not from the file name (`.server.luau` / `.client.luau` are rejected):
  - `--@script:server` → Script (server), `--@script:client` → Script with Client run context, `--@localscript` → LocalScript, no directive → ModuleScript.
  - `--@disabled` on its own line disables a script.
- A folder becomes a Folder.
- Use `.luau`, tabs, and double quotes. Run `stylua src` and `selene src` before committing.

## Tooling
- `rokit.toml` pins lync, selene, stylua and luau-lsp. After cloning, run `rokit install`. On this machine Smart App Control blocks `rokit install`'s relink step, but the tools are already installed and work.
- Sync: run `lync serve` in the repo root (port 34873), then click Connect in the Lync plugin in Studio. Lync also writes `sourcemap.json` (gitignored) for luau-lsp.
- GitHub: the remote is `RedFishStudios/starducks`. `gh` is logged in. The user also uses GitHub Desktop.

## Roblox Studio MCP
- `.mcp.json` registers `roblox-studio`, which is Studio's built-in MCP server (`%LOCALAPPDATA%\Roblox\mcp.bat`). Studio must be open with MCP turned on.
- Use it to inspect the live DataModel (`search_game_tree`, `inspect_instance`), run Luau (`execute_luau`), playtest (`start_stop_play`, `get_console_output`), and take screenshots (`screen_capture`).
- **Source of truth for scripts is this repo.** Edit the files here and let Lync sync them. Do not edit synced scripts through MCP, because Lync would overwrite those edits. Use MCP for building and parts, testing, and inspecting. Save the place with `save_place` after world edits.
