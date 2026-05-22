# Publisher CLI v1.0 — 需求提案

> 基于 SKILL.md（Publisher v5.5 观者范式）的 CLI 化实现

---

## 1. 目标确认

### 1.1 核心目标
把 `SKILL.md` 中定义的 S1-S7 硬隔断文章创作流程，从纯 Markdown 指令升级为**可执行的 Python CLI 工具**。利用命令行特性（多进程并行、文件持久化、状态机），解决当前纯 Skill 模式的三大痛点：

1. **硬隔断无法强制执行** — CLI 状态机确保阶段锁真正生效
2. **S2 并行搜索受限于 LLM 上下文** — CLI 多进程同时拉取多源素材
3. **过程产物无持久化** — 素材结构图、术语锁定表、核查结果落盘可追溯

### 1.2 非目标（明确排除）
- ❌ 不直接生成图片（S7 只输出提示词文本，保持 SKILL.md 现状）
- ❌ 不做 Web 界面或 IDE 插件（本次只做 CLI）
- ❌ 不做多用户协作或云端同步

### 1.3 成功标准
- [ ] 用户输入选题后，可通过独立命令逐步走完 S1-S7
- [ ] S2 并行搜索耗时 < 单线程串行的 40%
- [ ] S2.5/S6 核查结果可复现（同一份素材结构图 → 相同核查输出）
- [ ] 终端崩溃或中断后，可 `publisher status` 恢复现场

---

## 2. 输入 / 输出清单

### 2.1 用户输入（人能做什么）

| 动作 | 形式 | 说明 |
|------|------|------|
| 发起选题 | 命令参数 | `publisher init "某科技公司裁员"` |
| 推进阶段 | 子命令 | `publisher research / verify / draft / polish / finalize / audit / image` |
| 查看状态 | 查询命令 | `publisher status` 查看当前所处阶段和产物清单 |
| 配置 LLM | 配置文件 | `~/.publisher/config.yaml` 填写 API Key、模型选择 |
| 配置搜索 | 配置文件 | 选择搜索后端（LLM 自带 / Tavily / Serper）并填写 Key |
| 干预流程 | 选项 / 文件编辑 | S3 改写后可手动修改 `drafts/s3-draft.md` 再进入 S4 |

### 2.2 系统输出（系统展示什么）

| 阶段 | 终端输出 | 落盘文件 |
|------|---------|---------|
| S1 | 定调确认卡（Markdown 表格） | `workspace/{topic}/s1-brief.md` |
| S2 | 素材结构图摘要 + 术语锁定表 | `workspace/{topic}/s2-materials.md` |
| S2.5 | 核查报告（通过项 / 警告项 / 阻塞项） | `workspace/{topic}/s2-5-verify-report.md` |
| S3 | 改写正文预览（前 300 字） | `workspace/{topic}/s3-draft.md` |
| S4 | 优化简报（改动点 + 风格评分） | `workspace/{topic}/s4-polished.md` |
| S5 | 标题建议（3 个）+ 终稿路径 | `workspace/{topic}/s5-final.md` |
| S6 | 事实核查结果 + 审核风险分级 + 反常识追问 | `workspace/{topic}/s6-audit-report.md` |
| S7 | 英文配图提示词（可直接复制使用） | `workspace/{topic}/s7-image-prompt.md` |

---

## 3. 步骤 / 流程确认

### 3.1 命令级交互流程（硬隔断）

