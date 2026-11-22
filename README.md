# 小手机 V1

这是**第一版**。已经被 V2 取代了，但留着是有用的——它是"做成 App 装在手机里"的那一版。

## 它和 V2 的根本差别

| | V1（这个仓库） | V2（[xiaoshouji-server](https://github.com/QwQing0118/xiaoshouji-server)） |
| --- | --- | --- |
| 形态 | Android App，WebView 装一个单文件 HTML | 零依赖 Node 服务 + 浏览器访问 |
| 数据 | 全在 WebView 的 localStorage | SQLite（`data/sm.db`） |
| 后端 | 没有 | 有，自带工具引擎、心潮注入、多模态 |
| 清缓存 | 数据全丢 | 数据在服务端，不会丢 |
| 离线 | 能 | 需要服务在跑 |

V1 的界面部分其实做得很完整——桌面图标网格、可拖动 Dock、聊天流、长按菜单、引用回复、搜索、记录 / 收着的 / 日记 / 相册 / 备忘录 / 日历、记忆库卡片散落布局和连线、设置里的外观字体气泡壁纸、小游戏。V2 把这些重写成了带后端的版本。

## 目录结构

```
app/
├── src/main/
│   ├── AndroidManifest.xml
│   ├── java/com/qwqing/phone/MainActivity.java   # 整个 App，就这一个类
│   ├── res/values/strings.xml                    # 应用名
│   ├── res/values/styles.xml
│   ├── res/mipmap-anydpi-v26/ic_launcher.xml
│   └── assets/index.html                         # 整个前端（约 519 KB）
├── build.gradle                                   # applicationId com.qwqing.phone
.github/workflows/build.yml                        # 推上去自动出 APK
xiaoshouji-v1-webview519k.apk                      # 现成的安装包
```

## 怎么装

直接下仓库里的 `xiaoshouji-v1-webview519k.apk`，传到手机上装。

包名 `com.qwqing.phone`，versionCode 1，minSdk 24，targetSdk 34。

## 怎么改

`MainActivity.java` 只有一件事：起一个全屏 WebView，开 JS / DOM Storage / 文件访问 / 文件选择器，加载 `file:///android_asset/index.html`。所有界面和逻辑都在 `assets/index.html` 里，改完重新打包就行。

想重新出包：在 GitHub 上把仓库推到 `main`，`.github/workflows/build.yml` 会用 Gradle 8.7 + JDK 17 跑 `assembleDebug`，产物在 Actions 的 artifact 里。

## 两个 APK 的区别

- **`xiaoshouji-v1-webview519k.apk`**（198 KB，本仓库）：对应 `smbuild/`，`assets/index.html` 是 519 KB 那一版，也就是本地最完整的那份。
- Release 标签 `v1.0` 挂的 `xiaoshouji.apk`（89 KB）：对应 `sm_src/`，`index.html` 只有 204 KB，是十月二十号的更早快照。

要玩就装前者。

## 现在不用它的原因

数据全在浏览器存储里，清缓存就没了；没有后端，所以没有真正的记忆库搜索、没有工具调用、没有心潮。这些问题 V2 都解决了，所以这个仓库只作存档，不再往下做。
