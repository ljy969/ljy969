# SETUP · Claude 风格主页改造说明

这套设计模仿 Claude / Anthropic 的界面语言：暖纸底色、陶土橘点缀、衬线标题、发丝级分隔线，
并全程跟随 GitHub 深色 / 浅色模式自动切换（`<picture>` + `prefers-color-scheme`）。

## 一、怎么部署

1. 解压后把**整个目录**覆盖到你本地 clone 的 `ljy969/ljy969` 仓库（或直接把内容拖进仓库）。
2. `git add . && git commit -m "redesign: claude-style profile" && git push`
3. 仓库已自带一版静态贪吃蛇占位图（`assets/claude/snake-light.svg` / `snake-dark.svg`），
   push 后 README 不会出现破图；`.github/workflows/snake.yml` 会在 push 后和每天 UTC 00:23
   自动用真实贡献数据重新生成这两张动图并提交回来（约 1 分钟）。
   - 若 Actions 没有自动运行：仓库页 → Actions 标签 → 左侧选 "Contribution Snake" →
     Enable workflow / Run workflow。

## 二、文件地图

```
README.md               英文主页（默认展示）
README.zh-CN.md         中文主页
assets/claude/
  banner-{lang}-{mode}.svg      顶部横幅卡片（4 个：中英 × 深浅）
  h-{section}-{lang}-{mode}.svg 七个章节标题（01-07 编号 + 衬线标题 + 发丝线）
  tech-{lang}-{mode}.svg        技术栈徽章条（自制圆角 chips + 真实品牌图标）
  divider-{mode}.svg            页脚前的分隔线
  footer-{lang}-{mode}.svg      页脚致谢卡
  snake-{light,dark}.svg        贡献贪吃蛇（Actions 每日自动重新生成，勿手改；
                                首版为静态占位图，随首次 Actions 运行替换为动图）
.github/workflows/snake.yml     贪吃蛇每日自动生成工作流（Platane/snk，
                                蛇 / 格子颜色在该文件 outputs 两行里改）
```

其余原有文件（LICENSE、CODE_OF_CONDUCT.md、CONTRIBUTING.md 等）原样保留。

## 三、改文字 / 配色

所有 SVG 都是纯文本，直接用编辑器打开改：

- **横幅里的名字 / 简介**：搜 `MOX` 和副标题文字，四个 banner 文件同步改。
- **配色**（两套主题）：
  - 浅色：纸底 `#F7F4EC`、正文 `#3D3929`、次要 `#8A8577`、发丝线 `#D9D2C2`、陶土橘 `#C96442`
  - 深色：纸底 `#1B1A17`、正文 `#C9C5B8`、次要 `#8B877C`、发丝线 `#36342E`、陶土橘 `#D97757`
- **统计卡 / 连续贡献卡**：颜色写在 README 的两个长 URL 参数里（`bg_color=`、`text_color=` 等），
  与上表保持一致即可。
- **贪吃蛇颜色**：改 `.github/workflows/snake.yml` 里 `outputs:` 两行的 `color_snake=` / `color_dots=`。

## 四、GitHub Markdown 排版规则（重要，别改坏）

README 里两张卡片表格（精选项目、数据一览）能正确分栏渲染，依赖一条
GitHub Markdown 解析规则：**空行之后的 HTML 标签行必须顶格书写（行首无缩进）**。

- 为什么：GitHub 把空行视为 HTML 块的结束；空行后如果出现缩进 4 空格以上的
  标签行（如 `    </td>`），会被解析为「缩进代码块」，于是 `</td>`、
  `<td width="50%">` 这类原始标签会以可见代码块的形式泄漏到页面上，
  表格分栏也随之失效（2025-11 就是因为这个问题出现过泄漏）。
- 怎么做：表格区域里所有 `<table>`、`<tr>`、`<td>`、`</td>` 等标签行一律顶格；
  单元格内的 Markdown 内容（`**加粗**`、`` `代码` `` 等）前后保留空行即可正常渲染。
  两个 README 表格上方都留有注释提醒，请勿删除。
- 其他区域（`<p align="center">`、`<picture>` 组）内部没有空行，属于完整
  HTML 块，保持原样即可，不要在其中插入空行。

## 五、奖杯卡片与镜像维护

「里程碑奖杯」卡片不再使用官方实例 `github-profile-trophy.vercel.app`
（该实例已被主机商停用，返回 HTTP 402 DEPLOYMENT_DISABLED，所有人都是破图），
现改用官方 README 列出的社区负载均衡镜像 `github-trophies.vercel.app`。

- 如果某天奖杯又显示不出来：先 `curl -I` 看镜像是否返回非 200，
  再把 README（中英文两份）里奖杯 URL 的域名换成下列任一可用镜像：
  `github-profile-trophy-unserori.vercel.app` · `github-trophy-jagratadeb.vercel.app`
- 深色主题用的是 `theme=gruvbox`（暖棕底 + 陶土色点缀，与整套设计同调），
  浅色用 `theme=flat`；如需其他主题可查官方 README 的主题表。

## 六、本次精简掉的旧模块（可自行加回）

- 顶部彩色小徽章条（status / Python 3.12 / contributors / forks / visitor 计数器）
- GitAscii 字符画横幅（中文版）
- mermaid 机器学习结构图、Skyline、3D 贡献图（原本就未被 README 引用）
- 随机开发者名言、贡献最多仓库两张卡
- 技术栈里偏硬件 / 娱乐的徽章：Steam、AMD、NVIDIA、XFCE、OpenGL、Unity、Adobe、Gitee

保留了：统计三卡、Star History、奖杯（全部换成暖色 / 无框样式），并新增了「精选项目」表格卡片区。
