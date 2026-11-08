# DeepSeek Harness Windows One-Click Installer

[中文](README.zh.md) | [English](README.en.md) | [Français](README.fr.md) | [Español](README.es.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Double-click `install-deepseek-harness.cmd` to run it. This is a **single-file, self-contained CMD installer** and does not require a separate `.ps1` file. The CMD extracts its embedded installer logic to the system temporary directory, runs it with the built-in Windows PowerShell, and removes the temporary file afterward. The installer checks the environment, installs dependencies, silently installs the official Windows desktop app, then installs the plugins and skills you select into the current user's profile. The installer no longer collects an API key. After the first launch, enter it in DeepSeek Harness under **Settings → Models**.

The installer automatically detects the Windows system language and supports Chinese, English, French, Spanish, Russian, and Arabic. You can force a language with `set DSH_INSTALLER_LANG=en` before running the installer.

## Quick start

1. Double-click `install-deepseek-harness.cmd`.
2. Wait for Node.js, Git, and DeepSeek Harness to finish installing.
3. The installer briefly launches DeepSeek Harness once to initialize the Desktop profile, then closes it automatically.
4. Select the plugins you want by number, then select the skills you want. Examples: `1 3 5` or `1,3,5`; `all` selects everything; `0` installs nothing.
5. The installer tries GitHub first for every selected plugin/skill and automatically falls back to a China-accessible mirror on timeout or failure.
6. DeepSeek Harness starts automatically when the installer finishes.
7. Enter your DeepSeek API key in DeepSeek Harness under **Settings → Models**.

Check the environment without changing the system:

```bat
install-deepseek-harness.cmd -CheckOnly -NoLaunch
```

## Options

| Option | Description |
|---|---|
| `-CheckOnly` | Only checks the environment and network; installs nothing |
| `-SkipPlugins` | Skips plugin/skill selection and installation |
| `-NoLaunch` | Does not launch DeepSeek Harness after installation |
| `-Force` | Forces a desktop reinstall |

## What gets installed

| Component | Source / behavior |
|---|---|
| DeepSeek Harness official Windows desktop app | `https://download.deepseek.com/desktop/dsh-latest-windows-x64.exe` |
| Node.js 24+ | Requires `>=24.9.0`; prefers WinGet LTS and falls back to the official portable ZIP. WinGet may trigger UAC |
| Git for Windows | Prefers WinGet; failure does not block the main flow. WinGet may trigger UAC |
| WinGet | Used when available; otherwise Node.js falls back to the official ZIP |

Node.js is required by `find-skills` and the DSH CLI ecosystem. Python is not required for the base install, and the optional plugins do not require a system Python installation.

## Default paths

| Content | Path |
|---|---|
| DeepSeek Harness | `%LOCALAPPDATA%\Programs\DeepSeek Harness` |
| DSH_HOME | `%USERPROFILE%\.dsh` |
| Skills | `%USERPROFILE%\.dsh\skills` |
| Installer log | `%LOCALAPPDATA%\DeepSeekHarnessInstaller` |

If `DSH_HOME` is set, the installer respects it.

## Optional plugins (10 popular plugins, multi-select)

Plugins are installed into the Desktop profile through the bundled `dsh plugin --profile desktop add` command. The installer lists the 10 plugins below for multi-select and does not install all of them by default. The combination was installed in a temporary `DSH_HOME` with DSH `0.2.2-rc.1` and validated with `--dump-config`.

| # | Plugin | Version | Stars | Purpose | GitHub |
|---:|---|---:|---:|---|---|
| 1 | `dshmarket` | 1.73.15 | 10.3k | Visual plugin marketplace | https://github.com/dsh-market/dsh-market |
| 2 | `dsh-mnemon` | 0.5.42 | 912 | Layered memory and context management | https://github.com/omdsh-dev/dsh-mnemon |
| 3 | `superpowers-dsh` | 0.4.0 | 135 | TDD, debugging, planning, and collaboration skills | https://github.com/LayneChai/superpowers-dsh |
| 4 | `@liustack/modlens` | 3.27.3 | 4.7k | Image understanding, OCR, and UI reconstruction | https://github.com/liustack/modlens |
| 5 | `@next-ai-drawio/dsh` | 0.1.0 | 36.7k | Live draw.io diagram editing | https://github.com/DayuanJiang/next-ai-draw-io |
| 6 | `@linxin666/dsh-client-ui-task-board` | 0.4.16 | 10.2k | Task board, subtasks, and scheduled runs | https://github.com/zhu1090093659/dsh-web |
| 7 | `dsh-whale-widget` | 0.4.2 | 7.0k | DeepSeek balance and usage widget | https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget |
| 8 | `dsh-better-sidebar` | 0.30.1 | 5.0k | Enhanced sidebar, terminal, files, Git, and subagent pages | https://github.com/omdsh-dev/DSH-better-sidebar |
| 9 | `dsh-context` | 0.82.2 | 4.4k | Context, token, and tool-call insights | https://github.com/bowenliang123/dsh-context |
| 10 | `@michengai/dsh-skills-manager` | 1.2.35 | 444 | Local Agent Skills management | https://github.com/MichengAI/dsh-skills-manager |

> Star counts are from the Awesome DSH Plugin catalog snapshot dated 2026-11-07.

First-use notes: ModLens needs a vision model or an existing local vision configuration; Next AI Draw.io downloads its MCP server through `npx` on first use; Task Board uses the model currently configured in DSH; the whale widget uses the DSH-managed DeepSeek credential or account session.

## Optional skills (multi-select)

| Skill | Source | Compatibility |
|---|---|---|
| `ponytail` | https://github.com/DietrichGebert/ponytail | Standard `SKILL.md` format, directly discoverable by DSH |
| `find-skills` | https://github.com/vercel-labs/skills | Uses `npx skills`; Node.js/npm is installed by the installer |
| `find-plugins` | https://github.com/Nagi-ovo/dsh-find-plugins | Searches GitHub for DSH plugins |

Skills are pinned to the Git commits used when this installer was built to avoid `main` branch drift.

## GitHub-first with China mirror fallback

Plugins: the installer first tries the pinned GitHub commit for the selected repository. If it times out within 60 seconds, fails, or does not change the profile, it falls back to installing the same pinned npm version from `registry.npmmirror.com`.

Skills: the installer first tries GitHub Raw. On timeout or failure, it falls back to the China-accessible jsDelivr CDN mirror. All Git sources are pinned to commits and never use a floating `main`.

## API key configuration

The installer does not ask for, validate, or store a DeepSeek API key. After installation, launch DeepSeek Harness and enter the key under **Settings → Models**. The key is managed by DSH's own settings and credential mechanism.

## Administrator rights and UAC

- The DeepSeek Harness desktop app, plugins, skills, user PATH changes, and files under `%USERPROFILE%\.dsh` are user-level operations and do not require administrator rights.
- Node.js and Git are first installed through WinGet with `--scope user`. If the WinGet package does not support user scope, the installer may fall back to the default system install, and Windows may show a UAC prompt.
- `-CheckOnly` only checks the environment and network. It installs nothing and does not trigger UAC.
- If you need a strict zero-UAC setup, preinstall Node.js `>=24.9.0` and Git, or modify the installer to use fully portable Node.js/Git distributions.
- Run the installer as a normal user. The script warns when it detects an administrator session because administrator rights are usually unnecessary.

## Validation and safety

- The official DSH installer validates its Authenticode signature and aborts on failure.
- The installer prints the installer package SHA256.
- The Node.js portable fallback verifies the official `SHASUMS256.txt`.
- Plugins first use a pinned GitHub commit and fall back to the same version from npmmirror; skills use pinned commits with jsDelivr fallback.
- Third-party plugins run in the DSH process with your user permissions. Review each repository before installing it.

## Notes

- Requires 64-bit Windows 10/11, at least 2 GB free disk space, and internet access.
- The installer does not handle the API key. Configure it in **Settings → Models** after the first launch.
- The official DSH desktop app is installed per-user and usually does not require administrator rights.
- The app may briefly appear while the Desktop profile is initialized; the installer closes it automatically.
- If a plugin installation fails, rerun the installer and select it again. Already installed components are skipped or updated.
- DeepSeek Harness is still in Developer Preview, and plugin interfaces may break. Plugin versions are pinned in the installer and must be updated when compatibility changes.
- Before uninstalling DeepSeek Harness, remove the `dsh` command registered by the app. This installer only appends the bundled CLI directory to the user PATH.

## References

- DeepSeek Harness: https://github.com/deepseek-ai/deepseek-harness
- Official Web UI guide: https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.md
- Official skill filesystem rules: https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/README.md
