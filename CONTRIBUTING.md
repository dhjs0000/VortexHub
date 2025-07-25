## Git 规范（VortexHub 版 v1.0）

### 1 分支模型（Git-Flow Lite）
| 分支 | 用途 | 保护规则 |
|---|---|---|
| `main` | 随时可发布的稳定版本 | **强制 PR + CI 通过** |
| `dev` | 日常开发集成 | 允许直接 push，但每天 rebase 一次 |
| `feature/xxx` | 功能开发 | 从 `dev` 切，完成后 PR 回 `dev` |
| `hotfix/xxx` | 紧急修复线上 bug | 从 `main` 切，PR 同时合回 `main` 和 `dev` |

---

### 2 Commit 消息格式
```
<type>(<scope>): <subject>

<body>   // 可选，72 字符换行
<footer> // 可选：关联 issue、BREAKING CHANGE
```
- **type** 限以下 7 种  
  `feat / fix / docs / style / refactor / test / chore`  
- **scope** 用模块名：`gui`, `core`, `ci`, `build`  
- **subject** 用祈使句，50 字符以内，**不要句号**

示例  
```
feat(gui): 一键切换 Blender 版本  
fix(core): 解决路径含空格时启动失败  
docs(readme): 添加 Linux 安装说明
```

---

### 3 Pull Request 模板
```markdown
## 变更内容
- 简述改动

## 自检清单
- [ ] 代码通过 `flake8`/`black`
- [ ] 本地单元测试通过
- [ ] 在 Windows / macOS / Linux 实测通过
- [ ] 文档同步更新

## 关联 issue
Closes #123
```

---

### 4 版本号 & Tag
- 采用 **SemVer**：`v<major>.<minor>.<patch>`  
- 每次 `main` 合并即打 tag：
  ```
  git tag -a v1.3.0 -m "release: 支持自动检测安装路径"
  git push origin v1.3.0
  ```

---

### 5 忽略与属性
- `.gitignore` 必须包含：
  ```
  __pycache__/
  *.pyc
  /dist/
  /build/
  /venv/
  *.spec
  ```
- `.gitattributes` 统一换行：
  ```
  * text=auto eol=lf
  *.bat text eol=crlf
  ```

---

### 6 CI 底线（GitHub Actions 示例）
- 每次 push/PR 自动跑：
  - `black --check`
  - `flake8`
  - `pytest tests/`
- 失败即拒绝合并。

---

### 7 快速命令卡
```bash
# 开始新功能
git checkout dev
git pull --rebase
git checkout -b feature/auto-scan

# 完成开发
git push -u origin feature/auto-scan
# 打开网页发 PR，合并后删除分支
```

---

The New Generation Of Blender Hub.
