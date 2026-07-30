# MARVIN SDK 文档导航网站维护说明

## 1. 文档目的

本说明用于维护 MARVIN SDK 文档导航网站：

- 网站地址：<https://hencaideyu.github.io/marvin-sdk-docs/>
- 网站仓库：<https://github.com/hencaideyu/marvin-sdk-docs>
- 原始 SDK 仓库：<https://github.com/cynthia-you/TJ_FX_ROBOT_CONTRL_SDK>

网站的定位是“使用说明导航器”。客户先按任务选择入口，再跳转到原始 SDK
仓库中对应的 README、接口文档、Demo 或 Release 页面。

网站不负责复制和维护完整 SDK 文档。SDK 的技术内容应继续以原始仓库为准。

---

## 2. 技术说明

本网站不是 Java 项目，主要使用：

- React：页面组件和交互
- TypeScript：页面内容与跳转配置
- CSS：页面布局和视觉样式
- Vite：生成可部署到 GitHub Pages 的静态文件

源代码与部署文件是两套不同的文件：

| 类型 | 用途 | 是否适合直接修改 |
| --- | --- | --- |
| `app/page.tsx` | 页面文字、分类、按钮和链接 | 是 |
| `app/globals.css` | 页面颜色、字号、间距和响应式布局 | 是 |
| `app/layout.tsx` | 网站标题、简介和分享信息 | 是 |
| `pages-src/index.html` | GitHub Pages 页面标题和描述 | 必要时修改 |
| `assets/index-*.js` | 构建后生成的压缩程序 | 否 |
| `assets/index-*.css` | 构建后生成的压缩样式 | 否 |
| `index.html` | GitHub Pages 的入口文件 | 由构建过程生成 |

不要直接修改 `assets/index-*.js`。这类文件是构建后的成品，文件名每次构建都可能改变。

---

## 3. 内容维护原则

### 3.1 原始 SDK 仓库负责什么

研发人员继续在原始 SDK 仓库维护：

- 根目录 `README.md`
- Python/C++ 控制 SDK 文档
- Python/C++ 运动学 SDK 文档
- Demo 代码及说明
- 控制器和 SDK 版本记录
- Releases 下载内容
- 错误码、参数和安全说明

### 3.2 导航网站负责什么

导航网站只维护：

- 客户任务分类
- 入口名称和简短说明
- 指向 GitHub 的准确链接
- 快速上手内容
- 页面搜索关键词
- 手机和电脑端阅读体验

### 3.3 什么情况下不需要更新网站

如果研发只修改了某份 Markdown 文档的正文，而且文件名和章节标题没有变化，
网站通常不需要更新。客户点击网站后会自动看到 GitHub 上的最新内容。

### 3.4 什么情况下必须更新网站

出现以下情况时需要更新网站：

- Markdown 文件改名或移动
- README 章节标题改变
- GitHub 分支名改变
- 新增 SDK、Demo、控制模式或下载入口
- 删除了网站正在使用的文件或章节
- 修改网站上的分类、说明或视觉样式

---

## 4. 最常见的维护操作

### 4.1 修改卡片标题或说明

打开 `app/page.tsx`，搜索页面上显示的原文字。

例如：

```tsx
{
  tag: "FIX",
  title: "连接失败或机器人报错",
  description: "订阅为 0、状态 100、急停与错误码",
  href: `${repositoryUrl}/blob/master/README.md#七主要问题和解决`,
}
```

只修改客户看到的标题：

```tsx
title: "机器人出现报错怎么办？",
```

### 4.2 修改 GitHub 跳转链接

假设 README 的“七、主要问题和解决”改名为“八、故障排查手册”，需要把：

```tsx
href: `${repositoryUrl}/blob/master/README.md#七主要问题和解决`,
```

改成：

```tsx
href: `${repositoryUrl}/blob/master/README.md#八故障排查手册`,
```

修改后必须在浏览器中测试卡片是否能打开正确章节。

### 4.3 增加一个新的导航入口

在 `app/page.tsx` 的 `guideLinks` 列表中增加一项：

```tsx
{
  tag: "ROS2",
  title: "ROS2 集成说明",
  description: "连接、订阅和控制节点示例",
  note: "GitHub · ROS2 文档",
  href: `${repositoryUrl}/blob/master/ROS2_README.md`,
},
```

同时建议在 `searchEntries` 中增加对应搜索项。

### 4.4 修改颜色、间距或移动端布局

页面样式位于 `app/globals.css`。

修改前建议先记录原值，不要直接删除大段样式。重点检查：

- 电脑端导航栏
- 手机端单列布局
- 中文字号和行距
- 卡片悬停效果
- 代码块是否横向溢出

---

## 5. 本地生成 GitHub Pages 文件

在网站源代码目录执行：

```bash
npm install
npm run build
npx vite build --config vite.pages.config.ts
```

构建完成后，GitHub Pages 文件位于：

```text
github-pages-package/
```

需要确认该目录至少包含：

```text
github-pages-package/
├── index.html
├── 404.html
├── og.png
├── favicon.svg
└── assets/
    ├── index-*.js
    └── index-*.css
