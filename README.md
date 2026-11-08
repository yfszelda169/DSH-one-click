# DeepSeek Harness Windows 一键部署

[English](README.en.md) | 中文

双击 `install-deepseek-harness.cmd` 即可运行。它是一个**单文件自包含 CMD**，不需要旁边的 `.ps1`；安装逻辑会由 CMD 解码到系统临时目录后调用 Windows 自带 PowerShell 执行，结束后自动删除临时文件。脚本会按顺序检查环境、安装依赖、静默安装官方 Windows 桌面端，再把插件和技能装到当前用户目录。API Key 不再由安装器收集，首次启动后在 DSH 的 **Settings → Models** 中填写。

## 快速使用

1. 双击 `install-deepseek-harness.cmd`。
2. 等待 Node.js、Git、DeepSeek Harness 安装完成。
3. 脚本首次会短暂启动一次 DeepSeek Harness 以初始化 Desktop profile，然后自动关闭。
4. 按编号多选要安装的插件，再按编号多选要安装的技能。输入示例：`1 3 5`、`1,3,5`；`all` 全选；`0` 不安装。
5. 脚本优先从 GitHub 拉取所选插件/技能；GitHub 超时或失败时自动切换国内镜像。
6. 完成后 DeepSeek Harness 会自动启动。
7. 在 DeepSeek Harness 的 **Settings → Models** 中自行填写 DeepSeek API Key。

只检查、不修改系统：

```bat
install-deepseek-harness.cmd -CheckOnly -NoLaunch
```

## 可选参数

| 参数 | 作用 |
|---|---|
| `-CheckOnly` | 只做环境与网络检查，不安装任何软件 |
| `-SkipPlugins` | 跳过插件和技能的选择与安装 |
| `-NoLaunch` | 安装完成后不自动启动 DeepSeek Harness |
| `-Force` | 强制重新安装桌面端 |

## 安装内容

| 组件 | 来源 / 说明 |
|---|---|
| DeepSeek Harness 官方 Windows 桌面版 | `https://download.deepseek.com/desktop/dsh-latest-windows-x64.exe` |
| Node.js 24+ | 要求 `>=24.9.0`；优先 WinGet LTS，失败时回退官方绿色包。WinGet 可能触发 UAC |
| Git for Windows | 优先 WinGet；安装失败不会阻断主流程。WinGet 可能触发 UAC |
| WinGet | 已有则使用；不存在时 Node.js 会走官方绿色包回退 |

Node.js 是 `find-skills` 和 DSH CLI 生态所需依赖。基础安装不需要 Python；可选的 10 个插件也不依赖系统 Python。

## 默认路径

| 内容 | 路径 |
|---|---|
| DeepSeek Harness | `%LOCALAPPDATA%\Programs\DeepSeek Harness` |
| DSH_HOME | `%USERPROFILE%\.dsh` |
| 技能目录 | `%USERPROFILE%\.dsh\skills` |
| 安装日志 | `%LOCALAPPDATA%\DeepSeekHarnessInstaller` |

如果设置了 `DSH_HOME`，脚本会尊重该环境变量。

## 可选插件（10 个热门插件，多选）

插件使用 Desktop profile，并通过官方桌面端自带的 `dsh plugin --profile desktop add` 安装。安装器会列出以下 10 个插件供你多选，不会默认全装。以下 10 个插件已在临时 `DSH_HOME` 中用 DSH `0.2.2-rc.1` 组合安装，并通过 `--dump-config` 验证。

| # | 插件 | 版本 | Star | 作用 | GitHub |
|---:|---|---:|---:|---|---|
| 1 | `dshmarket` | 1.73.15 | 10.3k | 可视化插件市场 | https://github.com/dsh-market/dsh-market |
| 2 | `dsh-mnemon` | 0.5.42 | 912 | 分层记忆与上下文管理 | https://github.com/omdsh-dev/dsh-mnemon |
| 3 | `superpowers-dsh` | 0.4.0 | 135 | TDD、调试、规划、协作技能包 | https://github.com/LayneChai/superpowers-dsh |
| 4 | `@liustack/modlens` | 3.27.3 | 4.7k | 图片识别、OCR、UI 还原 | https://github.com/liustack/modlens |
| 5 | `@next-ai-drawio/dsh` | 0.1.0 | 36.7k | draw.io 实时图表编辑 | https://github.com/DayuanJiang/next-ai-draw-io |
| 6 | `@linxin666/dsh-client-ui-task-board` | 0.4.16 | 10.2k | 任务看板、子任务、定时执行 | https://github.com/zhu1090093659/dsh-web |
| 7 | `dsh-whale-widget` | 0.4.2 | 7.0k | DeepSeek 余额与用量挂件 | https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget |
| 8 | `dsh-better-sidebar` | 0.30.1 | 5.0k | 增强侧边栏、终端、文件、Git 等页面 | https://github.com/omdsh-dev/DSH-better-sidebar |
| 9 | `dsh-context` | 0.82.2 | 4.4k | 上下文、Token 和工具调用洞察 | https://github.com/bowenliang123/dsh-context |
| 10 | `@michengai/dsh-skills-manager` | 1.2.35 | 444 | 本机 Agent Skills 管理 | https://github.com/MichengAI/dsh-skills-manager |

