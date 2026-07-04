# tiecode

> **AI Agent 工作台 —— 创建、管理、远程控制你的 AI Agent**
> 
> AI Agent Workbench – Create, Manage, and Remotely Control Your AI Agents
> 
> Rust 编写，单二进制（~18MB），跨平台（Linux / Windows）
>
> 📦 本仓库仅提供编译后的可执行文件
>
> Welcome! Your feedback and suggestions are appreciated.

<!-- 📸 SCREENSHOT_01_HERO: 建议放置 GUI 主界面全貌截图，展示：左侧 Agent 列表 + 中间对话窗口（含代码输出）+ 右侧或顶部会话标签页。宽 1000-1200px，这是 README 的首屏图，需要最有吸引力的一张。 -->

---

## 这是什么？

tiecode 是一个完整的 **AI Agent 工作台**。它不仅是一个能自主编程的 AI Agent，还提供了一套完整的 Agent 管理、创建、远程控制和行为分析系统。

简单来说，你可以在 tiecode 里：

- 🗣️ 用自然语言给 Agent 派任务，它读代码 → 写代码 → 跑测试 → 修 bug，循环直到完成
- 🤖 创建多个不同角色的 Agent（开发/理财/学习/自定义），一键切换
- ✨ 用 AI 帮你设计 AI Agent — 写一句粗糙描述，自动生成结构化提示词
- 📋 启用计划模式，让 Agent 先拆解步骤再执行，你可以审查每一步
- 🔗 在飞书/Telegram 里远程跟 Agent 对话，随时随地操控代码仓库
- 📊 自动分析 Agent 的工作行为，生成用户画像和问题追踪报告

**这是一个可以让你轻松拥有一个 AI Agent 团队的工具。**

---

## 快速开始

### 1. 下载

