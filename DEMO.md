# Demo: Open in JetBrains IDEs, at the line

Real code review UI against a real `plannotator review` server. `webstorm` and `rider` on PATH are stub launchers (append argv, exit 0). PyCharm and IntelliJ IDEA are not on PATH, so they do not appear in the menu.

## What the recording shows

1. Review of `src/retry.ts` with the change on line 42.
2. Open in menu: Files, WebStorm, Rider (not PyCharm).
3. Choosing WebStorm. The review file header has no selected line, so Playwright adds `line: 42` to the POST (Inbox already sends this field). The real `/api/open-in` handler launches the stub: `webstorm --line 42 /abs/path/src/retry.ts`.
4. Terminal replay of `commands.log` (banner: "Animated replay of real captured command output"): stubs on PATH, `which pycharm`/`idea` fail, GET `/api/open-in/apps`, 400 missing/unknown `appId`, `PyCharm not found`, then the click's POST and stub log.

## How it was captured

Scratch `HOME` and `PLANNOTATOR_DATA_DIR` (not `~/.plannotator`). `PLANNOTATOR_BROWSER=/usr/bin/true`. Stubs first on PATH. `plannotator review` in a throwaway git repo. Playwright Chromium at 1920×1080, dark theme.

Video: HyperFrames composition of the real screenshots plus the exact `commands.log`. GIF derived from the MP4 with ffmpeg palettegen/paletteuse.

## Media

- `out.mp4` — 1920×1080, 30 fps, 720 frames, 24.0 s, H.264 yuv420p
- `preview.gif` — 720×405, 12 fps, 288 frames, 24.0 s, 1.7 MB
- `screenshots/` — the Playwright captures
- `logs/` — server log, apps JSON, stub launches, ffprobe, test/typecheck output
- `commands.log` — exact commands, stdout/stderr, exit codes
