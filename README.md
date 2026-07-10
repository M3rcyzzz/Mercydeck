# mercydeck

一个 [Claude Code / Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)，用来按既定风格与标准生成自包含的 HTML 演讲 deck。

产出的 deck 是**一个文件夹**（`index.html` + `assets/` + `videos/`），零外部依赖，可直接浏览器打开，或上传到自建的 deck 托管站放映。

## 风格来源

风格只参照两份真实 deck，不掺别的：

| 参照 | 它定义了什么 |
|---|---|
| **henkel总体** | Tier0 品牌色板、eyebrow 标签、大数字 + 数据出处、案例页 |
| **AWS-TIER0** | 留白与克制、论断页、`.roomy`、字号编码量级、分步揭示 |

纸白内容页 + 深底封面/收尾；强调色只给少数关键词与大数字（浅底 `#73B200`，深底 `#B2ED1D`）。内容页以 `eyebrow`（mono、`大类 · 细分`）+ 标题开头；排版全部用 cqw/cqh 容器单位，跑在纯 CSS 的 16:9 舞台上。**留白是主角** —— 宁可拆页，不要填满。

键盘（←/→/空格）、点击分区、手机滑动均可翻页；视频进页自动播、翻走暂停；`.frag` 元素按 → 依次浮现。

## 结构

```
mercydeck/
├── SKILL.md                     # 入口：工作流、风格要义、硬性工程约束
├── assets/template.html         # 起手模板（6 个基础页型 + 导航脚本 + auto-fit）
├── references/
│   ├── slide-patterns.md        # 扩展页型配方（stat wall / 条形对比 / 结论链 / 案例 …）
│   └── publishing.md            # 发布规范 + 交付自检清单
└── evals/evals.json             # 评测用例
```

## 用法

把 `mercydeck/` 放进 `~/.claude/skills/`，然后正常提要求即可：

```
帮我做一个 deck，讲 XXX，给 YYY 看，中文，8 页左右
```

Skill 会自动触发：复制模板 → 逐页填内容 → 按 `publishing.md` 的清单自检。

## 三条硬版式规则

真实评测里栽过的坑，模板与文档都已针对性加固：

1. **`.row` 是 `.body` 的子元素**，绝不写成 `class="body row"` —— 那样 `.body` 的垂直居中会跑到横轴上，版面必歪。
2. **`.center` 只给无 header 的深底页**（封面/章节/收尾）。带 eyebrow + 标题的内容页，正文一律包进 `.body`；用反了正文会盖到标题上。
3. **大数字横向并排**（`.row.base` 共享底部基线，`.big.w1/.w2/.w3` 用字号编码量级）。纵向堆三张大数字卡会顶破 16:9 舞台。

模板脚本内置 **auto-fit**：内容真超高时整页等比缩小、绝不裁切 —— 但那是保险丝，触发就说明该拆页。

## License

私有仓库，仅供个人使用。
