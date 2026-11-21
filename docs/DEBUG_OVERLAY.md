# 临时调试浮层 — 删除说明

> 这份文档是写给"以后的自己"的。如果你（或另一个 AI）接手时不知道该删什么，
> 只要读这一页就够了，不需要理解整个主题。

## 这是什么

一个只在本地开发时用的状态浮层，用来在**没有开发者工具**的情况下看到主题内部状态。

- **开关**：`Ctrl + Alt + D`（再按一次关闭）
- **默认**：关闭。关闭时不会创建任何 DOM 元素，开销为零
- **位置**：屏幕右侧、垂直约 38% 处，黑底绿字

显示内容：

```
lain debug
tier    subtle        ← 当前效果档位（off / subtle / full）
ctx     running       ← AudioContext 状态（suspended / running）
startup buffer=ok played=1   ← 开机音是否解码完成 / 是否已播过
login?  no            ← 当前是否有密码输入框（判断登录界面）
nav     found         ← 是否已检测到顶部导航（判断是否进库）
cursor  custom        ← 自定义光标是否生效（判断 global.css 有没有加载）
```

## 引入版本

`1.7.2`

## 怎么删（三步）

### 1. 删掉代码块

打开 `js/sound.js`，搜索：

```
LAIN DEBUG OVERLAY 起
```

从这个标记所在的那一行开始，一直删到：

```
LAIN DEBUG OVERLAY 止
```

**包含这两行标记本身。** 删完之后，文件的结尾应该回到：

```js
    scheduleStartupSound();
  })();
})();
```

### 2. 验证没有残留

```powershell
cd <主题目录>
Select-String -Path 'js\sound.js' -SimpleMatch 'lainDebugOverlay'
Select-String -Path 'js\sound.js' -SimpleMatch 'LAIN DEBUG OVERLAY'
```

两条都应该**没有任何输出**。

### 3. 语法检查 + 发布

```powershell
node --check js/sound.js
```

通过之后提交，并把 `skin.json` 的 `version` 升一位（例如 1.7.2 → 1.8.0），这样装过旧版的人能收到更新。

## 不需要动的东西

| 项 | 说明 |
|---|---|
| `skin.json` | 没有为调试浮层加任何配置项 |
| CSS 文件 | 浮层样式全部是内联的，没有依赖任何 `.css` |
| 其他 JS | 浮层只读取已有变量，没有被别处引用 |

## 删掉会影响功能吗

不会。这个代码块：

- 不被任何其他函数调用
- 不修改任何状态（只读 `ctx` / `wantStartup` / `wantLogin` / `playStartup` 等变量并显示）
- 唯一注册的事件是 `keydown` 上的一个监听器，随代码块一起消失

## 顺带说明：为什么当初要做这个

Millennium 主题是往 Steam 的 CEF 里注入 CSS/JS，**没有开发者工具、没有热更新、没有控制台**。
所以"改了没生效"这种事只能靠肉眼猜。这个小浮层就是替代方案：出问题时按一下、
截个图，就能看出是哪一环断了（没注入 / 没解码 / 上下文挂起 / 档位是 off）。
