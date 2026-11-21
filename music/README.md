# music/

主题用到的全部声音，Ogg Opus 单声道格式，七个加起来约 1.3 MB。

| 文件 | 用途 | 代码里的 key | 大小 |
|---|---|---|---|
| `Lain.ogg` | 进入库时的开机音 | `STARTUP_B64` | 27.0 KB |
| `store.ogg` | 切到商店 | `NAV_B64.store` | 7.3 KB |
| `library.ogg` | 切到库 | `NAV_B64.library` | 9.3 KB |
| `community.ogg` | 切到社区 | `NAV_B64.community` | 7.2 KB |
| `duvet.ogg` | 个人主页背景音乐（Bôa - Duvet 整首，播一遍） | `NAV_B64.profile` | 1,262 KB |
| `personal-page.ogg` | 切到个人主页的短音效 | `NAV_B64.profileClick` | 9.4 KB |
| `friends.ogg` | 点右下角「好友与聊天」按钮 | `NAV_B64.friends` | 15.7 KB |

## 这几个文件跟实际播放的关系

`js/sound.js` 里内嵌着**同样字节**的 base64 副本（`Lain.ogg` 除外，那份是同一批转码参数单独转的）。实际发声用的是内嵌那份，原因：Steam 界面跑在 CEF 里，注入的 JS 拿不到可靠的主题目录路径，直接读 `music/*.ogg` 有被拦的风险。

所以 `music/` 的定位是**可读的素材存档**：想换音、想听、想归档、想让别人知道这是什么，看这里；真正生效的是 `js/sound.js`。

## 想换某个音

1. 把新音频转成 Ogg Opus（单声道 48–64 kbps）
2. 替换这个文件夹里对应的文件
3. 把同样的音频重新 base64 内嵌进 `js/sound.js`
4. 重启 Steam

第 3 步不做的话，`music/` 里换了也不会响——因为播放读的是内嵌那份。

## 为什么不是 WAV

原始 WAV 一共 984 KB，转成 Ogg 后 76 KB，听感几乎没差别。省下来的体积对仓库和主题加载都更友好。
