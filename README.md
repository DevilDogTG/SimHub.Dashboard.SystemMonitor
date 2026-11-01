# SimHub Dashboard: System Monitor

A 1280x720 SimHub dashboard showing CPU, GPU, RAM, VRAM and FPS.

## Requirements

- HWiNFO64 with Shared Memory Support enabled
- The [DevDogs System Info](https://github.com/DevilDogTG/SimHub.Plugins.SystemInfo) SimHub plugin (`DDSystemInfoPlugin.*` properties).
  Every binding uses its hardware-agnostic names, so the dashboard works on any PC the plugin supports. Values show `-`
  while HWiNFO isn't running.

## Layout

| Path | What it is |
|---|---|
| `src/System Monitor/` | The dashboard folder exactly as SimHub stores it in `DashTemplates` |
| `scripts/Install-SimHubLink.ps1` | Links SimHub's `DashTemplates\System Monitor` to `src/System Monitor/` |

`.simhubdash` exports are not tracked; they are attached to GitHub releases.

## Editing with SimHub

SimHub's Dash Studio saves into `DashTemplates`. A directory junction there makes it save straight into this repo.

1. Close SimHub.
2. From an elevated PowerShell in the repo root:

   ```powershell
   .\scripts\Install-SimHubLink.ps1
   ```

   SimHub's existing copy is moved to `$HOME\SimHub-DashBackups\`, not deleted.
3. Start SimHub and edit as usual, then commit from the repo.

The junction means SimHub always shows the branch that is checked out. Close the dashboard in Dash Studio before switching branches.

Don't import a `.simhubdash` of the same name while linked: SimHub would overwrite the folder, and that folder is now this repo.

## Readable diffs

`System Monitor.djson` is a single minified JSON line. Register the pretty-print diff driver once per clone:

```powershell
git config diff.djson.textconv "PYTHONUTF8=1 python -m json.tool --no-ensure-ascii"
```

## Releasing

Every PR that bumps `DashboardVersion` is released once it merges. PRs without a bump are not released.

`DashboardVersion` (set in Dash Studio's dashboard properties) is `X.Y.Z`, bumped in the PR that makes the change:

| Bump | When |
|---|---|
| X | A deliberate major release, decided by the maintainer |
| Y (Z resets to 0) | A new feature, or a dependency update: SimHub, the DevDogs System Info plugin, or another plugin |
| Z | A fix to something that already exists |
| None | Hidden layers, refactors with no visible change, repo tooling |

A `.simhubdash` is a zip of the `System Monitor/` folder, so build it from the merged commit rather than exporting it
from Dash Studio. It then holds exactly the files tracked under `src/` (SimHub's `_Backups/` is git-ignored):

```powershell
git archive --format=zip -9 -o "System.Monitor.v<version>.simhubdash" <commit>:src
```

Tag that commit and publish the release with the file attached:

```powershell
git tag -a v<version> <commit> -m "release: v<version>"
git push origin v<version>
gh release create v<version> --title "v<version>" --notes-file <notes.md> --verify-tag "System.Monitor.v<version>.simhubdash"
```

The release notes state the SimHub version the dashboard was saved with and the minimum DevDogs System Info plugin version.
