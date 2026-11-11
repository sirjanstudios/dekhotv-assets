# dekhotv-assets

Signed catalogs and media for DekhoTV, a private Android TV launcher. Data only: no code, no CI.

**Never edit this repo by hand.** Everything here is written by `tools/publish/publish.py` in the
DekhoTV repo, which builds, signs, commits and pushes. A hand edit breaks a hash or the signature,
and the TVs refuse it.

## Layout

| Path | What it is |
|---|---|
| `index.json` | The only signed file and the app's one fixed address: which catalog files are current, and the version. ECDSA P-256 / SHA-256 over the exact payload bytes (`{"payload", "key", "sig"}`). |
| `catalogs/<name>-<sha12>.json` | The catalogs (`wallpapers`, `icons`). Content-addressed: never edited in place, deleted once no index names them. |
| `media/frost/<clip id>-<sha12>.frost` | Per-clip frost tracks, one link each. |
| `media/…` | The owner's own small media. |

Big files (over ~50 MB, such as our own videos) go in Release assets, never in git. No Git LFS:
`raw.githubusercontent.com` doesn't serve it. `.gitattributes` keeps every byte exact.

The app reads `https://raw.githubusercontent.com/sirjanstudios/dekhotv-assets/main/index.json`,
only when the user presses Check for New, and verifies every file against the signed index.
