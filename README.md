# Mercydeck

制作自包含 HTML 演讲 deck，默认参考 [factory-software](https://m3rcyzzz.club/factory-software/)：白底、墨黑、少量绿、常规字重的大字和留白，以分步图解和演示视频推进内容。旧版默认的 eyebrow、暗底封面、渐变强调和 cqw 排版已由新的 1600×900 整体缩放模板替代。

[在线说明与模板预览](https://m3rcyzzz.club/mercydeck/) · [下载完整 skill](https://github.com/M3rcyzzz/Mercydeck/archive/refs/heads/main.zip)

## 安装

选择所用工具的技能目录，将仓库克隆为 `mercydeck`：

```bash
# Codex
git clone https://github.com/M3rcyzzz/Mercydeck.git ~/.codex/skills/mercydeck

# Claude Code
git clone https://github.com/M3rcyzzz/Mercydeck.git ~/.claude/skills/mercydeck
```

也可以下载 ZIP，将解压后的技能目录命名为 `mercydeck`，放进相应技能目录。如果该目录已存在，先保留自己的修改，再更新。技能名称仍为 `mercydeck`。

## 使用

从 [SKILL.md](SKILL.md) 开始；[页型与视觉规则](references/slide-patterns.md) 给出排版基准，[发布规范](references/publishing.md) 说明上传、文字编辑及离线兼容要求。[起始模板](assets/template.html) 提供静态 DOM、分步状态与键盘/点击/触摸导航。模板使用系统字体回退，不包含原参考内嵌的字体或业务素材。

示例请求：`做一个中文 HTML 产品演示 deck，给工厂技术负责人看，8 页左右，参考 factory-software。`

用户指定风格和既有 deck 优先；制作不自动授权上线。输出为根目录 `index.html` 加本地媒体的文件夹。评测场景在 [evals/evals.json](evals/evals.json)。
