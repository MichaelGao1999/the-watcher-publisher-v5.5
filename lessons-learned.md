| | TAG:build-env TAG:testing [来源:vibe-coding-project-sop @2026-05-22] | INFO | 纯 HTML+CSS+JS 项目无需 npm，双击 `index.html` 即可预览，但涉及 Web Worker（如 Stockfish）时必须启 HTTP 服务器 [来源:blindfold-chess @2026-05-21] | EngineModule |
| | TAG:dom TAG:api-design [来源:vibe-coding-project-sop @2026-05-22] | WARNING | 手写 IIFE 模块时，用 `window.ModuleName = Module` 暴露 API，内部私有变量用下划线前缀，避免全局泄漏 [来源:blindfold-chess @2026-05-21] | 所有 js/*.js |
| | TAG:data TAG:api-design [来源:vibe-coding-project-sop @2026-05-22] | INFO | PGN 解析器对空/无效输入返回 `[]`（空数组）而非 `null`，调用方需区分"无走法"和"解析失败" [来源:blindfold-chess @2026-05-21] | ReplayModule |
| | TAG:dom TAG:ux [来源:vibe-coding-project-sop @2026-05-22] | WARNING | 屏幕切换导航不能只隐藏上一个屏幕，必须遍历 `.screen` 全部隐藏后再显示目标，否则多层屏幕重叠 [来源:blindfold-chess @2026-05-21] | 全局导航 |
| | TAG:ai-workflow [来源:vibe-coding-project-sop @2026-05-22] | INFO | 项目文档结构会随时间进化，"存档"或"恢复"操作前应先 `ls`/`glob` 确认当前文件系统现状，避免按历史路径写入已不存在的文件 [来源:blindfold-chess @2026-05-21] | 文档维护 |
| | TAG:i18n [来源:vibe-coding-project-sop @2026-05-22] | CRITICAL | **i18n 分散架构必然导致翻译遗漏**：当项目同时存在"全局字典 + 模块私有字典 + 硬编码"三种翻译方式时，新增功能几乎必然漏掉其中一种或多种。唯一可持续的方案是"单一字典源" [来源:blindfold-chess @2026-05-21] | 全站 i18n |
| | TAG:i18n TAG:architecture [来源:vibe-coding-project-sop @2026-05-22] | WARNING | **模块内部字典若从不主动更新 DOM，则纯属冗余**：welcome.js 有 `_i18n` 和 `_t()`，但从不调用，完全依赖 common.js 的 `updateTexts()`。这种"假私有字典"不仅没用，还会给维护者造成"这里已经翻译了"的错觉 [来源:blindfold-chess @2026-05-21] | welcome.js |
| | TAG:i18n TAG:dom [来源:vibe-coding-project-sop @2026-05-22] | WARNING | **settings.js 的独立字典与 common.js 的全局扫描存在竞争**：settings panel 的元素带 `data-i18n`，settings.js 自己 `_updateAllTexts()` 会覆盖，但 common.js 的 `updateTexts()` 也会扫到，如果 common.js 缺键，用户会看到 key 名闪一下才被正确文本覆盖 [来源:blindfold-chess @2026-05-21] | settings.js / common.js |
| | TAG:testing TAG:architecture [来源:vibe-coding-project-sop @2026-05-22] | CRITICAL | **删除生产代码的 fallback 函数前，必须先评估测试环境是否提供了该依赖**：架构统一重构必须同时改代码+测试，只改一边会导致测试雪崩 [来源:blindfold-chess @2026-05-21] | 全站 i18n |
| | TAG:dom TAG:i18n [来源:vibe-coding-project-sop @2026-05-22] | WARNING | **全局 `updateTexts()` 与模块私有 `_updateXxx()` 可能存在 DOM 竞争**：两者操作同一 DOM 元素。测试必须验证最终渲染结果，而非中间状态 [来源:blindfold-chess @2026-05-21] | settings.js / common.js |
| | TAG:ux TAG:architecture [来源:vibe-coding-project-sop @2026-05-22] | WARNING | **UI 风格不一致的根因通常是「硬编码颜色」**：引入统一的「棋盘风格配置源」后，所有棋盘自动同步，消除不一致的根因 [来源:blindfold-chess @2026-05-21] | BoardRenderer / coordinate.js |
| | TAG:data TAG:architecture [来源:vibe-coding-project-sop @2026-05-22] | INFO | **数据层的双语字段与代码层的硬编码分支是两个问题**：区分"数据双语"和"代码分支"可避免过度重构 [来源:blindfold-chess @2026-05-21] | replay.js / data/games.js |
| | TAG:data TAG:build-env [来源:vibe-coding-project-sop @2026-05-22] | WARNING | **数据文件中的引号嵌套是极易被忽视的语法陷阱**：在真实浏览器中会抛出 `SyntaxError` 并阻断后续脚本执行 [来源:blindfold-chess @2026-05-21] | data/games.js |
| | TAG:testing TAG:debugging [来源:vibe-coding-project-sop @2026-05-22] | INFO | **playwright 是定位浏览器特有 bug 的有效手段**：通过 `page.add_init_script` 注入错误监听器 + `page.on('pageerror')`，可以精确定位到出错的文件、行号和列号 [来源:blindfold-chess @2026-05-21] | 调试工具 |
| | TAG:testing TAG:dom [来源:vibe-coding-project-sop @2026-05-22] | INFO | 浏览器集成测试阶段发现 welcome.js / replay.js / stats.js 的 DOM 事件绑定遗漏 [来源:blindfold-chess @2026-05-21] | |
| | TAG:cross-platform TAG:ai-workflow [来源:vibe-coding-project-sop @2026-05-22] | WARNING | **Shell here-document 在 Windows git bash 中不可靠**：含引号的多行复杂脚本会被截断或解析错误；应先 `WriteFile` 写脚本，再 `Shell` 执行 [来源:blindfold-chess @2026-05-21] | |
| | TAG:i18n TAG:ai-workflow [来源:vibe-coding-project-sop @2026-05-22] | WARNING | **翻译检查必须是独立任务，不能依赖"开发时顺手做"**：本次检查发现 25+ 处遗漏，分布在 HTML、JS 字典、硬编码三个层面 [来源:blindfold-chess @2026-05-21] | |
| | TAG:state-management [来源:vibe-coding-project-sop @2026-05-22] | CRITICAL | **绝对不要**在 `setState` 的 updater 函数内部调用 `dispatch()` 或其他 setState，会触发 React "渲染时更新" 警告 [来源:french-exit @2026-05-21] | `ResultsPage.tsx` |
| | TAG:cross-platform TAG:build-env [来源:vibe-coding-project-sop @2026-05-22] | CRITICAL | 中文路径 + MinGW = 链接器失败。解决方案：复制到纯 ASCII 路径（如 `/c/french-exit`）后编译 [来源:french-exit @2026-05-21] | |
| | TAG:testing TAG:cross-platform [来源:vibe-coding-project-sop @2026-05-22] | INFO | **`#[cfg(not(test))]` 隔离问题代码**是零副作用的修复手法：release 构建完全不受影响，测试逻辑移至独立模块继续跑 [来源:french-exit @2026-05-21] | |
| | TAG:data TAG:performance [来源:vibe-coding-project-sop @2026-05-22] | WARNING | 不要一次性加载所有完整 `TraceItem` 到前端（内存 + DOM 渲染压力大） [来源:french-exit @2026-05-21] | |
| | TAG:architecture TAG:data [来源:vibe-coding-project-sop @2026-05-22] | INFO | 正确做法：后端提供**轻量摘要接口**（只返回 id + category + suggested_action），前端用它批量生成 decisions [来源:french-exit @2026-05-21] | |
| | TAG:pagination TAG:architecture [来源:vibe-coding-project-sop @2026-05-22] | WARNING | 用户实际浏览仍按分页，但"全选全部"走轻量接口，两者解耦 [来源:french-exit @2026-05-21] | |
| | TAG:pagination TAG:state-management TAG:security [来源:vibe-coding-project-sop @2026-05-22] | CRITICAL | **事故经过**：ResultsPage 默认自动勾选所有扫描结果 → 用户点击"全选全部"（以为是全选当前页，实际是全选全部）→ 确认页看到"将删除 17,706 个文件"但未警觉 → 执行后大量文件丢失 [来源:french-exit @2026-05-21] | |
| | TAG:security TAG:ux [来源:vibe-coding-project-sop @2026-05-22] | CRITICAL | **教训**：涉及删除的安全工具，**默认安全 > 默认便利**。所有选择必须用户显式操作，任何"帮你选好"的设计都需反复审视 [来源:french-exit @2026-05-21] | |
| | 测试驱动开发能在手工测试无法触及的边界条件下发现 bug（如"恰好取消所有勾选"触发死循环）[来源:french-exit @2026-05-21] [来源:vibe-coding-project-sop @2026-05-22] |  |
| | `AGENTS.md` 定义触发词和行为约束，`status.md` 记录动态进度，两者分工明确，新会话读 2 份文件即可开工 [来源:french-exit @2026-05-21] [来源:vibe-coding-project-sop @2026-05-22] |  |
| | 涉及 7+ 文件读改测的架构重构，应新开会话执行，避免上下文压缩导致信息丢失 [来源:blindfold-chess @2026-05-21] [来源:vibe-coding-project-sop @2026-05-22] |  |
| | 中文路径 + MinGW = 链接器失败。解决方案：复制到纯 ASCII 路径后编译 [来源:french-exit @2026-05-21] [来源:vibe-coding-project-sop @2026-05-22] |  |
| | `cargo check --lib` 不需要链接，可以在中文路径直接跑；`cargo test --no-run` 同理 [来源:french-exit @2026-05-21] [来源:vibe-coding-project-sop @2026-05-22] |  |
| | Windows 路径在 git bash / Node.js / cmd 中转义规则不同，写跨平台脚本时优先用正斜杠或 `path.join` [来源:blindfold-chess @2026-05-21] [来源:vibe-coding-project-sop @2026-05-22] |  |
