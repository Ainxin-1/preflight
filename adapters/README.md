# 适配层

核心规则在 `../skills/preflight/`，与任何具体 agent 无关。这里只放"怎么让某个 agent 把它读起来"。

## Qoder（已验证）

```
~/.qoder/skills/preflight/            用户级，跨项目生效
<项目>/.qoder/skills/preflight/       项目级，随仓库走、可提交、团队共用
```

放进去后**无需重启**就会出现在可用技能列表；手动调用 `/preflight <一句话需求>`，或按 description 自动召回。卸载 = 删掉那个目录。

仓库根目录的 `.qoder-plugin/plugin.json` 是 Qoder 的插件清单（字段：`name` / `displayName` / `version` / `description`）。它留在根是因为官方插件就是这个布局；**移动到其他位置是否仍能加载，未验证**，所以别挪。

## 其他 agent（未验证，用前自己确认）

通用方法：找到该 agent 实际扫描的技能目录，把 `skills/preflight/` 整目录放进去，然后确认它列出了这个技能——**列不出来就是没生效，不是路径写对了就完事**。

各家的具体目录我没有一手证据，因此不在此列出。已知会踩的坑值得写下来：不少第三方 skill 仓库的安装脚本把文件复制到 `~/.agents/skills`、`~/.config/opencode/skills`、`~/.claude/skills` 这类路径，而某些 agent **根本不读这些目录**，于是"装成功了但永远不触发"。所以：先查你这套工具实际读哪里，再谈安装。

欢迎提 issue 补已验证的路径，附上 agent 名称、版本、以及"技能列表里能看到它"的证据。

## 移植时唯一不能动的东西

`SKILL.md` 里输出模板的**顺序**、硬规则、以及"测不了就写未测量 / 没查就写未验证"这两条诚实约束。把它们改没了，这个 skill 就退化成一份搜索清单。
