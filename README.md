# vencord-bootstrap

Installs and repairs Vencord from the official upstream release with one PowerShell line.

Unofficial, personal-use driver. All credit to [Vencord](https://github.com/Vendicated/Vencord) by Vendicated and contributors, and the [Vencord Installer](https://github.com/Vencord/Installer) engine it downloads at run time.

## Install

```powershell
iex (irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1')
```

Requires Windows with PowerShell 5.1 or newer and Discord already installed. The first run detects your installs, downloads the official engine once, installs or repairs Vencord, then launches Discord.

To include OpenAsar on each install:

```powershell
& ([scriptblock]::Create((irm 'https://raw.githubusercontent.com/nathwn12/vencord-bootstrap/main/vencord-setup.ps1'))) -IncludeOpenAsar
```

Add `-DryRun` to the second form to preview the plan without changing anything.

## What it does

- Detects Discord installs across stable, PTB, Canary, and Development.
- Installs Vencord on a fresh install, repairs an already patched install.
- Closes Discord automatically before patching so a file lock never blocks the run.
- Downloads the officially released executable at run time, so the engine is always current.
- Leaves OpenAsar off unless `-IncludeOpenAsar` is passed; `-ExcludeOpenAsar` restates the default and cannot be combined with `-IncludeOpenAsar`.
- Targets one branch with `-Branch` (stable, ptb, canary, development, or dev as an alias for development) or one folder with `-Location`; the two cannot be combined; default is all detected installs.
- Skips the automatic launch after success with `-NoLaunch`.
- Safe to re-run any time to repair or refresh; each install is verified on disk after the engine runs.
- Exit codes: `0` success including a dry run even when no installs are found, `1` install failure such as the engine or download failing, `2` usage or detection error such as conflicting flags, an unknown branch, or no Discord installs found.
- Needs network access to GitHub for the script and the engine download; no admin rights needed.

## Notes

- Derived from `Vencord/Installer`; patching is not reimplemented here, this script drives the upstream released engine with its own flags.
- Upstream Vencord: https://github.com/Vendicated/Vencord
- Upstream Installer: https://github.com/Vencord/Installer
- Upstream releases, the engine this script downloads at run time: https://github.com/Vencord/Installer/releases
- This repo: https://github.com/nathwn12/vencord-bootstrap

## License

GPLv3, see [LICENSE](LICENSE).