```bash
# S1 选题定调
$ publisher init "某科技公司裁员"
→ 输出定调确认卡
→ 写入 s1-brief.md

# S2 素材拆解（CLI 并行搜索核心场景）
$ publisher research
→ 同时启动 3 个进程/线程：
   ├─ Agent-A（事件线）：核心事件、时间点、当事人动作
   ├─ Agent-B（行业线）：行业数据、市场规模、政策
   └─ Agent-C（深度线）：论文原理、历史案例、反常识角度
→ Master 汇总为素材结构图
→ 强制输出术语锁定表
→ 写入 s2-materials.md

# S2.5 初稿前全面核查（重点）
$ publisher verify
→ 逐项执行 9 大核查：
   1. 引用分级（A/B/C/U）
   2. 强制交叉验证
   3. 联网事实核查
   4. 商业合理性校验
   5. AI 套话预清理
   6. LLM 幻觉预扫描
   7. 穿着/物品跨时间锁定
   8. 时间轴校准
   9. 术语锁定表澄清
→ 输出核查报告，标记阻塞项
→ 写入 s2-5-verify-report.md

# S3 风格改写
$ publisher draft
→ 抽象词转具体场景、去 AI 味、爹味清理
→ 输出正文 + 引用列表
→ 写入 s3-draft.md

# S4 结构+语言优化
$ publisher polish
→ 小标题字数、段落节奏、逻辑流、禁用词扫描
→ 写入 s4-polished.md

# S5 输出终稿
$ publisher finalize
→ 生成标题建议、修改简报、引用列表
→ 写入 s5-final.md

# S6 全文扫描（硬隔断·安全门，重点）
$ publisher audit
→ 事实核查回溯（8 项幻觉扫描）
→ 审核风险分级（🔴高危/🟡中危/🟢低危）
→ 领域自检清单自动挂载
→ 反常识追问（3 条，必须用户确认后才能结束）
→ 写入 s6-audit-report.md

# S7 配图（可选）
$ publisher image
→ 询问四层数字 N-N-N-N
→ 技法-主体冲突预判
→ 复用 image-standard.md 模板输出提示词
→ 写入 s7-image-prompt.md
```

### 3.2 状态机设计

CLI 内部维护一个 `state.json`，记录当前所处阶段。每个子命令执行前检查：

- `research` 要求 `state.currentStage >= S1`
- `verify` 要求 `state.currentStage >= S2`
- `draft` 要求 `state.currentStage >= S2.5` 且 `verify.blockers == 0`
- 以此类推

如果阶段不满足，终端报错并提示用户应先执行哪个命令。

### 3.3 并行搜索架构（CLI 优势落地）

```
┌─────────────────────────────────────────┐
│         publisher research              │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐  │
│  │ Agent-A │ │ Agent-B │ │ Agent-C │  │
│  │ 事件线   │ │ 行业线   │ │ 深度线   │  │
│  │ (进程1)  │ │ (进程2)  │ │ (进程3)  │  │
│  └────┬────┘ └────┬────┘ └────┬────┘  │
│       └─────────────┼─────────────┘    │
│                     ▼                   │
│              Master 汇总                 │
│           素材结构图 + 术语表             │
└─────────────────────────────────────────┘
```

实现方式：Python `concurrent.futures.ThreadPoolExecutor`（I/O 密集型）或 `asyncio.gather`。

---

## 4. 搜索后端设计（"教给我"方案）

### 4.1 当前默认：LLM 自带搜索
无需 API Key，直接调用 LLM 的联网能力（如 Kimi 的 `search` 工具、GPT-4 的 browsing）。适合个人起步。

### 4.2 进阶可选：真实搜索 API（可插拔）
CLI 预留接口，用户配置 Key 后自动切换：

