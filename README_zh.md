> [English](README.md) | **简体中文**

# Lain — Steam 主题

为 Steam 客户端做的《Serial Experiments Lain》风格主题：近黑底色、荧光青与脏粉、全角字符标签，叠加 CRT 扫描线、胶片颗粒和开机点亮。视觉上刻意做成 1998 年那台机器的样子 —— 低饱和、有颗粒、反应迟钝，而不是现代那种高饱和霓虹。

基于 [blueArchiveSteamSkin](https://github.com/EugeneXXXie/blueArchiveSteamSkin)（作者 EugeneXXXie，MIT）的骨架改写，原始文件结构和部分设置页样式来自该项目。

## 效果

**界面**

- 库页面、商店、社区、下载、好友列表全部换成 Lain 的界面
- 90 年代窗口的「内亮外暗」斜面边框，取代现代圆角
- hover 用 `steps()` 瞬跳，没有缓动曲线 —— 那个年代的反馈是"立刻"
- 标题、图标、右键菜单、设置页统一配色
- 新闻块右上角的「隐藏 / 显示」按钮，状态记在本地
- 好友按钮上的 Navi 图标
- 游戏启动弹窗：深色底、封面描边，启动过程中有一条亮带反复扫过
- 通知与成就弹窗做成「信号接收」：进入时扫过一条亮带，右上角有闪烁的接收指示

**动效**（三档可调）

- **开机 CRT 点亮**：一条亮线从中间展开成整屏再淡出，2.4 秒，配开机音
- 扫描线、胶片颗粒、缓慢滚动的场刷新带
- 鼠标划过文字时的**解码**：先变成 `▓▒░01ｱｲ` 再逐字定型
- Full 档另有：更重的扫描线、1.6 秒一圈的刷新带、每 14–24 秒一次的整屏抖动、右键菜单边框逐段绘制

**音效**

- CRT 底噪、悬停与点击反馈
- 切换界面音：商店 / 库 / 社区 / 个人主页，以及右下角「好友与聊天」

## 安装

### 1. 先装 Millennium

主题跑在 [Millennium](https://steambrew.app/) 上，需要先装它（否则 Steam 不认 `skin.json`）。

### 2. 放主题文件

把整个 `LainTheme` 文件夹放到 Millennium 的主题目录：

- **Windows**：`C:\Program Files (x86)\Steam\millennium\themes\`
- **Linux**：`~/.local/share/Steam/millennium/themes/`（或你 Steam 的安装位置）

放好后目录应该是：

```
millennium/themes/LainTheme/skin.json
```

> 注意：网上有些教程让放到 `steamui/skins`，那是 Steam 自带皮肤的老路径，和 Millennium 不是一回事。

### 3. 启用

打开 Steam → 设置 → 主题 → 选 **Lain**。

## 主题设置

界面文案是英文的（上架要求），Steam 设置 → 主题 → Lain → **Effects** 标签页：

- **Effects** 三档
  - `Off` —— 全关，最省。开机点亮也会一起关掉。
  - `Subtle`（默认）—— 轻扫描线 + 胶片颗粒 + 缓慢刷新带 + 悬停文字解码
  - `Full` —— 更重的扫描线、快速刷新带、偶发整屏抖动、菜单边框绘制
- **Sound**（底噪 / 悬停 / 点击）—— 关掉就完全没声音。

档位是通过 CSS 里的自定义属性 `--lain-tier` 传给 JS 的，所以选 `Off` 时 JS 里的解码和抖动根本不会启动，不是"启动了但看不见"。

## 声音素材

六个音频放在 `music/` 里：开机音（`Lain.ogg`）、切到商店、切到库、切到社区、切到个人主页（Bôa - Duvet 前 10 秒）、右下角好友按钮。全部是 Ogg Opus，加起来约 127 KB。

`js/sound.js` 里同时内嵌了同一批音频的 base64 副本 —— 因为 Steam 的界面跑在 CEF 里，注入的 JS 拿不到可靠的主题目录路径，直接读 `music/*.ogg` 有被拦的风险，内嵌那份是"保证能响"的版本。

要替换音效的话两边都要更新，细节见 [music/README.md](music/README.md)。

## 已知限制

- Steam 每次大版本更新会重算 CSS 类名（那些 `_1TdaAqMFadi0UTqilrkelR` 之类），届时部分位置可能掉样式，需要跟着更新。
- Steam 桌面客户端关闭了 Chromium 的自动播放限制，所以音效在启动时直接播放，不需要先点一下。如果 Valve 哪天打开这个限制，代码里有「第一次点击后再响」的兜底。
- 登录界面（未登录启动时那个）无法定制：Millennium 的注入发生在登录之后，登录窗口不在它的补丁范围内，所以那里没有主题样式、也没有音效。
- 文字解码会临时改写 Steam 自己的文本节点，React 下一次渲染会覆盖回去。正常使用没有问题，但如果看到某段文字卡在乱码状态，请提 issue。
- Full 档的整屏抖动会给 `body` 加 transform，那一瞬间所有固定定位的元素会一起位移 —— 这是效果的一部分。
- 失去焦点（窗口被别的程序完全盖住、或点到 Steam 的子窗口）时，底噪会淡出并暂停音频上下文，有 600ms 宽限，切回来会恢复。
- 只在 Windows + Millennium 最新版上测过。

## 许可

代码部分沿用上游的 [MIT License](LICENSE)。原作者版权声明保留，Lain 主题的改动部分版权归 SUmmerLunch。

主题里使用的图片、字体和音频来自动画《Serial Experiments Lain》，版权归原权利人所有，仅作个人欣赏用途。
