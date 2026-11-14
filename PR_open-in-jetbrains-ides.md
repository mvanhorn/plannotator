Title: Open in: JetBrains IDEs, at the line

The Open in menu now lists the common IntelliJ-platform IDEs when they are actually installed: IntelliJ IDEA, WebStorm, PyCharm, GoLand, Rider, PhpStorm, RubyMine, CLion, and RustRover. Community Edition bundles are not separate entries, because they share the same Linux/Windows launcher names as Ultimate (`idea`, `pycharm`).

On Linux and Windows the documented CLI is `--line <n> <path>` (JetBrains: [Open files from the command line](https://www.jetbrains.com/help/idea/opening-files-from-command-line.html), and the same page for WebStorm). Toolbox and "Create Command-line Launcher" put `idea`, `webstorm`, `pycharm`, `goland`, `rider`, `phpstorm`, `rubymine`, `clion`, and `rustrover` on PATH (`idea.cmd` on Windows is what `which idea` finds). The catalog has a third `line.cli` mode, `line-flag`, so those binaries get `["--line", String(line), path]`. A missing line, or an editor with no CLI mode, is still just the path.

On macOS we still launch with `open -a "<AppName>"`. JetBrains does not document a URL that fits the existing `<scheme>://file/<path>:<line>` shape, so a JetBrains IDE on macOS opens the file at the top. The Inbox already passes `line` on its Open in POST; review and annotate send it when the control has one.

Availability is unchanged: macOS needs the `.app` bundle, Linux/Windows need the binary on PATH. A click is the only time a launcher runs. Missing `appId` is 400, an unknown id is 400, and an IDE that is not on PATH is not listed (a direct POST still fails with "not found" rather than launching something else).

Related to #3368. That issue also asks for user-defined editors; this change does not add those.

## Tests

- `bun test packages/core/open-in-apps.test.ts packages/server/open-in.test.ts packages/server/inbox-open-in.test.ts packages/inbox/openIn.menu.test.ts apps/pi-extension/open-in-launch.test.ts apps/pi-extension/server/open-in-guards.test.ts packages/server/open-in-guards.test.ts` — 92 pass, 0 fail
- `bun test packages/server packages/core` (plus the Pi/Inbox files above) — 6926 pass, 52 skip, 2 fail: `packages/server/git-background.test.ts` "background HTTP(S) auth does not invoke askpass" on Bun and Pi. Unrelated to Open in; GIT_ASKPASS ran in this environment.
- `bun run typecheck` — pass
- `bun run --cwd apps/review build && bun run build:hook && bun run build:opencode` — pass

Linux only here, with stub `webstorm` / `rider` on a temp PATH against a real review server. macOS (`open -a`) and Windows (Toolbox `.cmd`) launches were not run.

## Demo

![demo](https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1114/open-in-jetbrains-ides/preview.gif)

<video src="https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1114/open-in-jetbrains-ides/out.mp4" controls width="720"></video>
