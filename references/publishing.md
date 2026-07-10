# mercydeck 发布规范(m3rcyzzz.club 托管管线)

deck 最终跑在用户自建的托管栈上:AWS 源站 → Cloudflare 隧道/边缘缓存 → 观众(常在国内、可能手机、可能弱网)。管理端 `m3rcyzzz.club/app` 上传后,deck 发布在 `m3rcyzzz.club/<slug>/`。管线会注入离线缓存/预加载脚本、双击文字编辑器。以下规范是为了让 deck 与这套管线无摩擦。

## 文件与结构

- **一个 deck = 一个文件夹**,入口必须是根目录 `index.html`,单文件内联 CSS/JS(样式脚本不拆文件)。
- 媒体放子目录:`videos/`、`assets/`。**全部小写 ASCII 文件名**(`hero-demo.mp4`,不要 `Hero Demo.mp4`/中文名)——托管在 Linux,大小写敏感;虽然管线有大小写自愈,但别依赖它。
- **只用相对路径**(`videos/x.mp4`),不要以 `/` 开头、不要跨目录 `..`。
- **零外部依赖**:不引 Google Fonts、CDN JS、外链图片。观众在国内,fonts.googleapis 会卡死加载;离线缓存也存不到外部资源。字体用系统栈(模板已配 PingFang/雅黑/IBM Plex 回退)。
- 保留 `</body>` 标签(管线靠它注入预加载/离线脚本)。
- 交付前把文件夹打成 zip 或整夹给用户,提醒:`/app` 上传时可自定义 URL 路径(slug 用短英文)。

## 视频规范(有硬教训)

- **H.264 mp4、≤1080p、CRF 20–23**。用户曾传过 4K/16Mbps 的录屏,169MB 一个——压到 1080p CRF20 后 8MB 且肉眼无差。ffmpeg 一行:
  ```bash
  ffmpeg -i in.mp4 -vf "scale='min(1920,iw)':'min(1080,ih)':force_original_aspect_ratio=decrease,scale=trunc(iw/2)*2:trunc(ih/2)*2" -c:v libx264 -crf 20 -preset medium -c:a aac -b:a 128k -movflags +faststart out.mp4
  ```
- `<video>` 必带:`muted loop playsinline preload="metadata" controls`(iOS 内联播放靠 playsinline;muted 才允许自动播)。模板导航已做"进页自动播、翻走暂停"。
- 演示录屏默认无声循环;确需声音的单独说明并去掉 muted loop。

## 移动端

- 必须有 `<meta name="viewport" content="width=device-width, initial-scale=1.0">`。
- 16:9 舞台在竖屏手机上会上下留黑边——正常,不做竖版重排。
- 触摸翻页(swipe)模板已带;点击区(右 40% 下一页/左 15% 上一页)桌面移动通用。

## 与编辑器/离线缓存共处

- 文字放普通标签里(h1/h2/p/li/span),**不要用 JS 渲染文字内容**——管理端的双击编辑器改的是 DOM 静态文本,JS 生成的文字改不了。
- 幻灯片全部内容预先在 DOM 里(display:none 切换,模板即是),不要懒挂载——离线缓存和预加载都靠扫 DOM/清单。

## 交付自检清单

发给用户前逐条过:

1. `index.html` 在根目录,打开即封面,←/→、空格、点击右侧、手机滑动都能翻页
2. 无任何外链请求(断网打开本地文件夹也应完整渲染,视频除外)
3. 所有文件名小写 ASCII;HTML 里引用与磁盘一致
4. 视频全部 ≤1080p、带 `muted loop playsinline preload="metadata" controls`
5. 手机宽度(390px)模拟:无横向滚动,文字可读
6. **16:9(如 1600×900)逐页看竖直方向**:内容不被裁切、标题没被正文盖住、大数字页是横向并排。模板 auto-fit 会兜底把超高的页等比缩小(不裁切),但**若某页明显被缩小**就是塞太多了——拆页或降密度,别靠缩放救
7. 每个内容页有 eyebrow(`大类 · 细分` 英文 mono)+ 标题;`.row` 都嵌在 `.body` 里(没有 `class="body row"`)
8. 单页文字 ≤5 个要点;数字/结论用 `.hl` 或 `.big` 强调
