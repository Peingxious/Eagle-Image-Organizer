# Obsidian Eagle Image Organizer

这是一个用于将 Obsidian 与 Eagle 集成的插件，能够在笔记中便捷地插入与管理 Eagle 资产（图片/视频/音频等），并支持上传、反向同步、标签同步等工作流。

[Eagle](https://eagle.cool) 是一款强大的素材管理软件，适合收藏、整理与检索大量图片、视频、音频等素材，支持 Windows。

## 功能概览

- 资产插入：在 Obsidian 内搜索并插入 Eagle 资产
- 拖拽/粘贴上传：将本地图片拖入/粘贴到笔记，自动上传至 Eagle
- 反向同步：自动或手动将笔记中的 Eagle 链接标题更新为 Eagle 实际文件名
- 标签同步：将当前笔记的 YAML 标签同步到链接涉及的 Eagle 项
- 资产管理：在右键菜单中修改 Eagle 资产属性（名称、标签、注释等）或移动到目标文件夹
- 多库支持：配置多个 Eagle 库路径，兼容多设备差异

[![GitHub stars](https://img.shields.io/github/stars/zyjGraphein/Obsidian-EagleBridge?style=flat&label=Stars)](https://github.com/zyjGraphein/Obsidian-EagleBridge/stargazers)
[![Total Downloads](https://img.shields.io/github/downloads/zyjGraphein/Obsidian-EagleBridge/total?style=flat&label=Total%20Downloads)](https://github.com/zyjGraphein/Obsidian-EagleBridge/releases)
[![GitHub Release](https://img.shields.io/github/v/release/zyjGraphein/Obsidian-EagleBridge?style=flat&label=Release)](https://github.com/zyjGraphein/Obsidian-EagleBridge/releases/latest)

## 初始设置

1. 配置 Eagle 库
    - 打开插件设置，点击 **+** 添加库
    - 在 **Library Paths** 中填写 Eagle 库的绝对路径（以 `.library` 结尾的文件夹）
    - 同一库可添加多个路径（用于不同设备盘符差异）
    - 点击库名右侧的对勾图标设为当前库
2. 配置监听端口
    - 默认端口为 `6060`；确保未与其他服务冲突
    - 配置完成后建议重启 Obsidian 以确保本地服务正确启动

## 核心功能

### 搜索并插入 Eagle 资产

- 通过命令面板运行 `Insert Image From Eagle`（建议设置快捷键）
- 支持多关键词（空格分隔）检索
- 支持文件夹过滤与键盘上下选择、回车插入

### 上传本地文件到 Eagle

- 将本地图片拖入或粘贴到 Obsidian，自动上传至 Eagle 并生成可预览链接
- 可在设置中指定上传目标 **Folder ID**

### 反向同步

- 开启 `Reverse Sync on Open` 后，打开笔记时自动将 Eagle 链接标题更新为实际文件名
- 也可手动运行 `Reverse Sync Eagle Links in Current File` 命令

## 文件夹相关设置

- Incoming Target Folder（上传目标文件夹）：指定从 Obsidian 上传到 Eagle 的默认目标文件夹 ID
- Project Folder Roots（项目根）：用于资产移动管理；其子文件夹将出现在“管理文件夹”菜单中，便于快速归类
- Folder Filters（过滤器）：为“插入资产”窗口提供顶部过滤标签；点击后仅搜索该文件夹（及其子文件夹）

## 安装

### 通过 BRAT 安装

- 在 [BRAT](https://github.com/TfTHacker/obsidian42-brat) 中添加：`https://github.com/zyjGraphein/Obsidian-Eagle-Image-Organizer`

### 手动安装

- 前往最新 Release 页面，下载 `main.js`、`manifest.json`、`style.css`，放置到 `<your_vault>/.obsidian/plugins/Obsidian-Eagle-Image-Organizer/`

## 使用说明

- 图文教程（[中文](doc/TutorialZH.md) / [English](doc/Tutorial.md)）
- 视频教程（[Obsidian EagleBridge - bilibili](https://www.bilibili.com/video/BV1voQsYaE5W/?share_source=copy_web&vd_source=491bedf306ddb53a3baa114332c02b93)）

## 最新 Eagle 实时链接（Latest Eagle URL）

本插件会在本地库 images 目录发生新建时，生成一个“最新链接”，其他插件或用户可通过以下途径获取：

- 命令（适合用户）
    - Copy Latest Eagle URL：复制当前最新链接到剪贴板
    - Insert Latest Eagle URL：在光标处插入当前最新链接文本
- 插件 API（适合开发者）
    - const p = app.plugins.getPlugin('eagle-image-organizer')
    - const url = p?.api?.getLatestEagleUrl()
    - const path = p?.api?.getActiveLibraryPath() // 获取当前激活的库路径
    - const off = p?.api?.onEagleUrlUpdated((url) => { /_ 使用 url _/ })
- HTTP 接口
    - GET `http://localhost:<端口>/latest`
    - 返回：`{ "url": "http://localhost:<端口>/images/<id>.info" }`
    - GET `http://localhost:<端口>/libraryPath`
    - 返回：`{ "path": "E:\\Eagle\\MyLibrary.library" }`

## 注意事项

- 使用插件时请确保 Eagle 在后台运行
- 若 Eagle 未运行，图片预览、上传与管理功能将不可用
- 导出 PDF 可显示图片，但视频/音频等动态链接在本地环境之外不可访问

## 致谢

- 基于 [Eagle API](https://api.eagle.cool)
- 菜单/缩放参考 [AttachFlow](https://github.com/Yaozhuwa/AttachFlow)，嵌入预览参考 [auto-embed](https://github.com/GnoxNahte/obsidian-auto-embed)

## 许可

[GNU GPL v3](LICENSE)

## 支持

如果你喜欢这个插件，可以请我喝杯咖啡！

## 更新日志

### 0.4.0

性能优化专项版本。对启动、渲染、操作三条热路径全部做了优化，无任何对外行为变化（接口与嵌入逻辑保持不变），纯加速。

| # | 优化点 | 优化前 | 优化后 |
|---|--------|--------|--------|
| C1 | 启动写盘 | 每次启动都无条件执行 `updateLibraryPath()` → `saveSettings()` 写盘 | 仅在库路径配置真正变更时才写盘 |
| C2 | 启动阻塞 | `onload` 内同步建立 HTTP 服务 + 文件监听，阻塞 UI 就绪 | 延迟到 `onLayoutReady` 后再启动；库路径为空时跳过建立监听 |
| C3 | 渲染热路径 | 每个图片/链接节点每次渲染、每次按键都做两次 `new URL()` + 正则；`shouldEmbed` 无缓存 | 改用预编译正则；`shouldEmbed` 按 `src` 缓存结果；删除冗余 `isURL` 判定 |
| C4 | 逐部件定时器 | 每个 `EmbedWidget` 各起一个 `setTimeout(0)` 隐藏源 `<img>` | 改为 `requestAnimationFrame` 在一帧内批量隐藏 |
| C5 | 正则重编译 + 重复联网 | `reverseSync` 逐行重编译正则；`fetchImageInfo` 带 `&t=now` 每次都联网 | 正则提到循环外只编译一次；`fetchImageInfo` 按 id 内存缓存、去掉防缓存时间戳 |
| C6 | 设置面板写盘 | 文本输入每按键一次就 `saveSettings()` 写盘 | 改为 300ms 防抖保存（路径类输入额外同步库路径） |
| C7 | 点击处理器 | 两个独立的 document 级 capture `click` 处理器 | 合并为单个处理器，链接判定前廉价早退 |
| C8 | 插入弹窗拉取 | 每次按键都重新向 Eagle 拉取全量结果再前端过滤 | 按「查询 + 已选文件夹」缓存，相同查询直接复用 |

### 0.3.9

- 改进：重构为模块化类系统，提升性能与可维护性。
- 改进：通过批处理编辑器更新优化反向同步性能。
- 改进：优化事件监听器，降低 CPU 占用。
- 功能：新增获取当前激活 Eagle 库路径的 API (`getActiveLibraryPath`)。
- 功能：新增 HTTP 接口 `GET /libraryPath` 以获取当前激活库路径。

### 0.3.8

- 修复：修复了新版本中 Notice 提示无法显示的问题
- 改进：优化了 i18n 语言检测逻辑
- 改进：命令面板中的命令名称现已支持中英文切换
- 修复：增强了粘贴和拖拽功能的错误处理稳定性

<img src="assets/coffee.png" width="400">
