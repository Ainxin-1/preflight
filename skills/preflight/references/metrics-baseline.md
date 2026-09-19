# 基线测量口径

只有本轮实测到的数字才算基线。估算值一律写「未测量」，不写「影响不大」「开销很小」。

## 先推导，再查表

下面所有表都是**示例，不是清单**。先用 SKILL.md 第 3 步的判据回答「这个功能做错会在本项目里以什么形式变坏」，再来抄可复用的命令；表里没有的栈自己写命令并实测。不相关的指标不要硬套。

## 通用四项（跨栈等价）

| 指标 | 怎么测 | 口径注意 |
|---|---|---|
| 产物体积 | 构建后量最终产物：可执行文件 / 安装包 / `dist` 目录 / 镜像 | 必须同 profile，debug 与 release 不可混比 |
| 依赖数量 | 各生态的树命令：`cargo tree`、`npm ls --all`、`pip tree`、`go list -m all` | 报**总数**和**新增数**两个值；只报增量会掩盖基数 |
| 启动耗时 | 首屏渲染完成 / 服务可接受请求 / 窗口出现，取第 3 次之后的中位数 | 首启含杀软与冷缓存；固定同一机器同一状态 |
| 新增概念数 | 人工判定：新名字、新配置项、新状态、新抽象 | 需要解释才能改对的抽象都算一个——这一项最贴近「臃肿」 |

前两项几乎零成本，**任何情况下都没有理由标未测量**。后两项测不了要写明为什么测不了。

## 示例口径 A：桌面端（Rust / Tauri）

| 指标 | 命令 | 注意 |
|---|---|---|
| 安装包体积 | `du -sh src-tauri/target/release/bundle/nsis/*.exe` | NSIS 压缩率会掩盖依赖增量，同时看未压缩产物 |
| 常驻内存 | `Get-Process <name> \| Select PrivateMemorySize64`（PowerShell）；类 Unix 用 `ps -o rss=` | 以 PrivateMemorySize 为准，WorkingSet 会被系统回收干扰 |
| feature 面 | `cargo tree -e features` | 传递开启的 feature 是最容易被忽略的体积来源 |
| 界面开关数 | 数设置页控件数（`grep -c` 对应组件标签） | 用户可见复杂度，比代码行数更接近体感 |

## 示例口径 B：Web 前端

| 指标 | 命令 | 注意 |
|---|---|---|
| 打包体积 | 构建工具的 bundle 分析（如 `rollup-plugin-visualizer`、`webpack-bundle-analyzer`） | 报 gzip 后与首屏路由两条数，只看总量会漏 |
| 运行时依赖数 | `npm ls --all --depth=1` | 区分 devDependencies 与进包的 dependencies |
| 首屏延迟 | Lighthouse / Performance API 取 LCP 中位数 | 别用一次冷跑的极值 |
| 全局状态数 | 数新增的 store/context 实例 | 每多一个全局状态就多一处真相来源 |

## 换环境时先探测

命令可用性因机器而异，用前先测：`command -v <cmd>` 或 `where <cmd>`。**shell 无外网的环境里**，`cargo add`、`git clone`、`curl` 都会失败——依赖信息改用可用的外部检索工具查注册表页，别拿安装命令试；连检索能力都没有时，把相关结论标成「未外部验证」。
