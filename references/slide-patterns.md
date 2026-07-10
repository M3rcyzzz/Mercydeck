# mercydeck 页型配方

模板 `assets/template.html` 已含 6 个基础页型。本文件是扩展配方——都来自真实 deck(tier0-uns / tier0-dx / ia2-cn)里被反复使用的模式。所有尺寸用 cqw/cqh(容器单位),不要用 px 定字号。

**两条排版铁律**(违反必翻车,真实评测里两次都栽在这):
1. **大数字并排,不要纵向堆叠**。三个大号数字/卡片一律横向 `.row`(见下 [三个大数字](#三个大数字-stat-wall));纵向叠三张大数字卡会顶破 16:9 舞台。模板脚本内置 auto-fit:真超高会把整页等比缩小、不裁切——但那是保险丝,触发说明该拆页,别当排版手段用。
2. **`.center` 只给无 header 的深底页**(封面/章节/收尾)。带 eyebrow+标题的内容页,正文包进 `.body`(在剩余空间居中);误用 `.center` 会让正文盖到标题上。

## 目录
- [Eyebrow 写法](#eyebrow-写法)
- [三个大数字(stat wall)](#三个大数字-stat-wall)
- [章节分隔页](#章节分隔页)
- [时间线](#时间线)
- [流程图(纯 CSS)](#流程图纯-css)
- [对比页(旧 vs 新)](#对比页旧-vs-新)
- [案例页](#案例页)
- [多步渐进页](#多步渐进页)
- [全幅大图页](#全幅大图页)
- [引用页](#引用页)
- [自定义强调色](#自定义强调色)

## Eyebrow 写法

真实 deck 的惯例:`大类 · 细分`,mono 字体,英文大写(哪怕正文是中文):

```
CHALLENGE            DATA INTEGRATION      DEMO · UNS Agent
TIMELINE             DEPLOYMENT            SMART FACTORY · ELECTRONICS
RESULTS              APPLICATION SCENARIOS PROCESS SAFETY · API 754
```

每个内容页都以 eyebrow + accent-line + page-title 开头——这是这套风格最强的识别特征,别省略。

## 三个大数字(stat wall)

量化效果页的标配:三个大数字**横向并排**(模板 slide 4 就是此型)。数字用 `.big`,下面跟一句说明。**关键:用 `.row` 横排,一行三列**——这样在 16:9 里刚好放得下;若改成纵向一个个往下堆,三个 `.big` 会超出舞台。

```html
<section class="slide">
  <div class="eyebrow">RESULTS · 量化效果</div>
  <div class="accent-line"></div>
  <h2 class="page-title">上线之后,账算得清</h2>
  <div class="body row mt2" style="align-items:flex-start">
    <div class="col"><div class="big hl">-60%</div><p class="mt muted">数据接入成本<br>免定制开发,图形化替代人月</p></div>
    <div class="col"><div class="big">15<span class="num">→2min</span></div><p class="mt muted">故障响应时间<br>边缘就地告警</p></div>
    <div class="col"><div class="big hl">+8%</div><p class="mt muted">OEE 设备综合效率</p></div>
  </div>
</section>
```

数字多于三个,或每个还带 2–3 条支撑要点(会很高)→ 拆成两页,或改用纵向卡片但**每张只留一行说明**;别硬塞。

## 章节分隔页

深底、超大编号、只有一句话:

```html
<section class="slide dark">
  <div class="center">
    <p class="mono muted" style="font-size:2cqw;letter-spacing:.3em">PART 02</p>
    <h1 class="mt">章节标题</h1>
    <div class="accent-line mt"></div>
  </div>
</section>
```

## 时间线

```html
<div class="row mt2" style="align-items:flex-start;gap:2cqw">
  <!-- 每个节点一个 col;当前/关键节点加 .key -->
  <div class="col card"><p class="mono small">2018</p><h3 class="mt">节点标题</h3><p class="muted">一句说明</p></div>
  <div class="col card key"><p class="mono small">2024</p><h3 class="mt">当前</h3><p class="muted">一句说明</p></div>
</div>
```

## 流程图(纯 CSS)

来自 ia2-cn 的 `.flow` 模式——盒子 + 箭头,重点盒子高亮。不要为流程图引入图片:

```html
<div class="row mt" style="gap:1.4cqw;align-items:stretch">
  <div class="col card" style="display:flex;align-items:center;justify-content:center;text-align:center"><h3>输入</h3></div>
  <div style="flex:0 0 3.5cqw;display:flex;align-items:center;justify-content:center;font-size:3.6cqw;color:#cbcbcb">→</div>
  <div class="col card key" style="display:flex;align-items:center;justify-content:center;text-align:center"><h3>核心步骤</h3></div>
  <div style="flex:0 0 3.5cqw;display:flex;align-items:center;justify-content:center;font-size:3.6cqw;color:#cbcbcb">→</div>
  <div class="col card" style="display:flex;align-items:center;justify-content:center;text-align:center"><h3>输出</h3></div>
</div>
```

## 对比页(旧 vs 新)

左右两卡,旧的灰、新的 key 高亮;标题用 ✗/✓ 或 Before/After:

```html
<div class="body row mt">
  <div class="col card"><h3 class="muted">传统方式</h3><ul class="mt"><li class="muted">痛点…</li></ul></div>
  <div class="col card key"><h3 class="hl">mercy 方式</h3><ul class="mt"><li>优势…</li></ul></div>
</div>
```

## 案例页

真实惯例(tier0-uns 的 Henkel/Aramco/SMT 页):eyebrow 写行业+场景,标题写 `客户 · 一句话成果`,正文左窄右宽——左边 3 条要点,右边截图/视频:

```html
<section class="slide">
  <div class="eyebrow">WAREHOUSE MANAGEMENT · CUSTOM WMS</div>
  <div class="accent-line"></div>
  <h2 class="page-title">Henkel · 两周上线定制 WMS</h2>
  <div class="body row mt">
    <div class="col" style="flex:0 0 30cqw">
      <ul><li>背景一句</li><li>做了什么</li><li><span class="hl">量化结果</span></li></ul>
    </div>
    <figure class="media col"><img src="assets/henkel-dashboard.png" alt=""><figcaption>图注</figcaption></figure>
  </div>
</section>
```

## 多步渐进页

模板导航脚本原生支持:`data-step="0" data-steps="N"`,右键/右击先走步骤再翻页。CSS 按步骤显隐:

```html
<section class="slide" data-step="0" data-steps="2">
  <style>
    /* scoped:第 N 步出现什么 */
    [data-step="0"] .st1, [data-step="0"] .st2 { opacity:.15 }
    [data-step="1"] .st2 { opacity:.15 }
    [data-step="1"] .st1, [data-step="2"] .st1, [data-step="2"] .st2 { opacity:1; transition:opacity .3s }
  </style>
  <div class="eyebrow">ARCHITECTURE</div>
  <h2 class="page-title">分层揭示</h2>
  <div class="body">
    <div class="card">基础层(一直可见)</div>
    <div class="card st1 mt">第 1 步出现</div>
    <div class="card st2 mt">第 2 步出现</div>
  </div>
</section>
```

## 全幅大图页

图占满、文字压在上面(深色渐变保证可读):

```html
<section class="slide" style="padding:0">
  <img src="assets/hero.jpg" alt="" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover">
  <div style="position:absolute;inset:0;background:linear-gradient(180deg,transparent 40%,rgba(5,11,20,.82))"></div>
  <div style="position:absolute;left:8cqw;bottom:9cqh;color:#fff;max-width:70cqw">
    <h2 style="font-size:4.6cqw">压在图上的标题</h2>
    <p class="mt" style="color:#c9cdd3">一句说明。</p>
  </div>
</section>
```

## 引用页

```html
<section class="slide">
  <div class="center" style="max-width:78cqw">
    <p style="font-size:3.6cqw;line-height:1.5;font-weight:500">"引用原话,别超过两行。"</p>
    <p class="small mt2 mono">— 姓名 · 头衔 · 公司</p>
  </div>
</section>
```

## 自定义强调色

默认 Tier0 lime 系。如果 deck 有自己的主题色(如 ia2-cn 用蓝紫渐变),只改 `:root` 的 `--lime/--deep-green/--green-tint`,或加渐变文字类;**其它 token 不动**,保证同族观感:

```css
:root { --lime:#00B3FF; --deep-green:#0057E3; --green-tint:#EAF3FF; }
/* 渐变高亮字(ia2 风格,只用于个别重点词) */
.hlg { background:linear-gradient(100deg,#00B3FF,#0057E3 40%,#5A2BE0 75%);
       -webkit-background-clip:text; background-clip:text; color:transparent; font-weight:700 }
```
