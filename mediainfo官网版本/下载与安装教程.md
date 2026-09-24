# MediaInfo 下载与安装

> 本文档整理 MediaInfo 的官方下载渠道、各平台安装方法与基本使用方式，供官网版本软件页参考。

## 一、软件简介

MediaInfo 是一款开源免费的多媒体文件信息查看工具，由 MediaArea 团队维护。它能够读取并展示音视频文件的**容器格式、编码格式、码率、分辨率、帧率、时长、音轨与字幕轨、语言、章节**等技术参数，常用于：

- 核对下载到的影片 / 音乐文件的实际规格是否与描述一致；
- 排查「能播放但没声音」「画面卡顿」这类由编码或封装引起的问题；
- 为转码、压制、归档工作提供参数依据。

它提供两种形态：

| 形态 | 说明 | 适用人群 |
| --- | --- | --- |
| **GUI 图形界面版** | 拖入文件即可查看，支持多视图切换 | 普通用户 |
| **CLI 命令行版** | 可批量处理、可输出 JSON / XML，便于脚本调用 | 进阶用户、自动化流程 |

两种形态通常包含在同一个安装包中，安装后即可同时使用。

## 二、官网与下载地址

- 官方网站：<https://mediaarea.net/MediaInfo>
- 官方下载页：<https://mediaarea.net/en/MediaInfo/Download>

**请优先从官方网站下载。** 第三方软件站提供的安装包可能被捆绑推广程序或二次修改，存在安全风险。

官方下载页会按操作系统自动推荐对应版本，并同时列出 Windows、macOS、Linux 的完整安装包清单。

## 三、Windows 安装

Windows 平台提供两类版本，按需选择：

### 1. 安装版（推荐）

1. 打开官方下载页，选择 **Windows** 分类，下载 `.exe` 安装程序；
2. 双击运行，按向导提示完成安装（默认会同时安装 GUI 与 CLI）；
3. 安装完成后，开始菜单中会出现 MediaInfo 快捷方式。

### 2. 便携版

下载 `.zip` 压缩包，解压到任意目录，直接运行其中的可执行文件即可，**不写注册表、可放在 U 盘中随身携带**。

> 提示：若需要命令行使用，请在安装时勾选将 MediaInfo 加入系统 `PATH`，否则需手动添加安装目录。

## 四、macOS 安装

### 方式一：官方 dmg 安装包

1. 从官方下载页下载 `.dmg` 文件；
2. 双击打开，将 MediaInfo 图标拖入「应用程序」文件夹；
3. 首次运行时若提示「无法验证开发者」，前往「系统设置 → 隐私与安全性」点击「仍要打开」。

### 方式二：Homebrew（适合命令行用户）

```bash
brew install --cask mediainfo
```

安装后即可在终端直接调用 `mediainfo` 命令。

## 五、Linux 安装

### Debian / Ubuntu 系

```bash
sudo apt update
sudo apt install mediainfo mediainfo-gui
```

### Fedora / RHEL 系

```bash
sudo dnf install mediainfo mediainfo-gui
```

### Arch 系

```bash
sudo pacman -S mediainfo
```

### 通用方式

官方下载页同时提供 `.deb`、`.rpm` 安装包以及免安装的 `.tar.xz` 二进制包，适用于上述包管理器未收录或需要指定版本的场景。

## 六、基本使用

### GUI 图形界面

1. 打开 MediaInfo；
2. 将音视频文件拖入窗口（或通过「文件 → 打开」选择）；
3. 在「视图」菜单中切换展示方式：
   - **基本**：只显示关键参数，适合快速查看；
   - **树状**：按 常规 / 视频 / 音频 / 文本 / 章节 分组展开；
   - **文本 / HTML / XML**：便于复制粘贴或程序处理。

### 命令行

```bash
# 查看单个文件信息
mediainfo video.mkv

# 输出 JSON（便于脚本解析）
mediainfo --Output=JSON video.mkv

# 只提取分辨率与时长
mediainfo --Inform="Video;%Width%x%Height% %Duration%" video.mkv

# 批量处理当前目录下的所有 mp4 文件
for f in *.mp4; do echo "== $f"; mediainfo --Inform="Video;%Width%x%Height% %BitRate%" "$f"; done
```

## 七、常见问题

**Q：安装后命令行提示「找不到 mediainfo 命令」？**
A：多为安装目录未加入系统 `PATH`。Windows 可重新运行安装程序勾选该选项；Linux 请确认已安装 `mediainfo`（非仅 `mediainfo-gui`）包。

**Q：某些文件读取不到时长或码率？**
A：说明文件头信息不完整或封装异常，常见于未下载完整、被截断的文件。可先用播放器确认文件能否正常播放，必要时重新获取完整文件。

**Q：需要看更详细的底层数据？**
A：在 GUI 中把视图切到「文本」或「XML」，或使用 CLI 的 `--Full` 参数输出全部字段。

**Q：如何确认下载到的安装包未被篡改？**
A：官方下载页提供校验值，可用 `certutil -hashfile <文件名> SHA256`（Windows）或 `shasum -a 256 <文件名>`（macOS / Linux）比对。

## 八、小结

| 平台 | 推荐安装方式 |
| --- | --- |
| Windows | 官方 `.exe` 安装版；需便携则用 `.zip` |
| macOS | 官方 `.dmg`；命令行用户用 `brew install --cask mediainfo` |
| Linux | 系统包管理器（apt / dnf / pacman）或官方 `.deb` / `.rpm` |

无论使用哪个平台，都建议**从官网下载**并定期更新到最新版本，以获得更好的新格式支持与问题修复。
