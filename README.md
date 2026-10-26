# Claude Code 国内使用指南（2026）

> 最后更新：2026-10-26（第二次）· Claude 官方价格与政策复核（无变化）；文末新增"复测记录"。

这是一份面向中文用户的 Claude Code 使用说明：Claude Code 是 Anthropic 官方的终端编程助手，本文按官方文档整理它的安装、登录、Pro / Max 额度怎么选、官方价格，以及安装、登录、额度和网络相关的常见报错怎么处理，每条附官方链接。作者：瑶瑶ai训练师；更新于 2026-10-17。本仓库是非官方整理，与 Anthropic 没有隶属关系，文中品牌名仅用于说明。

**直接回答：** Claude Code 不单独收费：开通 Claude Pro 或 Max 订阅，运行 `claude` 命令登录就能用；也可以用 Console 的 API Key 按量付费。免费版 claude.ai 不包含 Claude Code。

**安装：** macOS / Linux / WSL 运行 `curl -fsSL https://claude.ai/install.sh | bash`，Windows PowerShell 运行 `irm https://claude.ai/install.ps1 | iex`，装完新开一个终端执行 `claude --version` 确认。

**选套餐：** 偶尔用选 Pro（$20/月），每天用几个小时选 Max 5x（$100/月），几乎全天跑选 Max 20x（$200/月）。额度按 5 小时窗口加每周上限计算，网页和 Claude Code 共用一份。

## 目录

