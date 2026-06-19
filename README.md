# electron-baker

Bakes a single HTML file into desktop apps (`.dmg`, `.exe`, `.AppImage`) using
GitHub Actions runners — one real runner per OS, no cross-compilation tricks.

## How it works

`main` holds a static Electron template. The local baker server (see the
`server/` project) pushes each job to a `build/<id>` branch with the user's HTML
dropped into `app/index.html`. The workflow in `.github/workflows/build.yml`
then:

1. Builds on `macos-latest`, `windows-latest`, and `ubuntu-latest` in parallel.
2. Uploads each platform's installer as an artifact.
3. Creates a GitHub Release tagged `<id>` with all three binaries attached.

The binaries are unsigned, so macOS Gatekeeper / Windows SmartScreen will warn
on first launch. Add signing secrets later if that matters.
