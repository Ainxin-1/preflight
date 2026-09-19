# 检索打法

## 种子词

种子词 = 目标的**核心动词 + 名词**，不是整句需求。`merge pdf`、`image compress`、`global hotkey`、`tray icon`。窄词找方案，宽词找生态。

第二轮必须换词，别用同义词重复第一轮：换成实现侧术语（`pdf join`、`page append`）、相邻领域术语（`pdfium`、`mupdf`）、或语言生态前缀（`rust pdf`、`web worker upload`）。

## 通道优先级

0. **`gh`（若可用）** — 先探测：`command -v gh` / `where gh`。可用则 `gh search repos "<seed>" --limit 15 --sort stars`，字段最全。
1. **网络检索工具（WebSearch 等）** — 广度。`<seed> <生态> site:github.com`、`<seed> alternatives`、`<seed> <语言/平台> library vs`。
2. **GitHub 搜索 API** — `https://api.github.com/search/repositories?q=<seed>&sort=stars&order=desc&per_page=10`，取 `stargazers_count`、`pushed_at`、`license`、`archived`。无需 token，返回 403 就是限流，改回通道 1，别重试轰炸。
3. **对应语言的包注册表页**（crates.io / npm / PyPI / pkg.go.dev / Maven Central）— 决定「加不加这个依赖」时比 GitHub 更权威：下载量曲线、版本节奏、feature 或传递依赖面。没有注册表的领域（系统组件、原生库）回到 GitHub + 官方文档。
4. **社区**（挖坑，不挖实现）：`site:reddit.com <seed> pitfalls OR "we switched"`、`site:news.ycombinator.com <seed>`、`site:lobste.rs`、v2ex。非母语社区结论只作线索，须回官方文档核对。

工具名按你所在 agent 环境替换；关键是**别把检索押在单一 CLI 上**——那台机器没装它就整条流程卡死。

## 排序偏差

只按 star 排会把「最热门」当成「最正确」，并系统性漏掉小众正解。补一轮 `sort=updated`，或搜「`<seed> minimal`」「`<seed> no dependencies`」专门找小而准的方案。体积敏感的场景里，star 高的全家桶常常是错的。

## 可信候选的最低标准

进结论的候选至少要有：可点开的仓库 URL、最近一次提交时间、许可证、以及它覆盖需求的哪几条。缺任一项就标「未核实」，不许用它支撑决策。

## 停手

满足任一即停（轮次上限见 SKILL.md 可调参数）：

- 已有足够可信候选（默认 2 个）且分档能拉开差距
- 连续 2 轮零有效结果 —— 这是空白区，本身就是有价值的结论，直接进自研分支
- 到达轮次上限 —— 把中间结论交给人，比继续搜索更有价值

## 红线

- 不把搜索引擎摘要当作已验证事实；引用的数字（星数、下载量、版本号）必须来自实际抓到的页面。
- 不引入破解、注册机、逆向绕过类方案，即使来源是技术论坛。
- 论坛片段不照抄进代码；只把「坑」转写成一条能复现的测试或一句解释为什么的注释。
- 引用外部内容注明来源，不复制成自己的产出。