- [使用前先读：关于地区](#使用前先读关于地区)
- [需要什么账号](#需要什么账号)
- [官方价格表](#官方价格表2026-10-17-核对)
- [安装](#安装)
- [登录](#登录)
- [Pro 和 Max 额度怎么选](#pro-和-max-额度怎么选)
- [报错 / 常见问题大全](#报错--常见问题大全)
- [网络与代理](#网络与代理)
- [常用命令和上手建议](#常用命令和上手建议)
- [和 Codex 怎么选](#和-codex-怎么选)
- [常见问题](#常见问题)
- [国内开通参考](#国内开通参考)
- [参考资料](#参考资料)

## 使用前先读：关于地区

Anthropic 官方公布了支持的国家和地区列表（[anthropic.com/supported-countries](https://www.anthropic.com/supported-countries)），页面写明未列出的国家和地区不受支持；2026-10-17 查看，中国大陆、香港、澳门都不在列表中（台湾在列表中）。官方文档里对应的提示是 `App unavailable in region`，意思是 Claude Code 在你所在的国家不可用。本指南只整理官方的安装、登录、订阅和报错说明，不提供绕过地区限制的方法；请在符合 Anthropic 服务条款的前提下使用。

## 需要什么账号

官方要求使用 Pro、Max、Team、Enterprise 或 Console 账号，免费版 claude.ai 不包含 Claude Code。

| 登录方式 | 怎么计费 | 适合谁 |
|---|---|---|
| Claude Pro / Max 订阅账号 | 含在会员里，有 5 小时窗口和每周上限，和网页版共用额度 | 个人开发者 |
| Console 账号 / API Key | 按 token 用量扣 API 余额，用得多费用涨得快 | 脚本、自动化、公司统一结算 |
| Team / Enterprise | 按席位，管理员邀请 | 团队 |
| Amazon Bedrock 等云平台 | 走云平台账单 | 企业 |

## 官方价格表（2026-10-17 核对）

| 套餐 | 官方价（美元） | 和 Claude Code 有关的说明 |
|---|---|---|
| Free | $0 | 不含 Claude Code |
| Pro | $20/月；年付一次交 $200，折合 $17/月 | 含 Claude Code；每 5 小时用量至少是 Free 的 5 倍 |
| Max 5x | $100/月 | 含 Claude Code；每 5 小时用量是 Pro 的 5 倍；附每月 API credits（只能在 API 平台用，不能在 Claude Code 里用） |
| Max 20x | $200/月 | 同上，用量是 Pro 的 20 倍 |
| Team | 标准席位 $20/人/月（年付）或 $25（月付）；高级席位 $100 / $125 | 含 Claude Code，2–150 人 |
| Console / API | 按模型和 token 用量计费 | 用 API Key 登录 Claude Code 时按这个算 |

来源：[Anthropic 官方定价页 claude.com/pricing](https://claude.com/pricing)（anthropic.com/pricing 会跳转到这里）、[What is the Pro plan?](https://support.claude.com/en/articles/8325606-what-is-the-pro-plan)、[What is the Max plan?](https://support.claude.com/en/articles/11049741-what-is-the-max-plan)、[Use Claude Code with your Pro or Max plan](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan)。帮助中心注明这是网页订阅价，App 内订阅价格由应用商店决定；价格和套餐可能调整，以官方为准。

## 安装

系统要求（官方）：macOS 13.0+、Windows 10 1809+ 或 Windows Server 2019+、Ubuntu 20.04+、Debian 10+、Alpine Linux 3.19+；内存 4 GB+，x64 或 ARM64；需要联网；Shell 支持 Bash、Zsh、PowerShell、CMD；所在地在 Anthropic 支持的国家和地区。

**方式一：原生安装（官方推荐，自动后台更新）**

macOS / Linux / WSL：

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Windows PowerShell：

```powershell
irm https://claude.ai/install.ps1 | iex
```

Windows CMD：

```batch
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

Windows 上提示符带 `PS C:\` 是 PowerShell，不带 `PS` 是 CMD，两边命令不能混用。安装过程没有进度条，装完后新开一个终端窗口运行 `claude --version`。

**方式二：Homebrew（不自动更新，用 `brew upgrade claude-code` 升级）**

```bash
brew install --cask claude-code
```

**方式三：WinGet（不自动更新，用 `winget upgrade Anthropic.ClaudeCode` 升级）**

```powershell
winget install Anthropic.ClaudeCode
```

**方式四：npm（需要 Node.js 22 或更高版本）**

```bash
npm install -g @anthropic-ai/claude-code
```

官方提醒：不要用 `sudo npm install -g`，会引发权限问题和安全风险。升级用 `npm install -g @anthropic-ai/claude-code@latest`。

安装完成后，打开一个新的终端窗口验证：

```bash
claude --version
claude doctor
```

`claude --version` 会打印版本号（例如 `2.1.211 (Claude Code)`），`claude doctor` 打印安装和配置的诊断信息。之后进入你的项目目录，运行 `claude` 启动。需要手动更新时运行 `claude update`。

Windows 说明：原生 Windows 建议安装 Git for Windows（可选），Claude Code 会用 Git Bash 作为 Bash 工具；不装就用 PowerShell 工具。需要 Linux 工具链或沙盒时，在 WSL 2 里运行 Linux 的安装命令和 `claude`。不想用终端的，官方也有 Claude Code 桌面应用。

## 登录

1. 在项目目录运行 `claude`，首次启动会自动打开浏览器让你登录。
2. 浏览器没有自动弹出：按 `c` 复制登录链接，粘贴到浏览器。
3. 登录后浏览器显示的是一串登录码而不是自动跳回：粘贴到终端的 `Paste code here if prompted` 提示处（WSL2、SSH、容器里常见）。
4. 终端显示 `Login successful` 后按 `Enter` 继续。

登录相关命令：

- `/login`：重新登录或切换账号；`/logout`：退出登录（同时清掉首次设置状态，下次启动会重新引导）。
- `/status`：查看当前账号、登录方式和连接状态；登录和 API Key 同时存在时，`/status` 会标出哪一个没在用。
- 粘贴登录码没反应：在终端里运行 `claude auth login`，它从标准输入读取登录码。
- 登录快到期时启动会提示 `Your login expires in 3 days · run /login to renew`，运行 `/login` 续期即可。

**订阅和 API 的优先级。** 如果电脑环境变量里设置了 `ANTHROPIC_API_KEY`，并且你在提示里同意使用，Claude Code 会优先用这个 Key 按 API 计费，而不是你的订阅。官方的处理办法：运行 `unset ANTHROPIC_API_KEY`（Windows PowerShell 用 `Remove-Item Env:ANTHROPIC_API_KEY`），再用 `/status` 确认当前用的是哪种方式；要永久生效，把 `~/.zshrc`、`~/.bashrc` 里对应的 `export` 行删掉。

**多账号。** 官方方法是给每个账号一个独立配置目录：`alias claude-work='CLAUDE_CONFIG_DIR=~/.claude-work claude'`，写进 `~/.zshrc` 或 `~/.bashrc`，之后 `claude-work` 用工作账号、`claude` 用个人账号，各自有自己的登录、设置和会话记录。

## Pro 和 Max 额度怎么选

| 套餐 | 官方价 | 用量 | 适合谁 |
|---|---|---|---|
| Free | $0 | 不含 Claude Code | 偶尔问几个问题 |
| Pro | $20/月；年付一次交 $200，折合 $17/月 | 基准 | 偶尔用 Claude Code 写脚本 |
| Max 5x | $100/月 | 每 5 小时用量是 Pro 的 5 倍 | 每天用几个小时 |
| Max 20x | $200/月 | 每 5 小时用量是 Pro 的 20 倍 | 几乎全天在跑 |

官方定价页和帮助中心说明：每个套餐的用量按滚动 5 小时窗口重置，付费套餐在此之外还有每周上限（每周在账号固定的时间重置），Max 另有 Fable 模型的单独每周上限，官方也可能按容量设置其他上限。网页、桌面端、手机、编辑器插件和 Claude Code 的使用都从同一份额度里扣。接近上限会先收到提醒；回复中途到了 5 小时上限，Claude Code 可能显示 `Usage limit reached · wrapping up` 把当前这一步收尾。

- 不确定就先买 Pro 用一个月，一周里好几天提示额度用完，再升 Max 5x；升级按剩余账期比例补差价，立即生效。
- 想看当前用量和重置时间：在 Claude Code 里输入 `/usage`，网页在 Settings → Usage。
- 额度用完后想继续：等重置、升级套餐，或者用 `/usage-credits` 开启用量额度（按标准 API 价格计费，可设每月上限；不想多花钱就不要开）。
- Max 附带的每月 API credits 只能在 API 平台用，不能在 Claude Code 和 Claude 应用里当额度用。

## 报错 / 常见问题大全

下面每条按"你看到的 → 原因 → 怎么办"写，报错文字和处理办法来自官方文档，最后一列是出处，以官方最新说明为准。

| 你看到的 | 原因 | 怎么办 | 官方出处 |
|---|---|---|---|
| `command not found: claude` / `'claude' is not recognized` | 安装目录不在 PATH 里（macOS / Linux 是 `~/.local/bin`，Windows 是 `%USERPROFILE%\.local\bin`） | 先新开一个终端；还不行，macOS / Linux 运行 `echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc` 然后 `source ~/.zshrc`（bash 用 `~/.bashrc`）；Windows PowerShell 把 `%USERPROFILE%\.local\bin` 加进用户 PATH 后重开终端 | [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `syntax error near unexpected token '<'`、`< was unexpected at this time` 或 `curl: (22) ... 403` | 安装地址返回的是网页而不是安装脚本：临时故障、代理或网络过滤拦截，或者所在国家不受支持（输出里会有 `App unavailable in region`） | 等几分钟重试；改用 Homebrew、WinGet 安装；`App unavailable in region` 说明当前所在国家不受支持，见 [支持的国家和地区](https://www.anthropic.com/supported-countries) | [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `irm is not recognized` / `The token '&&' is not a valid statement separator` / `'bash' is not recognized` | 在 CMD 里跑了 PowerShell 命令，或反过来 | 看提示符：`PS C:\` 用 `irm ... | iex`，`C:\` 用 CMD 那条命令 | [安装说明](https://code.claude.com/docs/en/setup) |
| `Failed to fetch version from downloads.claude.ai` | 网络无法访问下载服务器 | 运行 `curl -sI https://downloads.claude.ai/claude-code-releases/latest` 测试，返回 200 才正常；403 多是代理或网络过滤，或地区不受支持；检查 `HTTPS_PROXY` 设置，见下文"网络与代理" | [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `TLS connect error`、`unable to get local issuer certificate` 等 TLS 报错 | 系统根证书过旧，或公司代理做 TLS 检查 | 更新系统 CA 证书；公司代理用 `export NODE_EXTRA_CA_CERTS=/path/to/ca-cert.pem` 让它信任代理证书 | [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `EACCES: permission denied`（npm 安装时） | npm 全局目录没有写权限 | 不要用 `sudo npm install -g`；官方建议改用原生安装脚本，或按排错页修复 npm 目录权限 | [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `Unable to connect to Anthropic services`（后面跟 `ECONNREFUSED` 或 `timed out after 10 seconds`） | 首次启动时连不上 `api.anthropic.com` 或 `platform.claude.com` | 运行 `curl -I https://api.anthropic.com` 测试；检查提示里点名的代理变量是否正确、防火墙是否放行；官方提示网络正常仍失败时，可能是当前国家不可用 | [错误参考](https://code.claude.com/docs/en/errors) |
| `OAuth error: Invalid code. Please make sure the full code was copied` | 登录码过期或复制不完整；SSH 远程时浏览器开在了另一台机器上 | 回车重试，浏览器打开后尽快完成；按 `c` 复制完整链接；远程机器把终端里的链接复制到本地浏览器打开 | [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| 浏览器登录后显示一串登录码，不跳回终端（WSL2、SSH、容器） | 浏览器连不上 Claude Code 本机的回调端口 | 把登录码粘贴到 `Paste code here if prompted` 处；粘贴没反应就运行 `claude auth login`；WSL2 浏览器打不开时设置 `export BROWSER="/mnt/c/Program Files/Google/Chrome/Application/chrome.exe"` 后再运行 `claude` | [登录与认证](https://code.claude.com/docs/en/authentication)、[安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `API Error: 403 Request not allowed` | 订阅未生效，或 Console 账号没有 "Claude Code" / "Developer" 角色；公司代理干扰 | Pro / Max 用户在 claude.ai/settings 确认订阅有效；Console 用户让管理员在 Members 页分配角色；代理环境见"网络与代理" | [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `This organization has been disabled` 或 `Your ANTHROPIC_API_KEY belongs to a disabled organization`（但订阅有效） | 环境变量里有旧的 `ANTHROPIC_API_KEY`（常见是以前公司或项目留下的），它优先于订阅 | `unset ANTHROPIC_API_KEY`（PowerShell：`Remove-Item Env:ANTHROPIC_API_KEY`），删掉 `~/.zshrc`、`~/.bashrc` 或 `$PROFILE` 里的对应行，重新运行 `claude`，`/status` 确认 | [错误参考](https://code.claude.com/docs/en/errors) |
| `Credit balance is too low` | 用的是 Console 账号而 API 预付余额用完了；或者 `ANTHROPIC_API_KEY` 把请求导到了 API 账户 | 有 Pro / Max 的：`/status` 看当前凭据，有 API Key 就按上一条 unset 掉，必要时 `/login` 用订阅账号登录；确实要用 API 的到 platform.claude.com/settings/billing 充值 | [错误参考](https://code.claude.com/docs/en/errors)、[用 Pro / Max 使用 Claude Code](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan) |
| 登录选项里没有"订阅账号"，只能选 Console | 版本旧，或旧登录状态没清掉 | `/logout` → `claude update` → 完全关掉终端再打开 → `claude` 重新选账号 | [用 Pro / Max 使用 Claude Code](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan) |
| `Login expired · Please run /login`（脚本模式是 `Failed to authenticate: OAuth session expired and could not be refreshed`） | 保存的登录过期且无法刷新，Claude Code 已清掉凭据；系统时间不准也会导致 | 运行 `/login` 重新登录；经常出现就校准系统时钟；自动化脚本改用 `claude setup-token` 生成的长期令牌或 API Key | [错误参考](https://code.claude.com/docs/en/errors)、[安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install) |
| `You've hit your session limit · resets 3:45pm` / `You've hit your weekly limit` / `Opus limit` / `Sonnet limit` | 5 小时窗口或每周额度用完；Opus / Sonnet 上限只针对该模型家族 | 等提示里的重置时间；`/usage` 看各窗口用量；Opus / Sonnet 上限时用 `/model` 换另一家族的模型；长期不够升级套餐，或 `/usage-credits` 开启用量额度 | [错误参考](https://code.claude.com/docs/en/errors) |
| `You've hit your monthly spend limit · raise it at ...` | 套餐额度和你设的用量额度每月上限都用完了 | Pro / Max 在 claude.ai Settings → Usage 调高上限，或运行 `/usage-credits`；有重置时间就等；Team / Enterprise 找管理员 | [错误参考](https://code.claude.com/docs/en/errors) |
| `Usage credits required for 1M context` | 选的是带 `[1m]` 后缀的 100 万上下文模型，你的套餐只能通过用量额度使用它 | `/model` 选不带 `[1m]` 的版本；或开启 `/usage-credits` 后重启 | [错误参考](https://code.claude.com/docs/en/errors) |
| `API Error: Server is temporarily limiting requests (not your usage limit)` | 服务端短时限流，和你的额度无关 | 稍等再试；持续出现看 status.claude.com | [错误参考](https://code.claude.com/docs/en/errors) |
| `API Error: Request rejected (429)` | 你的 API Key（或 Bedrock / Vertex 项目）的速率上限 | `/status` 确认当前凭据，有多余的 `ANTHROPIC_API_KEY` 先去掉；到服务商后台申请更高档；减少并行子任务或用 `/model` 换小模型 | [错误参考](https://code.claude.com/docs/en/errors) |
| `API Error: Repeated 529 Overloaded errors` | 服务端繁忙，官方说明不占你的额度 | 几分钟后重试，或用 `/model` 换一个模型（容量按模型分别统计）；看 status.claude.com | [错误参考](https://code.claude.com/docs/en/errors) |
| `API Error: 500 Internal server error` | 服务端内部故障，和你的提示、设置、账号无关 | 等一分钟重发（长提示直接输入 `try again`）；看 status.claude.com；持续出现用 `/feedback` 反馈 | [错误参考](https://code.claude.com/docs/en/errors) |
| `Request timed out` / `API Error: No response from API` | 网络慢、代理拖慢，或模型正在生成很长的回复（默认超时 10 分钟） | 重试；网络或代理慢的调大超时：`export API_TIMEOUT_MS=900000` | [错误参考](https://code.claude.com/docs/en/errors) |
| 充了会员还显示 Free Plan，或 Claude Code 让去充 API | 登录了别的邮箱；钱充到了 API 平台（Console）；付款失败被降级 | 退出重新登录、核对邮箱和登录方式；Settings → Billing 看付款状态；API 余额不会变成 Pro；Claude Code 里 `/login` 选订阅账号 | [账单 FAQ](https://support.claude.com/en/articles/8325618-paid-plan-billing-faqs)、[用 Pro / Max 使用 Claude Code](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan) |
| `Autocompact is thrashing: the context refilled to the limit...` | 自动压缩后某个大文件或工具输出立刻又把上下文填满 | 让它分段读大文件；`/compact` 时说明保留什么；大文件任务交给子代理；不需要旧对话就 `/clear` | [运行排错](https://code.claude.com/docs/en/troubleshooting) |
| 卡住不动、占用内存很高、搜索找不到文件 | 上下文太大；内置 ripgrep 在你的系统上跑不起来；WSL 跨文件系统慢 | `/compact` 压缩、重启后 `claude --continue` 接着聊；`claude --safe-mode` 排查插件；装系统 ripgrep 并设 `USE_BUILTIN_RIPGREP=0`；WSL 把项目放到 `/home/` 下 | [运行排错](https://code.claude.com/docs/en/troubleshooting) |

拿不准是哪条：在 Claude Code 里运行 `/doctor` 做自动检查；`claude` 根本起不来就在终端运行 `claude doctor`。账号、账单、订阅类的问题（登录循环、订阅不被识别、组织被停用）官方让联系支持：登录 claude.ai，点左下角头像 → Get help（Console 用户在 platform.claude.com）。

## 网络与代理

Claude Code 需要联网，要访问的主要域名有 `api.anthropic.com`（模型请求）、`claude.ai` 和 `platform.claude.com`（账号登录）、`downloads.claude.ai`（安装和更新）等。排查时可以先测试：

```bash
curl -I https://api.anthropic.com
```

Windows PowerShell 里用 `curl.exe -I https://api.anthropic.com`（PowerShell 的 `curl` 是另一个命令的别名）。如果你所在的公司或学校网络要求走代理，官方支持标准的代理环境变量（不支持 SOCKS 代理）：

```bash
export HTTPS_PROXY=https://proxy.example.com:8080
```

公司代理做 TLS 检查时，用 `export NODE_EXTRA_CA_CERTS=/path/to/ca-cert.pem` 让 Claude Code 信任代理的证书。代理地址、证书请向你的网络管理员获取。

## 常用命令和上手建议

| 命令 | 作用 |
|---|---|
| `/init` | 为项目生成 `CLAUDE.md` 说明文件 |
| `/status` | 查看版本、模型、账号和连接状态 |
| `/usage` | 查看套餐用量和各窗口的重置时间 |
| `/usage-credits` | 开启或查看用量额度（按 API 价计费） |
| `/model` | 切换模型 |
| `/clear` | 开始新对话、清空上下文 |
| `/compact` | 压缩当前对话，释放上下文 |
| `/login` `/logout` | 登录 / 退出 |
| `/doctor` | 检查安装和配置问题 |
| `/help` | 查看所有命令 |

命令会随版本增减，以 `/help` 显示为准。三个建议：在 git 管理的项目里用，改坏了随时退回；先用 `/init` 让它熟悉项目；一个任务一个对话，做完用 `/clear` 再开始下一个，对话越长越耗额度。

## 和 Codex 怎么选

| 对比项 | Claude Code | OpenAI Codex CLI |
|---|---|---|
| 订阅 | Claude Pro / Max | ChatGPT Plus / Pro 等 |
| 官方入门价 | $20/月（Pro） | $20/月（Plus） |
| 安装命令 | `curl -fsSL https://claude.ai/install.sh \| bash` | `curl -fsSL https://chatgpt.com/codex/install.sh \| sh` |
| 额度窗口 | 5 小时窗口 + 每周上限 | Plus 按 5 小时计算，Pro 目前没有 5 小时限制 |
| 启动命令 | `claude` | `codex` |

两个都是在终端里让 AI 读代码、改代码、跑命令的工具，很多人两个都用。偏好 Claude 的选 Claude Code，偏好 OpenAI 的选 Codex，拿不准就各试一个月。Codex 的安装和额度见 [OpenAI Codex CLI 国内使用指南](https://github.com/haiwaihuyonghui-rgb/codex-cli-guide-cn)。

## 常见问题

**Q：Claude Code 要单独付费吗？**
A：不用。Pro、Max、Team、Enterprise 都包含 Claude Code，用订阅账号登录即可；也可以用 Console 的 API Key 按量付费。免费版不含。

**Q：网页版和 Claude Code 的额度是分开的吗？**
A：不是。官方说明 claude.ai、桌面端、手机、编辑器插件和 Claude Code 的使用都计入同一份 5 小时和每周额度。

**Q：Max 自带的每月 API credits 能在 Claude Code 里用吗？**
A：不能。官方说明这些 credits 只能在 API 平台（Console）用，不能当 Claude Code 或 Claude 应用的额度，也不能抵用量额度。

**Q：怎么确认 Claude Code 现在是在用订阅还是在扣 API？**
A：输入 `/status`。显示 API Key 时按上面"订阅和 API 的优先级"处理；官方建议不想碰 API 账单就拒绝使用 API Key 的提示，并在被问到时不要添加 Console 凭据。

**Q：两个账号怎么切换？**
A：官方方法是 `CLAUDE_CONFIG_DIR`，给每个账号一个目录（见"登录"一节）；偶尔切换也可以 `/logout` 后 `/login`。

**Q：额度用完了有没有办法马上继续？**
A：有三个官方选项：等重置（`/usage` 看时间）、升级套餐（立即生效）、`/usage-credits` 开启用量额度按 API 价付费。

**Q：订阅、账单出问题找谁？**
A：Anthropic 官方支持：登录 claude.ai，点左下角头像 → Get help。代充开通的订单先找卖家。

## 国内开通参考

没有海外卡、想了解国内怎么开通 Claude Pro / Max 来用 Claude Code，以及选哪一档，看这一篇：[Claude Code 怎么收费？多少钱一个月，Pro 和 Max 怎么选](https://kw.yaoyao.chat/guide/claude-code.html)。

## 在哪买、质保怎么算

作者"瑶瑶ai训练师"自己经营一家 AI 会员代充商城，这里如实写明，读者可以据此判断本文立场。

- **商城**：[kw.yaoyao.chat](https://kw.yaoyao.chat/)（瑶瑶ai训练师 · AI会员充值）；官网 [www.yaoyao.chat](https://www.yaoyao.chat/)。
- **能买什么**：ChatGPT Plus（一个月卡密自助开通，1～3 分钟到账；十二个月人工开通）、ChatGPT Pro、Claude Pro / Max、Gemini Pro、Grok 会员。都是充到你自己的账号，不卖成品号、不拼车。
- **价格**：以商城页面为准。各档价格和官方价对比见 [ChatGPT Plus 多少钱一个月](https://kw.yaoyao.chat/guide/chatgpt-plus-price.html)、[Claude 会员多少钱](https://kw.yaoyao.chat/guide/claude-price.html)。
- **质保**（全部商品通用，原文见 [售后与质保规则](https://kw.yaoyao.chat/guide/shouhou-zhibao.html)）：
  1. **没开通成功**：卡密不消耗、自动退回，可以重新提交；不想再试的，联系微信退款。
  2. **开通成功后掉会员**（订阅被撤销、账号变回 Free）：在这一期会员有效期内，凭订单号和截图联系微信，再补一个月，补到同一个账号。
  3. **账号被封 / 停用**：原因多在账号使用、网络环境或付款侧，不在质保范围、不退款；ChatGPT 封号可协助重新注册，Claude 封号协助申诉。
- **售后**：只找瑶瑶本人微信 yao77380。
- **怎么分辨真假**：瑶瑶的网址只有 www.yaoyao.chat、kw.yaoyao.chat、cz.yaoyao.chat 三个；其他自称"瑶瑶"的网站、账号或软件都不是本人。


## 参考资料

- [安装和系统要求](https://code.claude.com/docs/en/setup)
- [登录和认证](https://code.claude.com/docs/en/authentication)
- [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install)
- [运行时排错（性能、搜索）](https://code.claude.com/docs/en/troubleshooting)
- [错误参考（额度、认证、服务端错误）](https://code.claude.com/docs/en/errors)
- [网络与代理配置](https://code.claude.com/docs/en/network-config)
- [命令参考](https://code.claude.com/docs/en/commands)
- [Anthropic 官方定价页](https://claude.com/pricing)
- [支持的国家和地区](https://www.anthropic.com/supported-countries)
- [What is the Pro plan?](https://support.claude.com/en/articles/8325606-what-is-the-pro-plan)
- [What is the Max plan?](https://support.claude.com/en/articles/11049741-what-is-the-max-plan)
- [Use Claude Code with your Pro or Max plan](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan)
- [Manage usage credits for paid Claude plans](https://support.claude.com/en/articles/12429409-manage-usage-credits-for-paid-claude-plans)
- [Paid plan billing FAQs](https://support.claude.com/en/articles/8325618-paid-plan-billing-faqs)
- [How to get support](https://support.claude.com/en/articles/9015913-how-to-get-support)
- 同一作者的其他仓库：[OpenAI Codex CLI 国内使用指南](https://github.com/haiwaihuyonghui-rgb/codex-cli-guide-cn)、[Claude 充值指南](https://github.com/haiwaihuyonghui-rgb/claude-chongzhi)、[GPT代充指南](https://github.com/haiwaihuyonghui-rgb/gpt-chongzhi)


## 复测记录

按日期倒序。每次只记真实做过的核对；官方出新模型、改价格时补一行。

| 日期 | 核对了什么 | 结果 |
|---|---|---|
| 2026-10-26 | **官方价格：** Claude 定价页、帮助中心 What is the Max plan | Pro 月付 20 美元 / 年付折合 17 美元（一次 200 美元）；Max 5x 100、Max 20x 200 美元；Max 含每月 API 额度；官方注明手机端价格可能不同——与本页价格表一致 |
| 2026-10-26 | **官方政策：** Anthropic 支持地区列表、账单 FAQ、赠送订阅文章 | 大陆、香港、澳门不在支持列表，台湾在；App 内订阅由苹果 / 谷歌管理；赠送订阅暂停购买 |

---

整理：瑶瑶ai训练师（AI 内容创作者），[官网](https://www.yaoyao.chat/)。本仓库为非官方教程整理，与 Anthropic 没有隶属关系，品牌名仅用于说明。瑶瑶没有发布过任何桌面软件、手机 App、浏览器插件或"账号切换器"，本账号下只有文字教程仓库。内容采用 CC-BY-4.0 许可，转载请注明出处。
