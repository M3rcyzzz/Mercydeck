---
name: mercydeck
description: 按用户(mercy/zhanghuize)的既定风格与标准制作 HTML 演讲 deck——Tier0 工业风(lime 强调、eyebrow 标签、IBM Plex/系统字体)、16:9 纯 CSS 缩放舞台、键盘/点击/触摸翻页、视频自动播停,自包含文件夹可直接上传 m3rcyzzz.club。只要用户要做/改"deck、演讲、presentation、slides、汇报页、路演材料",或提到"做个 deck 讲 X""给客户演示 Y",一律先用本 skill,即使没点名 mercydeck。也适用于把 PPT/文档内容转成 HTML deck。
---

# mercydeck — 做一份 mercy 标准的 HTML deck

用户有一套自建 deck 托管管线(m3rcyzzz.club),已有多份成熟 deck。本 skill 把它们的共同风格与工程标准固化下来:**你产出的 deck 应该看起来像同一个人做的第 N 份,而不是新发明**。

## 工作流

1. **先问清三件事**(用户没说的话):主题与受众、页数量级(短 5–8 页 / 标准 12–20 页)、有哪些现成素材(截图/视频/数据)。有素材先看素材。
2. **复制模板起步**:把 `assets/template.html` 复制为工作目录里 `<deck-name>/index.html`,在其骨架上增删页,**不要从零手写** HTML 结构——模板里的舞台缩放、导航脚本、HUD、token 是经过真机验证的,重写只会引入 bug。
3. **逐页填内容**,页型从模板的 6 个基础型 + `references/slide-patterns.md` 的扩展配方里选。写文案遵循下面的"内容风格"。
4. **需要更多页型/自定义主题色** → 读 `references/slide-patterns.md`。
5. **收尾前必读 `references/publishing.md`**,按其自检清单逐条过一遍(文件名小写、无外链、视频规范、手机可读)。
6. 有浏览器工具时,打开成品截 2–3 页验证观感(封面、一个内容页、视频页);没有就提醒用户本地打开检查。

## 风格要义(这套 deck 的辨识度所在)

**布局**
- 16:9 舞台,纯 CSS 适配(`100vw/56.25vw` + max 上限),排版一律用 **cqw/cqh 容器单位**,禁止 px 字号——这保证投影仪/笔记本/手机同构。
- 每页结构:`eyebrow`(mono、英文大写 `大类 · 细分`)→ `accent-line`(lime 短线)→ `page-title` → 正文。封面/章节/收尾用 `.dark` 深底变体。
- 一页一件事:≤5 个要点,或 1 个图表/视频,或 3 个大数字。内容多就拆页,deck 页数不值钱。
- **版式容量**:大数字 / 多卡片用横向 `.row` 并排(模板 slide 4 的 stat wall),**别把大号数字卡纵向堆叠**——三张就顶破 16:9 舞台。模板内置 auto-fit 保险丝:真超高会把整页等比缩小、绝不裁切,但缩放是兜底不是排版手段;一旦触发就说明这页该拆或该换更密的版式。
- **`.center` vs `.body`**:`.center`(整页居中)只给**没有 header 的深底页**(封面 / 章节 / 收尾);任何带 eyebrow + 标题的内容页,正文一律包进 `.body`——用反了会让正文压到标题上(这是最常见的翻车)。

**色彩**(Tier0 token,模板 `:root` 已定义)
- 纸白底 + 墨黑字(`#050B14`),**lime `#B2ED1D` 只做点缀**(短线、marker、深底上的高亮字),浅底上的强调文字用深绿 `#73B200`。大面积色块用 `--green-tint`。
- deck 有自有品牌时只换 3 个强调 token(见 slide-patterns.md 末节),中性色不动。

**文案**
- 标题句式:`名词 · 一句话论断`(如 "Henkel · 两周上线定制 WMS");正文短句,数字用 `.hl`/`.big` 放大;中文正文 + 英文 eyebrow 混排是常态。
- 无 emoji,无营销腔;工业/工程口吻,让数据说话。

**媒体**
- 演示视频:无声循环录屏,`muted loop playsinline preload="metadata" controls`,进页自动播(模板脚本已管)。
- 图片截图统一走 `figure.media`(带边框圆角+图注),不裸贴。

## 硬性工程约束(违反=deck 在用户管线上出问题)

- 单文件 `index.html` 内联全部 CSS/JS;媒体在 `videos/`、`assets/` 子目录,**小写 ASCII 文件名,相对路径**。
- **零外部依赖**(无 Google Fonts/CDN/外链图)——观众多在中国大陆,外链会卡死;离线缓存也存不了外部资源。
- 文字必须是 DOM 静态文本(用户的管理端有双击编辑器,JS 生成的文字改不了)。
- 保留 `</body>`(管线注入点)。视频 ≤1080p H.264(编码命令见 publishing.md)。

## 交付

产出 `<deck-name>/` 文件夹(index.html + 媒体子目录)。告诉用户:到 `m3rcyzzz.club/app` 上传该文件夹,URL 路径建议短英文 slug;上传后手机/投影直接打开对应链接即可放映(←/→、空格、点击、滑动翻页)。
