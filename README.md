# mercydeck

一个 [Claude Code / Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)，用来按既定风格与标准生成自包含的 HTML 演讲 deck。

产出的 deck 是**一个文件夹**（`index.html` + `assets/` + `videos/`），零外部依赖，可直接浏览器打开，或上传到自建的 deck 托管站放映。

## 风格

Tier0 工业风：纸白内容页 + 深底封面/收尾，lime `#B2ED1D` 点缀，每个内容页 `eyebrow`（mono、`大类 · 细分`）+ accent-line + 标题；排版全部用 cqw/cqh 容器单位，跑在纯 CSS 的 16:9 舞台上。键盘（←/→/空格）、点击分区、手机滑动均可翻页；视频进页自动播、翻走暂停。

## 结构

```
mercydeck/
├── SKILL.md                     # 入口：工作流、风格要义、硬性工程约束
├── assets/template.html         # 起手模板（6 个基础页型 + 导航脚本 + auto-fit）
├── references/
│   ├── slide-patterns.md        # 扩展页型配方（stat wall / 时间线 / 对比 / 案例 …）
│   └── publishing.md            # 发布规范 + 交付自检清单
└── evals/evals.json             # 评测用例
```

## 用法

把 `mercydeck/` 放进 `~/.claude/skills/`，然后正常提要求即可：

```
帮我做一个 deck，讲 XXX，给 YYY 看，中文，8 页左右
```

Skill 会自动触发：复制模板 → 逐页填内容 → 按 `publishing.md` 的清单自检。

## 两条排版铁律

真实评测里两次都栽在这上面，模板与文档都已针对性加固：

1. **大数字横向并排，不要纵向堆叠。** 三张大号数字卡纵向叠会顶破 16:9 舞台。模板脚本内置 **auto-fit**：内容真超高时整页等比缩小、绝不裁切——但那是保险丝，触发就说明该拆页。
2. **`.center` 只给无 header 的深底页**（封面/章节/收尾）。带 eyebrow + 标题的内容页，正文一律包进 `.body`；用反了正文会盖到标题上。

## License

私有仓库，仅供个人使用。
