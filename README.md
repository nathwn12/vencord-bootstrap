# vencord-bootstrap

Install and repair Vencord from the official upstream release with one PowerShell line.

Unofficial, personal-use driver. All credit to Vencord by Vendicated and contributors, and the Vencord Installer engine it downloads at run time.

## Install

```powershell
iex (irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1')
```

You need Windows with PowerShell 5.1 or newer and Discord already installed. You run once to detect your installs, download the official engine once, install or repair Vencord, then launch Discord.

To include OpenAsar on each install:

```powershell
& ([scriptblock]::Create((irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1'))) -IncludeOpenAsar
```

Add `-DryRun` to the second form to preview the plan without changing anything.

## What it does

- Detect your Discord installs across stable, PTB, Canary, and Development.
- Install Vencord on a fresh install or repair an already patched install.
- Close Discord automatically before patching so a file lock never blocks the run.
- Download the officially released executable at run time, so you always run the current engine.
- Keep OpenAsar off unless you pass `-IncludeOpenAsar`; `-ExcludeOpenAsar` restates the default and cannot be combined with `-IncludeOpenAsar`.
- Target one branch with `-Branch` (stable, ptb, canary, development, or dev as an alias for development) or one folder with `-Location`; the two cannot be combined; default is all detected installs.
- Skip the automatic launch after success with `-NoLaunch`.
- Re-run any time to repair or refresh; you verify each install on disk after the engine runs.
- Read exit codes: `0` success including a dry run even when no installs are found, `1` install failure such as the engine or download failing, `2` usage or detection error such as conflicting flags, an unknown branch, or no Discord installs found.
- Use network access to GitHub for the script and the engine download; you need no admin rights.

## Links

- Upstream project `Vencord/Installer`: https://github.com/Vencord/Installer — this is a reduced fork of `Vencord/Installer`; the installer logic is upstream's work; you drive the upstream released engine with its own flags and patching is not reimplemented here.
- Upstream author/project `Vendicated/Vencord`: https://github.com/Vendicated/Vencord
- Upstream releases, the engine this script downloads at run time: https://github.com/Vencord/Installer/releases
- This repo: https://github.com/nathwn12/vencord-bootstrap
- License: GPL-3.0 — see [LICENSE](LICENSE).
