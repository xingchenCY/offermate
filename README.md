# OfferMate 桌面版

> 本地优先的求职管理桌面应用 —— 让你的每一次投递都心中有数。

OfferMate 是一款基于 Tauri 2.0 构建的求职管理桌面应用。所有数据存储在本地，不上传任何服务器，帮助你高效管理投递进度、简历版本与岗位信息。配套浏览器扩展可一键抓取主流招聘网站的岗位详情，告别手动复制粘贴。

---

## 功能特性

| 模块 | 说明 |
| --- | --- |
| 仪表盘 | 求职全貌一览：投递总数、各状态分布、近 30 天趋势、待办提醒 |
| 投递追踪 | 看板式管理投递状态（待投递 / 已投递 / 笔试 / 面试 / Offer / 不合适），支持拖拽流转、时间线记录 |
| 简历管理 | 多版本简历管理，按岗位方向维护不同简历，支持快速预览与导出 |
| AI 助手 | 智能生成求职信、面试复盘建议、岗位匹配度分析（可接入本地或云端模型） |
| 岗位聚合 | 统一收藏来自不同招聘平台的岗位，去重归档，支持标签与备注 |
| 浏览器扩展 | Chrome / Edge 扩展一键抓取 Boss 直聘、智联、牛客、拉勾、51job、猎聘的岗位信息并导入 |
| 求职笔记 | 按公司 / 岗位组织面试笔记、技术问答、复盘总结，支持 Markdown |

---

## 技术栈