从 [GitHub Releases](https://github.com/TieCoolLe/Tie) 下载对应平台的二进制文件：

| 平台 | 文件 |
|------|------|
| Linux x86_64 | `tiecode-vX.Y.Z-x86_64` |
| Windows x86_64 | `tiecode-vX.Y.Z-x86_64.exe` |

```bash
# Linux
chmod +x tiecode-vX.Y.Z-x86_64
sudo mv tiecode-vX.Y.Z-x86_64 /usr/local/bin/tiecode

# Windows
# 下载 .exe，双击启动 GUI，或放在 PATH 目录下命令行使用
```

### 2. 配置 API Key

```bash
# 方式一：环境变量
export ZAI_API_KEY="sk-your-api-key"

# 方式二：交互式配置向导（推荐首次使用）
tiecode --config-wizard
```

支持所有兼容 OpenAI Chat Completions API 的服务商：DeepSeek、智谱 GLM、通义千问、OpenAI、Ollama（本地模型）等。

### 3. 开始使用

```bash
# 命令行 — 一句话交任务
tiecode "写一个 Python 脚本，读取 CSV 并生成统计报告"

# GUI — 浏览器中打开完整工作台
tiecode --gui

# 切换到不同的 Agent
tiecode --agent taiyi "帮我分析这个项目的架构"
```

<!-- 📸 SCREENSHOT_02_GUI_TASK: GUI 中一次完整任务执行的截图，展示 Agent 读取文件 → 写代码 → 运行测试 → 输出结果的全流程。 -->

---

## 核心功能

### 🤖 Agent 平台 — 不止一个 AI

tiecode 内建完整的 Agent 管理系统。你不是在跟一个 AI 对话，而是可以创建和管理**多角色 Agent 团队**。

#### 4 个内置预设

| Agent | 角色 | 擅长 |
|-------|------|------|
| **太乙** (taiyi) | 通用 AI 助手 | 多场景辅助：读文件、写代码、搜索、解析文档、任务管理 |
| **开发老哥** (dev-bro) | 全栈工程师 | 写代码、修 Bug、重构、架构设计，独立完成开发到交付 |
| **理财顾问** (finance) | 财务分析师 | 收支分析、储蓄建议、投资组合评估（不推荐具体产品） |
| **学习教练** (study-coach) | 教学导师 | 费曼学习法、分阶段教学、编程实践指导 |

每个 Agent 独立配置：系统提示词、可用工具集、权限模式、LLM 模型、温度参数。

<!-- 📸 SCREENSHOT_03_AGENT_SIDEBAR: Agent 侧边栏特写，展示 4 个内置 Agent 列表（带图标）、右键菜单（编辑/删除/重命名）。 -->

#### 一键创建自定义 Agent

1. 在 GUI 中点「新建 Agent」
2. 填写名称和角色描述（随便写，不需要结构化）
3. 点击 **「✨ AI 增强」**— tiecode 自动将粗糙描述转换为六要素结构化提示词
4. 评估报告展示质量评分（20+ 项启发式检查）
5. 满意后保存，立即使用

```bash
# 也可以在命令行创建
tiecode --create-agent
```

<!-- 📸 SCREENSHOT_04_AGENT_CREATOR: Agent Creator 界面 — 左侧原始输入「你是一个 API 安全审计员」，右侧 AI 增强后的结构化结果：身份定义/认知框架/决策规则/工作流/约束/输出规范。底部显示质量评估分数。 -->

#### Agent Creator：AI 设计 AI

Agent Creator 是 tiecode 的元 Agent 功能——用 AI 帮你设计 Agent 的 system prompt。

**工作原理：**

1. 你输入一段自然语言描述（甚至可以是"帮我做一个代码审查的 agent"）
2. 元 Agent 按六要素架构（身份→认知→决策→工作流→约束→输出）生成结构化 prompt
3. 质量评估器跑 20+ 项启发式检查，生成评分报告
4. 提示词用 **AES-GCM 加密**存储到 `~/.tiecode/agents/<name>.yaml`
5. 运行时透明解密，原始输入保留用于后续再增强

**质量评估维度：**

| 维度 | 检查项 |
|------|--------|
| 身份 | 角色描述、能力声明、语气风格、默认偏好 |
| 认知 | 分析维度≥2、优先级排序 |
| 决策 | WHEN/THEN 规则数、BECAUSE 原因说明、数字阈值、分级标记 |
| 工作流 | 步骤数≥3、入口/出口条件、异常处理 |
| 约束 | 操作禁区、确认门槛、求助门槛 |
| 输出 | 格式描述、符号约定、摘要优先 |
| 示例 | Few-shot 数量≥2 |

---

### 🔧 工具系统 — Agent 能做什么

tiecode 为 Agent 提供 30+ 个工具，覆盖文件操作、命令执行、版本控制、网络搜索、代码分析等。

#### 文件操作

| 工具 | 功能 |
|------|------|
| `read_file` | 读取文件（支持分页 limit/N） |
| `write_file` | 写入文件 |
| `edit_file` | 精确编辑（替换指定位置的文本） |
| `apply_patch` | 应用 unified diff 补丁 |
| `list_dir` | 列出目录结构 |
| `search_files` | 按 glob 模式搜索文件 |
| `diff_files` | 生成 unified diff（纯 Rust LCS 算法） |

#### 文档与数据处理

| 工具 | 功能 |
|------|------|
| `read_document` | 读取 Office 文档：docx/xlsx/xls/ods/pdf/csv，程序化解析 |
| `read_html` | HTML→纯文本提取（剔除标签/样式/脚本，节省 90% token） |
| `wordstongue` | 大文件分段索引 + 指纹识别 + 按需读取/清洗 |

#### 命令执行

| 工具 | 功能 |
|------|------|
| `run_shell` | 执行 Shell 命令，三级风险分类 |
| 解释器扫描 | 自动检测 python/node/ruby 等解释器脚本内容，拦截危险模式 |

#### 版本控制（Git）

| 工具 | 功能 |
|------|------|
| `git_status` | 查看仓库状态 |
| `git_diff` | 查看变更 |
| `git_log` | 查看提交历史 |
| `git_commit` | 提交变更（格式：`type: 中文简述`） |
| `git_branch` | 查看分支信息 |

#### 代码分析

| 工具 | 功能 |
|------|------|
| `code_review` | 代码审查：安全漏洞、错误处理、TODO 密度、unwrap 统计 |
| `code_test` | 运行测试（自动检测 Rust/Python/Node 框架） |
| `code_refactor` | 代码重构（insert/replace/delete 操作） |
| `lsp_*` | LSP 集成：定义跳转、引用查找、诊断、悬停信息 |

#### 网络与搜索

| 工具 | 功能 |
|------|------|
| `web_search` | 多引擎搜索（Tavily/Brave/Baidu/Bing/SearXNG 降级链） |
| `web_fetch` | 抓取网页内容并提取正文 |

#### 任务与协作

| 工具 | 功能 |
|------|------|
| `todo_write` / `todo_show` | 任务管理，支持 checklist 进度追踪 |
| `spawn_agent` | 创建子 Agent 处理独立子任务（隔离上下文，15 轮限制） |
| `fork_agents` | N 路并行 Agent 分支（共享上下文，互不干扰） |
| `list_agents` | 列出可用 Agent（含角色、工具、权限摘要） |

#### 调度与监控

| 工具 | 功能 |
|------|------|
| `cron_add/list/remove` | Agent 可自行创建定时任务 |
| `heartbeat_configure/status` | Agent 配置自检心跳 |

#### 扩展能力

| 能力 | 说明 |
|------|------|
| **MCP 协议** | JSON-RPC stdio 客户端，自动注册外部 MCP 服务工具 |
| **自定义插件** | YAML 配置文件定义插件（Shell 命令、超时、作用域模式匹配） |

每个 Agent 可**按角色过滤工具**——比如理财顾问 Agent 看不到 shell/git/LSP 工具，只能看到文件/搜索/TODO 工具。

<!-- 📸 SCREENSHOT_05_TOOLS: Agent 设置表单的「启用工具」区域截图，展示可选的工具清单及勾选状态。 -->

---

### 📋 计划模式 — 先审查再执行

当你面对复杂任务时，可以让 Agent 先做计划再动手。

**工作流程：**

1. Agent 分析需求，拆解为 3~8 个可独立验证的子步骤
2. 构建步骤间的依赖关系（DAG）
3. 以**只读模式**探索代码库（只能读文件、搜索、diff，不能写）
4. 输出步骤计划供你审查
5. 你确认后，Agent 逐步执行，每步完成即验证

```bash
# 在 REST API 中启用计划模式
curl -X POST http://localhost:9876/api/task \
  -H "Content-Type: application/json" \
  -d '{"task": "实现用户认证模块", "plan": true}'
```

在 GUI 中，启动对话前勾选「计划模式」即可。

> **注意**：计划模式通过 REST API 和 GUI 提供，CLI 模式下暂不直接支持（先通过 `tiecode --serve` 启动服务后使用 API）。

<!-- 📸 SCREENSHOT_06_PLAN: 计划模式界面 — 左侧步骤列表（✅已完成 ⏳进行中 ⏸等待），右侧当前步骤的 Agent 输出。每个步骤有预估时间和依赖关系箭头。 -->

---

### 🖥️ Web GUI — 完整工作台

`tiecode --gui` 启动后自动打开浏览器，提供完整的工作台体验。

<!-- 📸 SCREENSHOT_07_GUI_FULL: GUI 整体布局截图，标注各区域功能。 -->

**界面组成：**

| 区域 | 功能 |
|------|------|
| **Agent 侧边栏** | 列出所有 Agent（图标+名称），点击切换，右键编辑/删除 |
| **会话标签页** | 多会话并行，Tab 切换、Pin 固定、右键菜单（关闭/关闭其他/关闭全部） |
| **聊天窗口** | WebSocket 实时流式输出，支持 Markdown 渲染和代码块高亮 |
| **设置面板** | 工作区路径、权限模式、LLM 提供商/模型、温度、思考深度 |
| **外部会话区** | 实时显示飞书/Telegram Bot 活跃会话，点击订阅查看 |

<!-- 📸 SCREENSHOT_08_SESSION_TABS: 多个会话标签页并排展示 — 不同 Agent 执行不同任务的实时输出。 -->

**浏览器兼容：**

- **Linux**：自动检测 Chrome/Chromium（`--app=` 无地址栏模式）→ Firefox（userChrome.css 方案）
- **Windows**：双击 `.exe` 自动启动 GUI 并打开浏览器，控制台窗口自动隐藏

---

### 🔗 飞书 & Telegram Bot — 远程操控

把 tiecode 接入飞书或 Telegram，在手机 IM 里直接跟 Agent 对话。

<!-- 📸 SCREENSHOT_09_BOT_SETTINGS: 设置面板中的「飞书 Bot」配置界面 — Bot Token、Agent 选择、LLM 设置。 -->

**飞书 Bot：**

- WebSocket 长连接（无需公网 IP，主动连接飞书事件网关）
- 双向实时通信，多轮对话保持会话上下文
- 每个 Bot 可独立配置 Agent、LLM 模型、工作区、权限

**Telegram Bot：**

- 长轮询（getUpdates），无需公网 IP
- 支持双向多轮对话
- 独立配置每个 Bot 的参数

**外部会话实时同步：**

在 GUI 左侧「外部会话」区域，你可以：

- 查看所有活跃的飞书/Telegram 会话
- 点击订阅，实时查看 Agent 的输出（和手机端同步）
- 查看历史消息记录
- 在 GUI 中直接回复（多端口控制同一会话）

<!-- 📸 SCREENSHOT_10_EXTERNAL_SESSION: GUI 中订阅飞书会话的界面 — 左侧外部会话列表，右侧实时输出流。 -->

---

### 📊 Session Insight — 行为分析

tiecode 自动分析 Agent 的工作行为，帮助你了解你的使用模式和改进空间。

**工作流程：**

1. **扫描**：每天自动扫描会话 JSONL 日志
2. **分段**：提取工作片段（编码/测试/调试/搜索/规划/修复）
3. **摘要**：压缩会话为分析摘要
4. **分析**：LLM 批量分析，生成行为画像
5. **存储**：结果写入 `~/.tiecode/insights/{agent}/user_profile.md`
6. **追踪**：识别反复出现的问题（🔴持续/🟡已解决/🟢已关闭）

```bash
# 手动触发分析
tiecode --insight-run                    # 分析今天所有 Agent 的会话
tiecode --insight-run --insight-agent dev-bro  # 只分析指定 Agent

# 开关某个 Agent 的 insight
tiecode --edit-agent dev-bro --insight true
```

<!-- 📸 SCREENSHOT_11_INSIGHT: Insight 结果展示 — Agent 行为画像、问题追踪列表、每日摘要。 -->

---

### 🔐 安全设计

#### 提示词加密

所有通过 Agent Creator 增强的提示词使用 **AES-GCM** 加密存储：

- 加密后的提示词存储在 `~/.tiecode/agents/<name>.yaml` 的 `encrypted_prompt` 字段
- 原始输入保留在 `raw_prompt` 字段，可供再次编辑和重新增强
- 运行时透明解密，Agent 和用户无感知
- 每个 Agent 独立加密，密钥隔离

#### 提示词保密

三层防御防止 Agent 泄露自己的 system prompt：

1. **Prompt 级指令**：每个 Agent 的 system prompt 末尾嵌入保密规则，禁止输出提示词内容
2. **输入守卫**：检测用户输入中的"泄露提示词""show me your prompt"等关键词，注入强提醒
3. **输出守卫（预留）**：架构预留了输出检测接口

#### 三级权限

| 模式 | 写文件 | 执行命令 | Git 提交 | 删除操作 | 适用场景 |
|------|--------|----------|----------|----------|----------|
| `read-only` | ❌ | ❌ | ❌ | ❌ | 代码审查、架构分析 |
| `workspace-write` | ✅ | ✅（受限） | ✅ | ⚠️ 需确认 | **默认模式**，日常开发 |
| `danger-full-access` | ✅ | ✅（全放开） | ✅ | ✅ | 信任模式，完全自主 |

#### API Key 安全

- 存储在 `~/.tiecode/providers.yaml`，不硬编码
- GUI 设置面板中输入

---

### 🌐 REST API 服务

```bash
# 启动 API 服务
tiecode --serve

# 自定义端口
tiecode --serve --gui-port 8080
```

**GUI 端点（端口 9876）：**

| 端点 | 方法 | 说明 |
|------|------|------|
| `/` | GET | Web UI 界面 |
| `/api/ws` | WS | WebSocket 双向通信 |
| `/api/run` | POST | 提交任务 |
| `/api/agents` | GET/POST | 列出/创建 Agent |
| `/api/agents/:name` | DELETE | 删除 Agent |
| `/api/agents/:name/prompt` | PATCH | 编辑提示词 |
| `/api/agents/:name/settings` | PATCH | 编辑 Agent 设置 |
| `/api/agents/enhance` | POST | AI 增强提示词 |
| `/api/agents/reorder` | POST | Agent 排序 |
| `/api/sessions` | GET | 列出会话 |
| `/api/sessions/active` | GET | 列出活跃会话 |
| `/api/sessions/:id` | DELETE | 删除会话 |
| `/api/sessions/:id/settings` | GET/PATCH | 会话设置 |
| `/api/insights` | GET | 列出 Insight 数据 |
| `/api/insights/:agent/profile` | GET | 查看 Agent 行为画像 |
| `/api/config` | GET | 获取配置 |
| `/api/config/providers` | GET/POST | 管理 LLM 提供商 |
| `/api/config/search` | GET/POST | 管理搜索配置 |
| `/api/bots/feishu` | GET/POST | 管理飞书 Bot |
| `/api/bots/telegram` | GET/POST | 管理 Telegram Bot |
| `/api/bots/feishu/test` | POST | 测试飞书 Bot 连接 |
| `/api/bots/telegram/test` | POST | 测试 Telegram Bot 连接 |
| `/api/mcp` | GET | 列出 MCP 服务 |
| `/api/plugins` | GET | 列出插件 |
| `/api/browse-dir` | GET | 浏览目录 |
| `/api/icon` | GET | 获取 Agent 图标（SVG） |
| `/api/setup/status` | GET | 初始设置状态 |
| `/api/setup/configure` | POST | 执行初始设置 |
| `/api/bye` | POST | 关闭服务器 |

**服务端点（--serve 模式任意端口）：**

| 端点 | 方法 | 说明 |
|------|------|------|
| `/api/health` | GET | 健康检查 |
| `/api/task` | POST | 提交任务（支持 `plan` 参数） |
| `/api/task/:id` | GET | 查询任务状态和步骤进度 |
| `/api/tasks` | GET | 列出所有任务 |
| `/api/agent` | WS | WebSocket Agent 实时通道 |

---

### ⌨️ CLI 全参数

```
tiecode [OPTIONS] [TASK]...

参数:
  [TASK]...  编程任务描述（可选，不提供则启动 GUI）

选项:
  -c, --config <PATH>         YAML 配置文件路径
  -w, --workspace <DIR>       工作目录 [env: TIECODE_WORKSPACE]
  --agent <NAME>              Agent 名称（默认: dev-bro）[env: TIECODE_AGENT]
  --model <NAME>              LLM 模型 [env: ZAI_MODEL]
  --api-key <KEY>             API Key [env: ZAI_API_KEY]
  --base-url <URL>            API 地址 [env: ZAI_BASE_URL]
  --provider <NAME>           服务商: deepseek/kimi/openai/ollama/modelscope
  --permission-mode <MODE>    权限: read-only/workspace-write/danger-full-access
  --max-turns <N>             最大对话轮数（默认 50）
  --thinking <LEVEL>          思考深度: off/low/medium/high/max
  --log-level <LEVEL>         日志级别: trace/debug/info/warn/error

  --gui                       启动 Web 工作台
  --gui-port <PORT>           GUI 端口（默认 9876）
  --install                   安装桌面快捷方式
  --serve                     启动 REST API 服务
  --notify-feishu <URL>       飞书通知 webhook
  --config-wizard             交互式配置向导

  --create-agent              创建新 Agent
  --list-agents               列出所有 Agent
  --edit-agent <NAME>         编辑 Agent（配合 --insight）
  --insight <BOOL>            开关 Agent Insight [values: true/false]
  --insight-run               运行行为分析
  --insight-agent <NAME>      指定分析的目标 Agent

  --sessions                  列出最近会话
  --resume <ID>               恢复指定会话
  --resume-last               恢复最近未完成的会话

  --json                      JSON 格式输出
  -v, --verbose               详细输出
```

---

### 🧠 上下文管理

tiecode 使用真实 token 计数（tiktoken-rs `o200k_base`），在 128K token 阈值触发自动压缩：

- 旧消息批量压缩为 LLM 生成的语义摘要
- 保留压缩边界的安全间隔，避免截断工具调用序列
- 每条 JSONL 日志带本地时间戳，支持精确恢复
- 多个压缩摘要合并时通过 LLM 做增量合并，避免信息丢失

---

### 📦 会话管理

- 会话自动保存到 `~/.tiecode/sessions/{agent}/`（JSONL 格式）
- 支持 `--resume` / `--resume-last` 恢复未完成会话
- `--sessions` 查看历史会话列表（含回合数、工具调用数、状态）
- 支持从 GUI 重新连接到之前的会话
- 会话可跨通道共享（飞书↔GUI↔Telegram 互操作）

---

## 为什么用 Rust 写？

| 特性 | 优势 |
|------|------|
| **单文件部署** | ~18MB 静态编译，扔到服务器上就能跑，零依赖 |
| **启动快** | 毫秒级启动，适合 Bot 和 API 服务 |
| **内存低** | Agent 常驻后台几乎不占资源 |
| **跨平台** | 同一份代码编译 Linux + Windows |
| **可靠性** | 编译期保证类型安全，没有运行时异常 |

---

## 配置

tiecode 的数据目录为 `~/.tiecode/`：

```
~/.tiecode/
├── agents/              # Agent 定义（YAML，含加密提示词）
│   ├── taiyi.yaml
│   ├── dev-bro.yaml
│   └── ...
├── sessions/            # 会话日志（JSONL，按 Agent 分目录）
│   └── {agent}/
│       └── {session_id}.jsonl
├── memory/              # Agent 长期记忆（Markdown）
│   └── {agent}.md
├── insights/            # 行为分析数据
│   ├── {agent}/
│   │   ├── user_profile.md
│   │   ├── daily/
│   │   └── problems.md
│   └── state.json
├── providers.yaml       # LLM 提供商配置（含加密 API Key）
├── search.yaml          # 搜索服务配置
├── mcp.yaml             # MCP 服务配置
├── plugins.yaml         # 自定义插件配置
├── scheduler.yaml       # 定时任务配置
├── feishu.yaml          # 飞书 Bot 配置
├── telegram.yaml        # Telegram Bot 配置
└── external_settings/   # 外部通道 Bot 独立设置
    └── {bot_id}.json
```

---

## 文档

- [Agent 设计方法论](docs/agent-design-methodology.md) — 五层经验模型、六要素架构
- [Agent 设计哲学](docs/agent-design-philosophy.md) — 为什么结构化 prompt 有效
- [多 Agent 平台方案](docs/multi-agent-platform.md)
- [Session Insight 设计](docs/insight-design.md) — 行为分析系统设计
- [提示词保密设计](docs/prompt-confidentiality-design.md) — 三层防御架构
- [数据飞轮策略](docs/data-flywheel-strategy.md)
- [追加式压缩设计](docs/append-only-compress.md)
- [思考深度设计](docs/thinking-effort-design.md)
