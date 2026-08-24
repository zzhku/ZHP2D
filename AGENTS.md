# ZHP2D_MULTI_LINK · 资源导航 + 更新日志 — AI 项目说明（AGENTS.md）

本仓库是 ZHP2D 的**网页资源导航**，并承载**面向用户的更新日志**（固件 + Web 控制台双频道）。

## 文件

| 文件 | 作用 |
|---|---|
| `index.html` | 资源导航聚合页（操作说明 / 上位机 / WebApp / 更新日志等入口，卡片式 + 星座粒子背景） |
| `changelog.html` | 更新日志页面（只读渲染层：固件 / Web 控制台 tab 切换 + 版本倒序卡片 + 三分类区块 + 锚点导航 + 粒子背景） |
| `changelog.js` | ★ 固件更新日志数据源：`const CHANGELOG = [...]`，最新版本在数组头部 |
| `changelog-web.js` | ★ Web 控制台更新日志数据源（展示副本）：`const CHANGELOG_WEB = [...]`，权威源是 webapp 仓库的 `CHANGELOG.md` |

## 更新日志维护约定（改 changelog*.js 前必读）

- 固件频道的写作规范以固件仓库 **`../ZHP2D/CHANGELOG_SPEC.md`** 为权威：
  三分类 feature=新增 / change=改动 / fix=修复；每条目必须经
  `git diff -w` 与 `git show HEAD:<file>` 取证，预存在功能不得写成新增。
- 每次固件发布后，在 `CHANGELOG` 数组【头部】追加新版本（最新在上）。
- Web 控制台频道：权威源是 webapp 仓库的 `CHANGELOG.md`；**一个发布周期 = 一条记录**
  （未发布到正式仓库 origin/main 的修改合并为一条「未发布测试版」）。每条记录带隐藏
  `hash`（完整 40 位，记录创建时的 HEAD），用于分隔版本：
  `git merge-base --is-ancestor <hash> origin/main` 为真 → 转正式版（version=发布日期）另起新记录；
  为假 → 继续累计进本条。由 Agent 同步到 `CHANGELOG_WEB`【头部】。
- 注意：修复的 BUG 若是本次更新（本发布周期）才引入的，无需显示（用户从未遇到过）。
- 面向用户语言：不出现寄存器名/函数名等技术名词。
- sections 字段：`{type, title, items[]}` 或 `{type, title, groups:[{name, items[]}]}`；
  type 对应 `changelog.html` 的 emoji（feature ✨ / change 🔧 / fix 🐛）。

## 本仓库约束

- 纯静态零依赖：不引入构建系统/CDN/框架；改动只在 `index.html` / `changelog.html` / `changelog.js` / `changelog-web.js`
- 中文 commit message；更新日志若关联固件改动，在固件提交 message 中互相标注
