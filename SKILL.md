---
name: mercydeck
description: 按用户(mercy/zhanghuize)的既定风格与标准制作 HTML 演讲 deck——Tier0 工业风(绿色强调、eyebrow 标签、系统字体)、16:9 纯 CSS 舞台、大量留白、键盘/点击/触摸翻页、视频自动播停,自包含文件夹可直接上传 m3rcyzzz.club。只要用户要做/改"deck、演讲、presentation、slides、汇报页、路演材料",或提到"做个 deck 讲 X""给客户演示 Y",一律先用本 skill,即使没点名 mercydeck。也适用于把 PPT/文档内容转成 HTML deck。
---

# mercydeck — 做一份 mercy 标准的 HTML deck

这套风格只有两份参照 deck,别的都不算数:

| 参照 | 位置 | 它定义了什么 |
|---|---|---|
| **henkel总体** | `decks/tier0-uns/` | Tier0 品牌色板、eyebrow 标签、大数字 + 数据出处、案例页 |
| **AWS-TIER0** | `decks/tier0-dx/` | 留白与克制、论断页、`.roomy`、字号编码量级、分步揭示 |

**你产出的 deck 应该像同一个人做的第 N 份,而不是新发明。**

## 工作流

1. **先问清三件事**(用户没说的话):主题与受众、页数量级(短 5–8 页 / 标准 12–20 页)、有哪些现成素材(截图/视频/数据)。有素材先看素材。
2. **复制模板起步**:把 `assets/template.html` 复制为 `<deck-name>/index.html`,在其骨架上增删页,**不要从零手写** HTML 结构——舞台缩放、导航脚本、auto-fit 都是真机验证过的,重写只会引入 bug。
3. **逐页填内容**,页型从模板的 6 个基础型 + `references/slide-patterns.md` 的扩展配方里选。
4. **需要更多页型 / 换主题色** → 读 `references/slide-patterns.md`。
5. **收尾前必读 `references/publishing.md`**,按自检清单逐条过。
6. 有浏览器工具就打开成品截几页验证观感(封面、一个内容页、数字页);没有就提醒用户本地打开检查。

## 风格要义

**留白是主角。** 这是最容易做丢的一条。AWS-TIER0 里大量页面只有一句大标题 + 一两行解释,剩下全是白。宁可拆成三页,也不要把一页填满。一页一件事:≤5 个要点,或 1 个图表/视频,或 3 个大数字。

**每页结构**
- 内容页:`eyebrow`(mono、`大类 · 细分`)→ `page-title` → `.body` 包正文。
- `accent-line`(lime 短横)是**可选**的——参照 deck 里定义了却几乎不用。想要更硬的分节感再加,别当成必需品。
- 封面 / 分章 / 收尾:`.slide.dark` 深底,内容用 `.center`(整页居中)。

**三条硬版式规则**(违反必翻车,真实评测里栽过)
1. `.row` 是 `.body` 的**子元素**,绝不写成 `class="body row"`——那样 `.body` 的 `justify-content:center` 会跑到横轴上,版面必歪。
2. `.center` 只给**没有 header 的深底页**;带 eyebrow + 标题的内容页,正文一律包进 `.body`,否则正文会盖到标题上。
3. **大数字横向并排**(`.row.base` 共享底部基线),字号编码量级(`.big.w1/.w2/.w3`)。纵向堆叠三张大数字卡会顶破舞台。

模板脚本内置 **auto-fit** 保险丝:内容真超高时整页等比缩小、绝不裁切。但那是兜底,不是排版手段——一旦触发就说明该拆页。

**色彩**(token 在模板 `:root`)
- 纸白底 + 墨黑字(`#050B14`),强调色只给**少数关键词与大数字**,整页最多两三处。
- 默认 Tier0 绿:浅底上用 `--deep-green #73B200`,深底上用 `--lime #B2ED1D`(lime 也是进度条色)。
- AWS-TIER0 用的是 SUPCON 炫彩渐变——想换主题只改模板末尾注释里的那一段,其余 token 不动。

**文案**
- 标题句式:`名词 · 一句话论断`(如 "Henkel · 两周上线定制 WMS");或直接把结论当标题("投入与回报,不成正比")。
- 中文正文 + 英文 mono eyebrow 混排是常态;数据出处用 `.source`(mono 灰),退到背景里。
- 无 emoji,无营销腔;工业/工程口吻,让数据说话。

**媒体**
- 演示视频:无声循环录屏,`muted loop playsinline preload="metadata" controls`,进页自动播(脚本已管)。
- 截图统一走 `figure.media`(边框圆角 + mono 图注),不裸贴。

**分步揭示**:给元素加 `.frag`,按 → 依次浮现;点完才翻页,往回翻自动全亮。只动 opacity,不跳版。

## 硬性工程约束(违反=deck 在用户管线上出问题)

- 单文件 `index.html` 内联全部 CSS/JS;媒体在 `videos/`、`assets/` 子目录,**小写 ASCII 文件名,相对路径**。
- **零外部依赖**(无 Google Fonts/CDN/外链图)——观众多在中国大陆,外链会卡死;离线缓存也存不了外部资源。用模板的系统字体栈。
- 文字必须是 DOM 静态文本(用户的管理端有双击编辑器,JS 生成的文字改不了)。
- 保留 `</body>`(管线注入点)。视频 ≤1080p H.264(编码命令见 publishing.md)。

## 交付

产出 `<deck-name>/` 文件夹(index.html + 媒体子目录)。告诉用户:到 `m3rcyzzz.club/app` 上传该文件夹,URL 路径建议短英文 slug;上传后手机/投影直接打开链接即可放映(←/→、空格、点击、滑动翻页)。
