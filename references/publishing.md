# mercydeck 发布与兼容规范

目标是自包含 HTML deck，在本地浏览器及 `m3rcyzzz.club/app/` 的上传、预览、编辑与离线播放管线中可用。站点发布地址为 `https://m3rcyzzz.club/<slug>/`。以下是作者交付规范，不能声称服务器会逐条强制校验。

## 文件与资源

- 一份 deck 一个文件夹，根目录 `index.html`，CSS/JS 内联。
- 图片、视频等可放 `assets/`、`media/`、`videos/`。文件名用小写 ASCII 与连字符；引用为相对路径，不以 `/` 开头，不跨 `..`，大小写与磁盘一致。
- 可内嵌图片、SVG 或已有可用字体；零外部资源依赖。不加载 Google Fonts、CDN 脚本、外链图片或远程播放器。正常来源链接、CTA 外链不属于加载依赖，可以保留。
- 原参考内嵌 IBM Plex Sans SC / IBM Plex Mono。未携带字体时使用系统回退，并据实际字形检查换行。不要把需要网络加载的字体误写成离线可用。
- 保留 `</body>` 供宿主注入脚本。宿主维护的 `__deck-*` 脚本和 `.deck-media.json` 不要硬编码为新 deck 的外部必需项。

## 视频

优先 H.264 MP4、最高 1080p、CRF 20–23，启用 faststart。按素材比例缩放，不拉伸：

```bash
ffmpeg -i in.mp4 -vf "scale='min(1920,iw)':'min(1080,ih)':force_original_aspect_ratio=decrease,scale=trunc(iw/2)*2:trunc(ih/2)*2" -c:v libx264 -crf 20 -preset medium -c:a aac -b:a 128k -movflags +faststart out.mp4
```

无声自动演示带 `muted playsinline preload="metadata" controls`；只有适合重复观看时加 `loop`。factory-software 的演示视频没有默认循环，不能再把 loop 当硬要求。需要声音时保留用户手动播放途径。

只播放当前页面、当前可见状态的视频，退出状态/页面、打开遮挡播放的总览或浏览器标签隐藏时暂停。`play()` 被浏览器阻止时不产生未处理异常，保留 controls；点击视频控件不能同时翻页。

## 静态文字与宿主交互

所有 slide 及其各步正文预先存在 DOM；正文放在普通 HTML 标签里，不以 canvas、图片文字或 JS 渲染代替。图解可用 SVG，但需要双击编辑的正文放 HTML。脚本只切换显示状态，不在翻页时用 `textContent`、`innerHTML` 或动画覆盖编辑后的文字。

新模板采用 `.slide.active` 标记当前页，初始封面可见，状态有前进/回退。非当前页面不能被 Tab 聚焦。链接、按钮、视频、输入框及 contenteditable 都有自己的操作，不触发空白区域翻页；双击文本不能先翻到下一页。

当前宿主有两个实际集成点（2026-10-21 代码核对；下列路径相对宿主项目，宿主源码不随本技能发布，也不是本地制作 deck 的依赖）：

- 宿主 `deck-assets/preload.js` 注入触屏导航 `#__deck-navbtns`，通过向 `document.body` 派发可取消的合成方向键驱动 deck。消费方向键时调用 `preventDefault()`，不要拒绝 `isTrusted === false` 的事件。
- 宿主 `src/routes/edit.$slug.tsx` 的编辑视图注入 `style[data-deckedit]` 并捕获指针事件；deck 不应抢占编辑点击，仍须响应编辑器按钮发出的合成方向键。

不额外复制一套常驻 HUD，不虚构宿主 postMessage/API 合同。`about:srcdoc` 编辑预览中不要强行改写地址；本地/正式页可使用 `#页号`，更新失败不影响放映。

## 视口与检查

必须有 `<meta name="viewport" content="width=device-width, initial-scale=1.0">`。模板使用 1600×900 设计舞台，按视口整体等比缩放；竖屏保留 16:9 留边，不重排为长页。页面内容超过舞台时拆分或改写，不靠单页 auto-fit 把字体越缩越小。

交付前在实际浏览器完成：

1. 本地打开入口即见封面；左右键、空格、点击与触摸可前进/回退，含分步状态；首页/末页边界正常。
2. 逐页及逐步检查 1600×900、1366×768 与 390×844：无文字裁切、覆盖、横向滚动；关注长标题、三栏、双视频和 Bento。新字体加载后重查。
3. 检查无外部加载依赖、资源缺失或控制台错误；文件路径大小写准确。区分“无网络请求”与“浏览器已经缓存了外部资源”。
4. 实际媒体可播放，状态退出与标签隐藏会暂停；静音自动播放失败时仍能手动播放。没有素材的示例占位在最终交付前替换或明确说明。
5. 静态文字修改后前进/回退不会被脚本重写；双击、视频控件、链接、输入不会误翻页；合成方向键只前进一步。
6. reduced motion 下内容可读、状态可达；关闭动画不会丢失结论；非当前 slide 不抢焦点。
7. 如果任务包含上传/发布，再在宿主预览/编辑与公开链接中核对注入导航、文字保存和离线缓存；只测本地时如实说明未做线上验收。

交付文件夹或 ZIP，URL slug 建议短英文。只在用户已要求发布时上传，不因自检自动改动线上 deck。