| 服务 | 免费额度 | 获取方式 | 特点 |
|------|---------|---------|------|
| **Tavily** | 1,000 次/月 | [tavily.com](https://tavily.com) → Sign Up → Dashboard 复制 Key | 专为 AI 设计，返回结构化摘要 |
| **Serper** | 2,500 次 | [serper.dev](https://serper.dev) → 注册即送额度 | Google Search API 封装，结果丰富 |
| **Exa** | 100 次/月 | [exa.ai](https://exa.ai) | 语义搜索，适合深度线 |

**配置方式**：
```yaml
# ~/.publisher/config.yaml
search:
  provider: tavily  # 可选: llm / tavily / serper / exa
  api_key: tvly-xxxxxxxx  # 从上述网站获取后填入
```

**切换逻辑**：CLI 启动时读取配置，根据 `provider` 加载对应搜索适配器。未配置 Key 时自动回退到 `llm` 模式并打印提示。

---

## 5. 技术栈

| 层级 | 选型 | 理由 |
|------|------|------|
| 语言 | Python 3.10+ | 生态成熟，LLM SDK（openai、zhipuai）丰富 |
| CLI 框架 | `click` 或 `typer` | 命令解析、参数校验、帮助文档自动生成 |
| 并行 | `asyncio` + `aiohttp` | 适合 I/O 密集型并发搜索 |
| 配置 | `pydantic-settings` + YAML | 类型安全，用户编辑友好 |
| 状态存储 | JSON 文件（`workspace/{topic}/state.json`） | 无需数据库， human-readable |
| LLM 调用 | `openai` SDK（兼容多数国内 API） | 统一接口，切换模型只需改 base_url |
| 文本处理 | `markdown-it-py`、`jinja2` | Markdown 解析、配图模板渲染 |

---

## 6. 目录结构预览

```
the-watcher-publisher-v5.5/
├── publisher/                  # CLI 源码包
│   ├── __init__.py
│   ├── cli.py                  # Click/Typer 命令注册
│   ├── stages/
│   │   ├── s1_init.py
│   │   ├── s2_research.py      # 并行搜索核心
│   │   ├── s2_5_verify.py      # 核查重点
│   │   ├── s3_draft.py
│   │   ├── s4_polish.py
│   │   ├── s5_finalize.py
│   │   ├── s6_audit.py         # 安全扫描重点
│   │   └── s7_image.py         # 配图提示词
│   ├── core/
│   │   ├── state.py            # 状态机
│   │   ├── config.py           # 配置管理
│   │   └── workspace.py        # 工作区管理
│   ├── search/
│   │   ├── base.py             # 搜索接口抽象
│   │   ├── llm_search.py       # LLM 自带搜索
│   │   ├── tavily_search.py    # Tavily 适配器
│   │   └── serper_search.py    # Serper 适配器
│   ├── checkers/               # 核查规则引擎
│   │   ├── citation.py         # 引用分级
│   │   ├── cross_verify.py     # 交叉验证
│   │   ├── hallucination.py    # 幻觉扫描
│   │   └── risk_audit.py       # 审核风险
│   └── templates/
│       └── image_prompts/      # image-standard.md 模板
├── docs/
│   └── proposal.md             # 本文件
├── references/                 # SKILL.md 规则原文
├── workspace/                  # 运行时产出（.gitignore）
├── config.yaml                 # 用户配置模板
├── pyproject.toml              # Python 包配置
└── README.md
```

---

## 7. 待确认清单（阶段一出口）

> 以下条目需全部确认后，才能进入阶段二（概要设计）。

| 序号 | 问题 | 用户答复 | 最终确认 |
|------|------|---------|---------|
| 1 | **LLM 提供商** | 使用当前 Kimi Code CLI 的 Kimi API（Moonshot AI） | ✅ 确认 |
| 2 | **S2 并行粒度** | 多线程（3 个 Agent 共享上下文，互相补充，速度快） | ✅ 确认 |
| 3 | **S6 反常识追问** | 终端实时输入 `过/查`（交互直观，不需要额外打开文件） | ✅ 确认 |
| 4 | **安装方式** | `git clone` + `pip install -e .` 开发模式（留在项目文件夹，随时改代码） | ✅ 确认 |
| 5 | **默认工作区位置** | 当前项目目录下 `./workspace/{topic}/`（产物就近存放，方便查看） | ✅ 确认 |

**全部确认完毕，可进入阶段二。**

---

*提案版本: v1.0*
*日期: 2026-05-22*