| 层级 | 技术 |
| --- | --- |
| 桌面框架 | [Tauri 2.0](https://tauri.app/) (Rust + WebView) |
| 前端框架 | React 18 + TypeScript |
| 构建工具 | Vite 6 |
| 样式方案 | Tailwind CSS 3 + tailwindcss-animate |
| UI 组件 | shadcn/ui (基于 Radix UI) |
| 图标 | Lucide React + Phosphor Icons |
| 状态管理 | Zustand |
| 路由 | React Router 7 |
| 图表 | Recharts |
| 动画 | Motion (Framer Motion) |
| 数据库 | SQLite (tauri-plugin-sql) |
| 本地存储 | tauri-plugin-store |
| 通知 | tauri-plugin-notification |
| 浏览器扩展 | 原生 JavaScript + Manifest V3 |

---

## 快速开始

### 环境要求

- [Node.js](https://nodejs.org/) >= 18
- [pnpm](https://pnpm.io/)（推荐）或 npm
- [Rust](https://www.rust-lang.org/tools/install) (stable)
- Tauri 2.0 系统依赖：参考 [Tauri 官方前置要求](https://tauri.app/start/prerequisites/)

### 安装依赖

```bash
pnpm install
```

### 开发模式

启动前端开发服务器与 Tauri 桌面窗口：

```bash
pnpm tauri dev
```

单独运行前端（仅 Web 预览，无 Tauri 能力）：

```bash
pnpm dev
```

### 构建生产版本

```bash
pnpm tauri build
```

构建产物位于 `src-tauri/target/release/bundle/` 下。

---

## 项目结构

```
offermate-desktop/
├── extension/                      # 浏览器扩展（Chrome / Edge MV3）
│   ├── manifest.json               # 扩展配置
│   ├── content.js                  # 内容脚本：多招聘网站岗位抓取
│   ├── background.js               # Service Worker：导入队列与通知
│   ├── popup.html                  # 扩展弹窗 UI（内联 CSS）
│   ├── popup.js                    # 弹窗逻辑
│   └── icons/
│       └── icon.svg                # 扩展图标
├── src/                            # React 前端源码
│   ├── components/                 # UI 组件（shadcn/ui）
│   ├── pages/                      # 页面（仪表盘、投递、简历等）
│   ├── stores/                     # Zustand 状态
│   ├── lib/                        # 工具函数
│   └── App.tsx
├── src-tauri/                      # Rust 后端
│   ├── capabilities/
│   │   └── default.json            # Tauri 2.0 权限配置
│   ├── src/
│   │   ├── commands/
│   │   │   └── server.rs           # HTTP 导入服务 (localhost:9420)
│   │   ├── lib.rs
│   │   └── main.rs
│   ├── Cargo.toml
│   └── tauri.conf.json
├── index.html
├── package.json
├── tailwind.config.ts
├── tsconfig.json
└── vite.config.ts
```

---

## 浏览器扩展

OfferMate 浏览器扩展支持从以下招聘网站一键抓取岗位信息并导入桌面应用：

- Boss 直聘 (zhipin.com)
- 智联招聘 (zhaopin.com)
- 牛客网 (nowcoder.com)
- 拉勾 (lagou.com)
- 前程无忧 (51job.com)
- 猎聘 (liepin.com)

### 安装方式（开发者模式）

1. 打开 Chrome / Edge，进入扩展管理页：
   - Chrome：地址栏输入 `chrome://extensions`
   - Edge：地址栏输入 `edge://extensions`
2. 打开右上角「开发者模式」。
3. 点击「加载已解压的扩展程序」，选择项目下的 `extension/` 目录。
4. 扩展图标出现在工具栏，即可使用。

### 使用方法

1. 启动 OfferMate 桌面应用（扩展需要连接 `localhost:9420`）。
2. 在支持的招聘网站打开岗位详情页。
3. 点击工具栏的 OfferMate 图标，弹窗会自动预览识别到的岗位信息。
4. 点击「导入到 OfferMate」即可将岗位保存到桌面应用。

> 若 OfferMate 未运行，岗位会自动加入待导入队列，下次启动应用后点击同步即可导入。

### 扩展与桌面应用的通信

```
浏览器扩展 content.js  ──抓取岗位──▶  background.js
                                          │
                                   POST /api/import  (localhost:9420)
                                          ▼
                              OfferMate 桌面应用 (server.rs)
                                          │
                                   写入 SQLite 数据库
                                          │
                                   触发前端事件刷新
```

---

## 设计系统

### 配色

| Token | 色值 | 用途 |
| --- | --- | --- |
| Primary (Indigo) | `#4F46E5` | 主色：按钮、链接、强调 |
| Primary Hover | `#4338CA` | 主色悬停态 |
| Success | `#10B981` | 成功状态 |
| Warning | `#F59E0B` | 警告状态 |
| Destructive | `#EF4444` | 危险 / 薪资强调 |
| Background | `#FFFFFF` | 页面背景 |
| Foreground | `#1F2937` | 主文本 |
| Muted Foreground | `#6B7280` | 次要文本 |

通过 CSS 变量与 Tailwind 主题令牌统一管理，支持深色模式（`darkMode: "class"`）。

### 字体

- 正文：`Geist`, `SF Pro Text`, `PingFang SC`, `system-ui`
- 等宽：`JetBrains Mono`, `SF Mono`, `Menlo`

### 圆角与间距

- 统一圆角令牌：`--radius`（默认 8px），派生 `lg` / `md` / `sm`
- 间距基于 4px 网格，使用 Tailwind 默认 spacing scale

### 组件规范

- 基于 shadcn/ui，组件位于 `src/components/ui/`
- 使用 `class-variance-authority` 管理变体样式
- 交互反馈：hover 过渡 150ms，fade-in 动画 300ms

---

## 数据隐私

OfferMate 坚持本地优先（Local-First）原则：

- 所有求职数据存储在本地 SQLite 数据库，不经过任何云端服务器。
- 浏览器扩展与桌面应用仅通过 `localhost:9420` 本地回环通信，数据不离开你的设备。
- AI 助手功能可配置为使用本地模型；若使用云端模型，仅传输必要文本，不存储对话历史。
- 应用不包含任何埋点、分析或追踪代码。
- 你可随时在应用内导出 / 删除全部数据。

---

## 许可证

MIT License
