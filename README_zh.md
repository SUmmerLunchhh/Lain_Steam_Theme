> [English](README.md) | **简体中文**

# Lain — Steam 主题

为 Steam 客户端做的《Serial Experiments Lain》风格主题：纯黑底、旧米色文字、脏粉描边、全角标签，加上一层很轻的 CRT 扫描线和闪烁。

基于 [blueArchiveSteamSkin](https://github.com/EugeneXXXie/blueArchiveSteamSkin)（作者 EugeneXXXie，MIT）的骨架改写，原始文件结构和部分设置页样式来自该项目。

## 效果

- 库页面、商店、社区、下载、好友列表全部换成 Lain 的界面
- 标题、图标、右键菜单、设置页统一配色
- 新闻块右上角有「隐藏 / 显示」按钮，状态记在本地
- 好友按钮上的 momo 图标
- 音效：打开 Steam 时的开机音，加上 CRT 底噪、悬停和点击音
- 切换界面音效：点「商店 / 库 / 社区 / 个人主页」时各有一段对应的音，点右下角「好友与聊天」也有

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

- **Effects** 三档 —— `Off` 全关（最省）；`Subtle`（默认）轻扫描线 + 胶片颗粒 + 悬停时文字解码；`Full` 更重的扫描线、1.6 秒一圈的刷新带、每 14–24 秒一次的整屏抖动、右键菜单边框逐段绘制。
- **Sound**（底噪 / 悬停 / 点击）—— 关掉就完全没声音。

## 声音素材

六个音频放在 `music/` 里：开机音（`Lain.ogg`）、切到商店、切到库、切到社区、切到个人主页、右下角好友按钮。全部是 Ogg Opus，加起来约 76 KB。

`js/sound.js` 里同时内嵌了同一批音频的 base64 副本 —— 因为 Steam 的界面跑在 CEF 里，注入的 JS 拿不到可靠的主题目录路径，直接读 `music/*.ogg` 有被拦的风险，内嵌那份是"保证能响"的版本。

要替换音效的话两边都要更新，细节见 [music/README.md](music/README.md)。

## 已知限制

- Steam 每次大版本更新会重算 CSS 类名（那些 `_1TdaAqMFadi0UTqilrkelR` 之类），届时部分位置可能掉样式，需要跟着更新。
- 开机音受 Chromium 的自动播放策略限制：如果 Steam 启动时页面没有得到过鼠标/键盘交互，声音会被推迟到你第一次点击时播放。这是浏览器的硬性规定，绕不过去，所以有时会感觉「开机音慢了一拍」。
- 只在 Windows + Millennium 最新版上测过。

## 许可

代码部分沿用上游的 [MIT License](LICENSE)。原作者版权声明保留，Lain 主题的改动部分版权归 SUmmerLunch。

主题里使用的图片、字体和音频来自动画《Serial Experiments Lain》，版权归原权利人所有，仅作个人欣赏用途。