```

每次构建后 `assets` 中的文件名可能变化，这是正常现象。

必须上传整个构建结果，不能只上传 `index.html`。

---

## 6. 发布到 GitHub Pages

1. 打开网站仓库：<https://github.com/hencaideyu/marvin-sdk-docs>
2. 点击 **Add file → Upload files**。
3. 打开本地 `github-pages-package` 文件夹。
4. 上传该文件夹里面的全部内容，不要把最外层文件夹作为子目录上传。
5. 允许覆盖 `index.html` 和 `404.html`。
6. 确认新的 `assets/index-*.js` 和 `assets/index-*.css` 已上传。
7. 填写清晰的提交说明。
8. 点击 **Commit changes**。
9. 等待仓库右侧 `github-pages` 部署状态变成绿色。
10. 打开网站并强制刷新。

建议的提交说明：

```text
v1.1 更新故障排查和运动学入口
```

GitHub Pages 设置应保持：

```text
Source: Deploy from a branch
Branch: main
Folder: /(root)
```

---

## 7. 发布后的检查清单

每次发布后至少完成以下检查：

- [ ] 网站首页可以正常打开
- [ ] 左侧“按需求找说明”可以定位到导航区
- [ ] Python/C++ 切换正常
- [ ] 安装与编译入口跳转正确
- [ ] Python 控制 SDK 入口跳转正确
- [ ] C++ 控制 SDK 入口跳转正确
- [ ] 控制模式入口跳转正确
- [ ] 运动学与轨迹规划入口跳转正确
- [ ] Python/C++ Demo 入口跳转正确
- [ ] 数据订阅入口跳转正确
- [ ] 故障排查入口跳转正确
- [ ] Releases 入口跳转正确
- [ ] 手机 Safari/Chrome 可以打开
- [ ] GitHub 页面没有显示 404

推荐使用一个不涉及真实机器人运动的测试：

> 客户遇到“连接成功但订阅数据全部为 0”，从网站点击“连接失败或机器人报错”，
> 应直接进入 GitHub 的“主要问题和解决”章节。

---

## 8. 旧版本与回退

### 8.1 查看旧代码

进入网站仓库，点击 **Commits**，可以查看每次更新前后的文件。

### 8.2 恢复旧版本

如果新版出现严重问题：

1. 找到最近一次正常的提交。
2. 恢复该提交中的网站文件。
3. 提交恢复操作。
4. 等待 GitHub Pages 重新部署。

不建议直接删除整个仓库或修改 Pages 发布分支。

### 8.3 长期保留旧版

如需让客户同时访问新旧版本，可以保留：

```text
新版：https://hencaideyu.github.io/marvin-sdk-docs/
旧版：https://hencaideyu.github.io/marvin-sdk-docs/classic/
```

旧版应放在独立目录中，不要依赖浏览器缓存。

---

## 9. 常见问题

### 网站显示的还是旧内容

处理顺序：

1. 检查 GitHub 仓库的 `index.html` 是否已经更新。
2. 检查右侧 `github-pages` 是否为绿色。
3. 等待 1～10 分钟。
4. 强制刷新浏览器。
5. 使用无痕窗口重新打开网站。

### 网站显示空白

重点检查：

- `index.html` 引用的 JS/CSS 文件是否存在
- 是否上传了完整 `assets` 文件夹
- 仓库名是否仍然是 `marvin-sdk-docs`
- `vite.pages.config.ts` 中的 `base` 是否为 `/marvin-sdk-docs/`

### 页面可以打开，但按钮跳转到 404

通常是原始 SDK 仓库中的文件名、目录或 README 标题发生了变化。
在 GitHub 中找到新的准确地址，然后更新 `app/page.tsx` 中对应的 `href`。

### 手机打不开

确认使用的是 GitHub Pages 地址：

<https://hencaideyu.github.io/marvin-sdk-docs/>

不要给客户发送 GitHub 仓库编辑页面或旧的 `chatgpt.site` 地址。

---

## 10. 安全与发布注意事项

- 不要在网站代码中保存机器人密码、密钥、Token 或客户数据。
- 不要把内部网络地址、未公开协议或客户现场信息发布到公共仓库。
- Demo 必须明确标注为测试示例，不能直接作为生产程序运行。
- 修改速度、刚度、阻尼、力控范围等参数说明前，应由机器人研发人员复核。
- 删除或修改安全警告前必须经过技术负责人确认。

---

## 11. 推荐的长期改进

当前方式是手动生成压缩包并上传，适合低频维护。

长期建议：

1. 把完整网站源代码保存到 GitHub。
2. 使用 GitHub Actions 自动构建和部署。
3. 把任务导航配置独立成一个容易修改的数据文件。
4. 为所有外部链接增加自动有效性检查。
5. 每次原始 SDK 仓库发布新版本时自动检查文档路径。

完成自动化后，维护流程可简化为：

```text
修改文字或链接
       ↓
提交到 GitHub
       ↓
自动构建
       ↓
自动发布
```

---

## 12. 维护职责建议

| 角色 | 主要职责 |
| --- | --- |
| 机器人研发 | 确认接口、参数、错误码和安全说明 |
| 文档维护人员 | 更新 Markdown、标题、目录和 Demo 说明 |
| 网站维护人员 | 更新导航卡片、链接、搜索词和页面样式 |
| 发布负责人 | 上传构建文件、检查部署状态并完成发布测试 |

建议每次 SDK 版本发布时进行一次网站链接检查；没有版本变化时，每季度检查一次即可。
