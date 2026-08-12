# 日照银行智慧消保平台投诉原型

本仓库主要维护一个静态 HTML 高保真原型，用于日照银行智慧消保平台投诉管理相关流程演示、评审和确认。当前不是完整前后端工程，核心原型直接由浏览器打开即可查看。

## 核心文件

| 文件 | 说明 |
| --- | --- |
| `日照银行_智慧消保平台_投诉原型.html` | 主原型文件，GitLab 镜像发布时同步这个文件 |
| `index.html` | GitHub/本地入口副本，日常修改后应与主原型完全一致 |
| `README.md` | 给后续对话和维护者看的项目说明 |

## 维护规则

1. 修改原型后，保持 `日照银行_智慧消保平台_投诉原型.html` 和 `index.html` 内容一致。
2. HTML 是预编译后的 React 产物，内部使用 `_jsx` / `_jsxs`，不要直接写 JSX。
3. 工作区长期存在一些无关删除和未跟踪文件，提交前必须确认暂存范围。
4. GitHub 主仓库通常提交 `日照银行_智慧消保平台_投诉原型.html`、`index.html`，以及用户明确要求维护的文档。
5. GitLab 镜像仓库只同步并提交 `日照银行_智慧消保平台_投诉原型.html`，不要把 `index.html` 或本 README 带过去。

## 最近功能状态

- 投诉管理左侧菜单中，`短信审核` 已归入 `申请处理`，同级新增 `延期审批`。
- 工单处理员在 `待处理`、`处理中` 工单上可发起延期申请。
- 延期申请弹窗显示 `原处理时限`、`延长时间：15个工作日`、`最新处理时限` 和 `申请延期理由`。
- 最新处理时限按原处理时限顺延 15 个工作日计算，跳过周六周日。
- 延期审批列表显示 `原处理时限`、`最新处理时限`、申请理由、同意/不同意操作；不同意时需填写理由，审批完成进入历史记录。
- 工单录入的投诉反映渠道已加入 `人民银行`，位置在 `其它` 之前。
- 统计分析 `渠道分析` 的办理质效表已删除 `渠道类型` 列，其它列保持不变。

## 常用校验

在 `D:\personal\trs` 下执行：

```powershell
git status --short
Get-FileHash -Algorithm SHA256 -LiteralPath "日照银行_智慧消保平台_投诉原型.html", "index.html"
```

两个 HTML 一致时，SHA256 应完全相同。不一致时通常以主原型同步 `index.html`：

```powershell
Copy-Item -LiteralPath "日照银行_智慧消保平台_投诉原型.html" -Destination "index.html"
```

脚本语法检查：

```powershell
$node = "C:\Users\20885\.cache\codex-runtimes\codex-primary-runtime\dependencies\node\bin\node.exe"
& $node -e "const fs=require('fs'); const html=fs.readFileSync('日照银行_智慧消保平台_投诉原型.html','utf8'); const matches=[...html.matchAll(/<script[^>]*>([\s\S]*?)<\/script>/g)]; if(!matches.length) throw new Error('script block not found'); new Function(matches.at(-1)[1]); console.log('syntax ok');"
```

## GitHub 提交流程

只暂存本次需要的文件，例如：

```powershell
git add -- "日照银行_智慧消保平台_投诉原型.html" "index.html" "README.md"
git diff --cached --name-only
git commit -m "feat(prototype): update Rizhao prototype"
git push origin main
```

## GitLab 发布流程

GitLab 镜像仓库：

```text
C:\Users\20885\.codex\visualizations\2026\07\21\019f839d-a3b8-7d30-bfae-b5aa69c26248\gitlab-single-html
```

远端：

```text
http://192.168.210.61/rzbank/rzbank-consumer-protection-platform-web.git
```

推荐流程：

```powershell
$gitlabRepo = "C:\Users\20885\.codex\visualizations\2026\07\21\019f839d-a3b8-7d30-bfae-b5aa69c26248\gitlab-single-html"
git -C $gitlabRepo fetch gitlab main
git -C $gitlabRepo switch --detach gitlab/main
Copy-Item -LiteralPath "D:\personal\trs\日照银行_智慧消保平台_投诉原型.html" -Destination "$gitlabRepo\日照银行_智慧消保平台_投诉原型.html"
git -C $gitlabRepo status --short
git -C $gitlabRepo add -- "日照银行_智慧消保平台_投诉原型.html"
git -C $gitlabRepo commit -m "feat(prototype): update Rizhao prototype HTML"
git -C $gitlabRepo push gitlab HEAD:main
```

如 push 被拒绝，先 `fetch gitlab main`，再把本地提交 rebase 到最新 `gitlab/main` 后推送。

## 工作区注意事项

当前主仓库可能存在以下无关变更。除非用户明确要求，不要提交或回滚：

- 删除：`后台管理分支_角色权限页面设计说明_AI版.md`
- 未跟踪：`generate_ai_resume.py`
- 未跟踪：`output/`
- 未跟踪：`switch-to-deepseek.ps1`
- 未跟踪：`switch-to-openai.ps1`
- 未跟踪：`侧边导航_象牙白三级菜单优化版.html`
- 未跟踪：`日照银行_UI设计规范总结.md`
- 未跟踪：`消审原型.html`
- 未跟踪：`统计分析.html`

## 给下一个对话

接手时先看本 README，再看 `git status --short`。改动完成后至少做三件事：两个 HTML 同步、脚本语法校验、SHA256 一致性检查。发布 GitLab 时只同步主原型 HTML 到镜像仓库。