> Star 数来自 2026-11-07 的 Awesome DSH Plugin 目录快照。

首次使用提示：ModLens 需要配置一个视觉模型或复用本机已有视觉配置；Next AI Draw.io 首次使用时会由 `npx` 下载 MCP server；Task Board 的 AI 解析使用 DSH 当前已配置模型；鲸鱼挂件直接使用脚本写入的 DeepSeek API Key 或 DSH 账号登录态。

## 可选技能（多选）

| 技能 | 来源 | 兼容性说明 |
|---|---|---|
| `ponytail` | https://github.com/DietrichGebert/ponytail | 标准 `SKILL.md` 格式，DSH 可直接发现 |
| `find-skills` | https://github.com/vercel-labs/skills | 依赖 `npx skills`；脚本已安装 Node.js/npm |
| `find-plugins` | https://github.com/Nagi-ovo/dsh-find-plugins | 专门用于在 GitHub 搜索 DSH 插件 |

技能固定到脚本编写时的 Git commit，避免 `main` 分支漂移。

## GitHub 优先与国内镜像

插件：先尝试对应 GitHub 仓库的固定 commit；如果 60 秒内超时、安装失败或 profile 未发生变化，则切换到 `registry.npmmirror.com` 安装同版本的 npm 包。

技能：先尝试 GitHub Raw；如果超时或失败，则切换到 jsDelivr 国内可访问 CDN 镜像。所有 Git 源都固定到 commit，不使用浮动的 `main`。

## API Key 配置

安装器不会询问、验证或保存 DeepSeek API Key。安装完成后启动 DeepSeek Harness，在 **Settings → Models** 中填写 API Key。API Key 由 DSH 自己的设置/凭据机制管理。

## 管理员权限与 UAC

- DeepSeek Harness 桌面端、插件、技能、用户 PATH 和 `%USERPROFILE%\.dsh` 下的配置都是用户级操作，不需要管理员权限。
- Node.js 和 Git 优先通过 WinGet 的 `--scope user` 安装；如果对应 WinGet 包不支持用户级安装，脚本可能回退到默认系统安装，此时 Windows 会弹出 UAC。
- `-CheckOnly` 只做检查和网络探测，不会安装软件，也不会触发 UAC。
- 如果你要求严格零 UAC，请预先安装 Node.js `>=24.9.0` 和 Git，或把脚本改成完全使用便携版 Node/Git。
- 建议用普通用户身份运行；脚本检测到管理员身份时会提示通常不需要管理员权限。

## 校验与安全

- 官方 DSH 安装包会检查 Authenticode 签名，失败则中止。
- 脚本会打印安装包 SHA256。
- Node.js 绿色包回退会校验 Node 官方 `SHASUMS256.txt`。
- 插件先安装固定 GitHub commit，失败时回退到同版本 npmmirror npm 包；技能使用固定 commit，并有 jsDelivr 回退。
- 第三方插件会以当前用户权限运行在 DSH 进程中，安装前请自行评估仓库可信度。

## 注意事项

- 需要 Windows 10/11 64 位、至少 2 GB 可用空间和互联网连接。
- 安装器不处理 API Key；请在首次启动后的 **Settings → Models** 中配置。
- 官方 DSH 桌面端是用户级安装，通常不需要管理员权限。
- 首次初始化 Desktop profile 时，应用可能短暂出现窗口，完成后由脚本自动关闭。
- 如果插件安装失败，重新运行脚本并重新选择即可；已安装的组件会跳过或更新。
- DeepSeek Harness 仍是 Developer Preview，插件接口可能发生兼容性破坏。插件版本固定在脚本中，后续升级时需要同步调整版本。
- 卸载 DeepSeek Harness 前，建议在应用菜单中移除它注册的 `dsh` 命令；本脚本只把桌面端自带 CLI 目录追加到用户 PATH。

## 参考

- DeepSeek Harness 官方仓库：https://github.com/deepseek-ai/deepseek-harness
- 官方 Web UI 指南：https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.md
- 官方技能目录规则：https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/README.md
