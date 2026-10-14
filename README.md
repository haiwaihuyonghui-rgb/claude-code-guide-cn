# Claude Code 国内使用指南（2026）

**直接回答：** Claude Code 是 Anthropic 官方的终端编程助手，不单独收费：开通 Claude Pro 或 Max 订阅，运行 `claude` 命令登录就能用；也可以用 Console 的 API Key 按量付费。

**安装：** macOS / Linux / WSL 运行 `curl -fsSL https://claude.ai/install.sh | bash`，Windows PowerShell 运行 `irm https://claude.ai/install.ps1 | iex`，装完执行 `claude --version` 确认。

**选套餐：** 偶尔用选 Pro，每天用几个小时选 Max 5x，几乎全天跑选 Max 20x。额度按 5 小时窗口加每周上限计算，网页和 Claude Code 共用一份。

> 更新日期：2026-10-14。安装步骤和报错文字核对自 Anthropic 官方文档，价格以官方页面为准；本仓库非官方，与 Anthropic 没有隶属关系。

## 目录

- [使用前先读：关于地区](#使用前先读关于地区)
- [需要什么账号](#需要什么账号)
- [安装](#安装)
- [登录](#登录)
- [Pro 和 Max 额度怎么选](#pro-和-max-额度怎么选)
- [常见报错与解决](#常见报错与解决)
- [网络与代理](#网络与代理)
- [常用命令和上手建议](#常用命令和上手建议)
- [和 Codex 怎么选](#和-codex-怎么选)
- [开通会员](#开通会员)
- [官方文档](#官方文档)

## 使用前先读：关于地区

Anthropic 官方公布了支持的国家和地区列表（[anthropic.com/supported-countries](https://www.anthropic.com/supported-countries)），页面写明未列出的国家和地区不受支持，中国大陆不在列表中。官方文档里对应的提示是 `App unavailable in region`，意思是 Claude Code 在你所在的国家不可用。本指南只整理官方的安装、登录、订阅和报错说明，不提供绕过地区限制的方法；请在符合 Anthropic 服务条款的前提下使用。

## 需要什么账号

官方要求使用 Pro、Max、Team、Enterprise 或 Console 账号，免费版 claude.ai 不包含 Claude Code。

| 登录方式 | 怎么计费 | 适合谁 |
|---|---|---|
| Claude Pro / Max 订阅账号 | 含在会员里，有 5 小时窗口和每周上限，和网页版共用额度 | 个人开发者 |
| Console 账号 / API Key | 按 token 用量扣 API 余额，用得多费用涨得快 | 脚本、自动化、公司统一结算 |
| Team / Enterprise | 按席位，管理员邀请 | 团队 |
| Amazon Bedrock 等云平台 | 走云平台账单 | 企业 |

## 安装

系统要求（官方）：macOS 13.0+、Windows 10 1809+ 或 Windows Server 2019+、Ubuntu 20.04+、Debian 10+、Alpine Linux 3.19+；内存 4 GB+；需要联网；Shell 支持 Bash、Zsh、PowerShell、CMD。

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

官方提醒：不要用 `sudo npm install -g`，会引发权限问题。

安装完成后，打开一个新的终端窗口验证：

```bash
claude --version
claude doctor
```

`claude --version` 会打印版本号，`claude doctor` 打印安装和配置的诊断信息。之后进入你的项目目录，运行 `claude` 启动。需要手动更新时运行 `claude update`。

Windows 说明：原生 Windows 建议安装 Git for Windows（可选），Claude Code 会用 Git Bash 作为 Bash 工具；不装就用 PowerShell。需要 Linux 工具链或沙盒时，可以在 WSL 里运行 Linux 的安装命令。

## 登录

1. 在项目目录运行 `claude`，首次启动会自动打开浏览器让你登录。
2. 浏览器没有自动弹出：按 `c` 复制登录链接，粘贴到浏览器。
3. 登录后浏览器显示的是一串登录码而不是自动跳回：粘贴到终端的 `Paste code here if prompted` 提示处（WSL2、SSH、容器里常见）。
4. 终端显示 `Login successful` 后按 `Enter` 继续。

登录相关命令：

- `/login`：重新登录或切换账号；`/logout`：退出登录。
- `/status`：查看当前账号、登录方式和连接状态。
- 粘贴登录码没反应：在终端里运行 `claude auth login`，它从标准输入读取登录码。

**订阅和 API 的优先级。** 如果电脑环境变量里设置了 `ANTHROPIC_API_KEY`，并且你在提示里同意使用，Claude Code 会优先用这个 Key 按 API 计费，而不是你的订阅。官方的处理办法：运行 `unset ANTHROPIC_API_KEY`（Windows PowerShell 用 `Remove-Item Env:ANTHROPIC_API_KEY`），再用 `/status` 确认当前用的是哪种方式；要永久生效，把 `~/.zshrc`、`~/.bashrc` 里对应的 `export` 行删掉。

## Pro 和 Max 额度怎么选

| 套餐 | 官方价 | 用量 | 适合谁 |
|---|---|---|---|
| Free | $0 | 不含 Claude Code | 偶尔问几个问题 |
| Pro | $20/月；年付一次交 $200，折合约 $17/月 | 基准 | 偶尔用 Claude Code 写脚本 |
| Max 5x | $100/月起 | 每 5 小时用量是 Pro 的 5 倍 | 每天用几个小时 |
| Max 20x | $200/月 | 每 5 小时用量是 Pro 的 20 倍 | 几乎全天在跑 |

官方定价页（claude.com/pricing）说明：每个套餐的用量按滚动 5 小时窗口重置，付费套餐在此之外还有每周上限，官方也可能设置其他上限；Max 还附带每月 API credits。网页、桌面端、手机和 Claude Code 的使用都从同一份额度里扣。

- 不确定就先买 Pro 用一个月，一周里好几天提示额度用完，再升 Max 5x。
- 想看当前用量和重置时间：在 Claude Code 里输入 `/usage`。
- 额度用完后想继续：等重置、升级套餐，或者用 `/usage-credits` 开启额外用量（按标准 API 价格计费，不想多花钱就不要开）。

## 常见报错与解决

下面的报错文字和处理办法来自官方文档，以官方最新说明为准。

| 你看到的 | 原因 | 怎么办 |
|---|---|---|
| `command not found: claude` / `'claude' is not recognized` | 安装目录不在 PATH 里 | 新开一个终端；macOS / Linux 把 `~/.local/bin` 加进 PATH（`echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc` 然后 `source ~/.zshrc`），Windows 对应目录是 `%USERPROFILE%\.local\bin` |
| `syntax error near unexpected token '<'` 或 `curl: (22) ... 403` | 安装地址返回的是网页而不是安装脚本 | 等几分钟重试；改用 Homebrew 或 WinGet 安装；如果输出里有 `App unavailable in region`，说明当前所在国家不受支持 |
| `Failed to fetch version from downloads.claude.ai` | 网络无法访问下载服务器 | 检查网络和代理设置，见下文"网络与代理" |
| `TLS connect error` 等 TLS 报错 | 系统根证书过旧或代理干扰 | 更新系统 CA 证书；公司代理需要让它信任代理的证书 |
| `Unable to connect to Anthropic services` | 启动时连不上 api.anthropic.com | 检查代理变量是否正确、网络是否放行；官方提示网络正常仍失败时，可能是当前国家不可用 |
| `OAuth error: Invalid code` | 登录码过期或复制不完整 | 回车重试，浏览器打开后尽快完成；按 `c` 复制完整链接 |
| `API Error: 403 Request not allowed` | 订阅未生效或账号角色不对 | 在 claude.ai/settings 确认订阅有效；代理环境见"网络与代理" |
| `This organization has been disabled`（但订阅有效） | 环境变量里有旧的 `ANTHROPIC_API_KEY` | `unset ANTHROPIC_API_KEY` 后重新运行 `claude` |
| `Login expired · Please run /login` | 登录过期 | 运行 `/login` 重新登录；系统时间不准也会导致 |
| `You've hit your session limit · resets 3:45pm` | 5 小时窗口额度用完 | 等重置时间，或升级套餐，`/usage` 查看用量 |
| 充了会员还显示 Free Plan | 登录了别的账号，或把钱充到了 API 平台 | 退出重新登录、核对邮箱；API 余额不会变成 Pro |
| `API Error: Repeated 529 Overloaded errors` | 服务端繁忙，不占你的额度 | 几分钟后重试，或用 `/model` 换一个模型 |
| `Request timed out` | 网络慢或代理拖慢 | 重试，或调大超时：`export API_TIMEOUT_MS=900000` |

## 网络与代理

Claude Code 需要联网，要访问的主要域名有 `api.anthropic.com`（模型请求）、`claude.ai` 和 `platform.claude.com`（账号登录）、`downloads.claude.ai`（安装和更新）等。排查时可以先测试：

```bash
curl -I https://api.anthropic.com
```

Windows PowerShell 里用 `curl.exe -I https://api.anthropic.com`。如果你所在的公司或学校网络要求走代理，官方支持标准的代理环境变量（不支持 SOCKS 代理）：

```bash
export HTTPS_PROXY=https://proxy.example.com:8080
```

公司代理做 TLS 检查时，用 `export NODE_EXTRA_CA_CERTS=/path/to/ca-cert.pem` 让 Claude Code 信任代理的证书。代理地址、证书请向你的网络管理员获取。

## 常用命令和上手建议

| 命令 | 作用 |
|---|---|
| `/init` | 为项目生成 `CLAUDE.md` 说明文件 |
| `/status` | 查看版本、模型、账号和连接状态 |
| `/usage` | 查看会话花费、套餐用量和重置时间 |
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

## 开通会员

官方主要支持信用卡等海外付款方式，国内银行卡和支付宝付不了。如果你需要订阅代付（把会员开到你自己的账号上），2026 年 10 月商城标价如下，以商城为准：

| 套餐 | 商城标价 |
|---|---|
| [Claude Pro 一个月](https://kw.yaoyao.chat/item/12) | ¥150 |
| [Claude Max 5X 一个月](https://kw.yaoyao.chat/item/14) | ¥900 |
| [Claude Max 20X 一个月](https://kw.yaoyao.chat/item/13) | ¥1,800 |

商品均带订阅质保。更详细的选购说明见 [Claude Code 怎么收费](https://kw.yaoyao.chat/guide/claude-code.html) 和 [Claude 会员多少钱](https://kw.yaoyao.chat/guide/claude-price.html)。批量或代理合作请看 [官网](https://www.yaoyao.chat/)。

## 官方文档

- [安装和系统要求](https://code.claude.com/docs/en/setup)
- [登录和认证](https://code.claude.com/docs/en/authentication)
- [安装与登录排错](https://code.claude.com/docs/en/troubleshoot-install)
- [网络与代理配置](https://code.claude.com/docs/en/network-config)
- [命令参考](https://code.claude.com/docs/en/commands)
- [Claude 定价](https://claude.com/pricing)

---

整理：瑶瑶ai训练师（AI 内容创作者），[官网](https://www.yaoyao.chat/)。本仓库为非官方教程整理，与 Anthropic 没有隶属关系，品牌名仅用于说明。内容采用 CC-BY-4.0 许可，转载请注明出处。
