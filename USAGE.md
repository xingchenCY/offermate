# 使用说明

## 本地预览

环境要求：

- Node.js 18 或更高版本
- npm 或 pnpm
- Rust stable
- Tauri 2.0 对应的系统依赖

安装依赖并启动前端预览：

```bash
npm install
npm run dev
```

启动 Tauri 桌面应用：

```bash
npm run tauri dev
```

## 构建

```bash
npm run build
npm run tauri build
```

## 浏览器扩展

1. 打开 Chrome 或 Edge 的扩展管理页。
2. 开启开发者模式。
3. 选择“加载已解压的扩展程序”。
4. 选择项目中的 `extension/` 目录。
5. 启动 OfferMate 桌面应用后，在招聘网站岗位详情页点击扩展图标。

扩展支持将识别到的岗位信息通过本地回环接口导入桌面应用。未启动桌面应用时，岗位会暂存到扩展队列中。
