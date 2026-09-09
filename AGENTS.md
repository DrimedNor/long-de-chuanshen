# AGENTS.md · 龙的传人网站（ZCode 工作区指令）

> 全局原则见 `C:\Users\Drime\.zcode\AGENTS.md`（总持 × 四肢）。本文件是项目级细化。

## 项目快照

- 自研零依赖 Python 构建器 `build_site.py`（约 5600 行），**无 package.json/node_modules**
- 产物：`dist/index.html`（预渲染）+ `dist/pages/*.json`（按需）+ `knowledge.json`（AI 搜索语料）
- 后端：Cloudflare Workers（`stats-auth-worker.js` 统计/密码/注册遮罩；`ai-ask-worker.js` AI 问答代理）+ KV
- 托管：Cloudflare Pages，正式域名 longchen-nyingtik.wiki；**发布＝本地构建 + `npx wrangler pages deploy dist --branch=main`，git push 不触发部署**。⚠️ **生产分支是 main 而非 v5**：不带 `--branch=main` 会部署成 Preview（生产域名不更新）；git 仓库里并没有 main 分支，`--branch=main` 只是部署元数据（2026-09-09 实测踩坑，WorkBuddy）
- git：工作分支 v5；每次发布必推 GitHub（`git@github.com:DrimedNor/longchen-nyingtik.git`）
- 统计：GoatCounter + 自建设备统计（10 台密码 610 / 100 台注册审核）

## 必读规范（总持，开工前先读）

位置：`D:\Users\Drime\Documents\Obsidian\龙的传人（网站建设）\规范与盘点\`

1. **项目设计原则与规范.md**——重点：第二章法律合规（不传教不募捐、密码 610、10/100 台阶梯）、第十一章**已确认的固定修改清单（不可随意改动）**
2. **配色方案_藏红主题.md / 藏传佛教主题.md**——颜色只用现有 CSS 变量（--bg/--surface/--ink/--accent 等 12 token）
3. 历史外包任务书通用约束（AI协作\任务书归档\外包任务书-给其他AI.md）：只改 `build_site.py`、动效只用 transform/opacity、交付写「位置+新旧对比+自测清单」

## 硬约束

1. **content/ 是发布源**：`is_excluded_dir()`（build_site.py:252）只排除目录名含「不推送」、`.` 开头、`_backup`、assets——**新文件进 content/ 前必须确认允许上线；私人日志只进 `日志-不推送\`**
2. 不动 `.workbuddy\`；密钥/密码不入库不入 git（密码 610 除外，它是合规设计的站内密码）
3. 操作前备份（规范第七章维护规则）；`工作进度看板.md` 不入 git
4. **音频分类必须镜像「上师开示」**：`content/音频资源/1. 上师开示（AI朗读）/` 的文件夹结构与 `content/上师开示/` 严格一致，音频跟着文章走（规范 6.2）。**每次发布前先跑 `python check_audio_taxonomy.py`，不通过不许发布**（规范 6.4）

## 工作流

1. **接任务**：读总持 `AI协作\任务包\` 中状态为「待开工」的任务包 → 按 PRD 实施
2. **实施**：改代码 → 按任务包《检查清单.md》自测（构建通过、移动端、暗色模式、音频播放）
3. **收尾**：调用技能 `brain-deposit`（日志→`content\日志-不推送\`，交接簿→AI协作日志.md，状态.md 结案）
4. **发布**：调用技能 `longchen-publish`（含发布前计划外文件检查）

## 协作路径速查

| 内容 | 路径 |
| --- | --- |
| 任务包 | `…\Obsidian\龙的传人（网站建设）\AI协作\任务包\` |
| 交接簿 | `…\AI协作\AI协作日志.md`（先读后写/只追加/最新在上） |
| 日报 | `…\日报\`（WorkBuddy 自动产出） |
| 素材池 | `…\整理输出\`（上师开示上游，网站 content 的候选内容） |
| 工作进度看板 | 本仓库根 `工作进度看板.md` |
