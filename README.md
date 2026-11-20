<h1 align="center">vencord-bootstrap</h1>
<p align="center"><strong>Install and repair Vencord from the official upstream release with one PowerShell line.</strong></p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-windows--64bit-2b3137?logo=windows&logoColor=white" alt="Platform" />
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-GPL--3.0-green" alt="GPL-3.0" /></a>
</p>

## Why vencord-bootstrap

Installing Vencord normally means finding the right installer for each Discord branch and running it by hand. This script finds every standard install, drives the official released CLI and verifies the result, from one line you can paste into PowerShell.

- **Every branch in one run.** Stable, PTB, Canary and Development are detected and patched together.
- **Upstream engine.** It downloads Vencord's released `VencordInstallerCli.exe`; patching is not reimplemented here.
- **Verified afterwards.** Each install is checked on disk and launched through its own updater on success.
- **Dry run first.** `-DryRun` prints the exact plan and changes nothing.

## Install

```powershell
iex (irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1')
```

The one-liner finds every standard Discord install (stable, PTB, Canary, Development), downloads
upstream's released `VencordInstallerCli.exe` into the temp folder, then installs or repairs Vencord
on each install and launches it. Run it again whenever you want a repair. For OpenAsar, use the
parameterised form:

```powershell
& ([scriptblock]::Create((irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1'))) -IncludeOpenAsar
```

## Use it

Start with a dry run. It prints the plan and the exact engine commands, and changes nothing:

```powershell
& ([scriptblock]::Create((irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1'))) -DryRun
```

Then run the first command. During a real run the script:

- Closes Discord before patching, so a file lock cannot block the engine, and retries a transient failure twice.
- Verifies each install on disk after the engine exits: the original asar is renamed and the stub is in place.
- Launches every patched install through its own updater when the run succeeds.

Exit codes: `0` success, including a dry run that finds no install; `1` install failure, such as the download or the engine failing; `2` usage or detection error, such as conflicting switches or an unknown branch.

## Configure it

Every switch is optional. The defaults install or repair all detected installs, keep OpenAsar off, and launch Discord afterwards.

| Switch | Default | Effect |
| --- | --- | --- |
| `-DryRun` | off | Print the plan; download, execute and modify nothing. |
| `-IncludeOpenAsar` | off | Also install OpenAsar on each detected install. |
| `-ExcludeOpenAsar` | off | Restate the default; cannot be combined with `-IncludeOpenAsar`. |
| `-Branch <name>` | all detected | One branch: `stable`, `ptb`, `canary`, `development` (alias `dev`). |
| `-Location <path>` | all detected | One install base directory; cannot be combined with `-Branch`. |
| `-NoLaunch` | launch | Do not start Discord after a successful run. |

One branch, with OpenAsar, without launching:

```powershell
$script = irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1'
& ([scriptblock]::Create($script)) -Branch canary -IncludeOpenAsar -NoLaunch
```

## Requirements

- Windows 10 or 11, with the Discord desktop app already installed on at least one branch.
- Windows PowerShell 5.1 or newer; `pwsh` 7 works too.
- Network access to `github.com` for the script and the engine download. No admin rights.

## Development

The repository is one script. Edit `vencord-setup.ps1`, keep it pure ASCII with no BOM, and prove it before committing:

```powershell
pwsh -NoProfile -File vencord-setup.ps1 -DryRun
```

That command exits `0` and prints either the plan or `Nothing to do.` when no Discord install exists. CI runs the same dry run on `windows-latest` for every push to `main` and every pull request, in `.github/workflows/ci.yml`. A pull request that only touches Markdown, `docs/` or `LICEN[CS]E` reports the check green without running it.

## Status

**Experimental** — pre-release; interfaces may change.

## Links

- [Issues](https://github.com/nathwn12/vencord-bootstrap/issues)
- Upstream project: [Vencord Installer](https://github.com/Vencord/Installer) — the installer the Vencord project ships; this repo is a reduced fork that drives its released CLI, and patching is not reimplemented here.
- Upstream product: [Vencord](https://github.com/Vendicated/Vencord) — the client mod this installs, by Vendicated and contributors.
- Upstream releases: [VencordInstallerCli.exe](https://github.com/Vencord/Installer/releases) — the engine each run downloads.
- [GPL-3.0](LICENSE)

Built by nathwn12
