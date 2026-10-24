# SimHub Dashboard: System Monitor

A 1280x720 SimHub dashboard showing CPU, GPU, RAM, VRAM and FPS.

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

Export the dashboard from SimHub's Dash Studio and attach the `.simhubdash` to a GitHub release.
