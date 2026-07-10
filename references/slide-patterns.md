# mercydeck 页型配方

模板 `assets/template.html` 已含 6 个基础页型。本文件是扩展配方,**全部来自两份参照 deck**:

- **「henkel总体」**(`decks/tier0-uns/`)—— eyebrow、大数字 + 数据出处、案例页、合作方 logo 墙
- **「AWS-TIER0」**(`decks/tier0-dx/`)—— 留白、论断页、`.roomy`、条形对比、结论链、分步揭示

尺寸一律 cqw/cqh(容器单位),不要用 px 定字号。带 CSS 的配方,把 `<style>` 里的规则并进模板的 `<style>` 即可。

## 三条硬版式规则(先记住,再往下抄)

1. `.row` 是 `.body` 的**子元素**,不要写 `class="body row"`(会把 `.body` 的垂直居中变成横向居中,版面必歪)。
2. `.center` 只给**无 header 的深底页**(封面/分章/收尾);有 eyebrow + 标题的内容页,正文包进 `.body`。
3. 大数字**横向并排**(`.row.base`),纵向堆三张大数字卡会顶破 16:9 舞台。auto-fit 会兜底缩放不裁切,但触发 = 该拆页。

## 目录
- [Eyebrow 写法](#eyebrow-写法)
- [论断页](#论断页)
- [章节分隔页](#章节分隔页)
- [三个大数字(stat wall)](#三个大数字stat-wall)
- [条形对比页](#条形对比页)
- [结论流程链](#结论流程链)
- [对比页(旧 vs 新)](#对比页旧-vs-新)
- [案例页](#案例页)
- [分步揭示](#分步揭示)
- [引用页](#引用页)
- [全幅大图页](#全幅大图页)
- [换主题色](#换主题色)

## Eyebrow 写法

henkel总体 的惯例:`大类 · 细分`,mono,英文大写(哪怕正文是中文):

```
CHALLENGE            DATA INTEGRATION      DEMO · UNS Agent
RESULTS · 量化效果    DEPLOYMENT            MARKET LEADER · Domestic Standing
```

内容页以 eyebrow + page-title 开头。`accent-line` 是可选的 lime 短横——参照 deck 里定义了却几乎不用,别每页都加。

## 论断页

AWS-TIER0 的招牌:**把结论当标题,独占一页**,底下最多一两行。大量留白就是设计。

```html
<section class="slide roomy">
  <div class="center">
    <h2 class="page-title">投入与回报,<span class="hl">不成正比</span>。</h2>
    <p class="lead mt">"云、大、智、物、移"俱全的技术,最后做出来的,是一块看板。</p>
  </div>
</section>
```

## 章节分隔页

深底、只有一句话:

```html
<section class="slide dark">
  <div class="center mid">
    <p class="source" style="letter-spacing:.3em">PART 02</p>
    <h1 class="mt">章节标题</h1>
  </div>
</section>
```

## 三个大数字(stat wall)

量化效果页的标配。**横向并排,共享底部基线**(`.row.base`),字号本身编码量级——最重要的那个用 `.w3`。数据出处用 `.source` 挂在下面。

```html
<section class="slide">
  <div class="eyebrow">RESULTS · 量化效果</div>
  <h2 class="page-title">上线之后,账算得清</h2>
  <div class="body">
    <div class="row base">
      <div class="col"><div class="big w1 hl">-60%</div><p class="mt muted">数据接入成本</p></div>
      <div class="col"><div class="big w2">15<span class="num">→2min</span></div><p class="mt muted">故障响应时间</p></div>
      <div class="col"><div class="big w3 hl">+8<span style="font-size:.5em">%</span></div><p class="mt muted">OEE 设备综合效率</p></div>
    </div>
    <p class="source mt2">DATA SOURCE · 项目复盘 2026-06</p>
  </div>
</section>
```

数字多于三个,或每个还要挂 2–3 条支撑要点 → 拆成两页。别硬塞。

## 条形对比页

AWS-TIER0 的 `.barrow`:轴标题降为小号灰色 overline,不与数据抢戏;强调项用主色填充。

```html
<style>
  .axis { font-size:1.9cqw; font-weight:700; letter-spacing:.15em; color:var(--ink-3); margin-bottom:.6cqh }
  .barrow { display:flex; align-items:center; gap:2cqw; margin-top:1.8cqh }
  .barlabel { flex:0 0 11cqw; font-size:2.1cqw; color:var(--ink-2) }
  .barlabel strong { color:var(--ink) }
  .bartrack { flex:1; background:#F0F0F0; height:3.8cqh }
  .barfill { background:#B9B9B9; height:100%; width:var(--w) }
  .barfill.on { background:var(--accent) }
  .barval { flex:0 0 14cqw; font-size:2.1cqw; font-weight:700; font-variant-numeric:tabular-nums }
</style>
<div class="body">
  <p class="axis">交付周期</p>
  <div class="barrow"><div class="barlabel">传统方式</div>
    <div class="bartrack"><div class="barfill" style="--w:100%"></div></div><div class="barval">12 周</div></div>
  <div class="barrow"><div class="barlabel"><strong>Tier0</strong></div>
    <div class="bartrack"><div class="barfill on" style="--w:17%"></div></div><div class="barval hl">2 周</div></div>
</div>
```

## 结论流程链

AWS-TIER0 末页的 `.chain`:主词大、垂落小注、运算符退后。不要为它引入图片。

```html
<style>
  .chain { display:flex; align-items:flex-start; justify-content:center; gap:2.5cqw; margin-top:6cqh }
  .chainitem { display:flex; flex-direction:column; align-items:center; text-align:center }
  .chainmain { font-size:2.9cqw; font-weight:700; line-height:1.3 }
  .chainsub { font-size:1.7cqw; color:var(--ink-3); margin-top:.8cqh }
  .chainop { font-size:2.6cqw; color:#C5C5C5; line-height:1.45 }
</style>
<div class="chain">
  <div class="chainitem"><div class="chainmain">Agent</div><div class="chainsub">会用工具的模型</div></div>
  <div class="chainop">×</div>
  <div class="chainitem"><div class="chainmain">Skill</div><div class="chainsub">把手艺写进提示</div></div>
  <div class="chainop">→</div>
  <div class="chainitem"><div class="chainmain hl">TIER0</div><div class="chainsub">现场可交付</div></div>
</div>
```

## 对比页(旧 vs 新)

左右两卡,旧的灰、新的 `key` 高亮:

```html
<div class="body">
  <div class="row">
    <div class="col card"><h3 class="muted">传统方式</h3><ul class="mt"><li class="muted">痛点…</li></ul></div>
    <div class="col card key"><h3 class="hl">mercy 方式</h3><ul class="mt"><li>优势…</li></ul></div>
  </div>
</div>
```

## 案例页

henkel总体 的惯例(Henkel / Aramco / SMT 页):eyebrow 写行业 + 场景,标题写 `客户 · 一句话成果`,左窄右宽——左边 3 条要点,右边截图。

```html
<section class="slide">
  <div class="eyebrow">WAREHOUSE MANAGEMENT · CUSTOM WMS</div>
  <h2 class="page-title">Henkel · 两周上线定制 WMS</h2>
  <div class="body">
    <div class="row media">
      <div class="col" style="flex:0 0 30cqw">
        <ul><li>背景一句</li><li>做了什么</li><li><span class="hl">量化结果</span></li></ul>
      </div>
      <figure class="media col">
        <img src="assets/henkel-wms-dashboard.png" alt="">
        <figcaption>上线后的 WMS 看板</figcaption>
      </figure>
    </div>
  </div>
</section>
```

## 分步揭示

模板脚本原生支持:给元素加 `.frag`,→ 依次点亮,点完才翻页;往回翻自动全亮。只动 opacity,元素始终占位,不跳版、不扰乱 auto-fit。

```html
<div class="body">
  <ul>
    <li>一直可见的前提</li>
    <li class="frag">按一下才出现</li>
    <li class="frag">再按一下</li>
  </ul>
</div>
```

## 引用页

客户原话,细线分隔:

```html
<div class="body">
  <p class="quote">"以前排查一次要两小时,现在告警自己就把根因贴脸上了。"
    <span class="by">— 姓名 · 头衔 · 公司</span></p>
</div>
```

## 全幅大图页

图占满、文字压在上面(深色渐变保证可读):

```html
<section class="slide" style="padding:0">
  <img src="assets/hero.jpg" alt="" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover">
  <div style="position:absolute;inset:0;background:linear-gradient(180deg,transparent 40%,rgba(5,11,20,.82))"></div>
  <div style="position:absolute;left:8cqw;bottom:9cqh;color:#fff;max-width:70cqw">
    <h2 style="font-size:4.6cqw">压在图上的标题</h2>
    <p class="mt" style="color:#C9CDD3">一句说明。</p>
  </div>
</section>
```

## 换主题色

默认 Tier0 绿(henkel总体)。**AWS-TIER0 用的是 SUPCON 炫彩渐变**——只改这一段,中性色 token 一律不动:

```css
:root { --accent: #0057E3; }
.hl, .metric {
  background: linear-gradient(100deg,#00B3FF 0%,#0057E3 36%,#5A2BE0 70%,#C71E8E 100%);
  -webkit-background-clip: text; background-clip: text;
  -webkit-text-fill-color: transparent; color: transparent;
}
```

深底页上的 `.hl` 仍用 `--lime`,渐变在黑底上会糊。
