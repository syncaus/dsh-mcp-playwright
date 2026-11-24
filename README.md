# @local/dsh-mcp-playwright

把 [Playwright MCP](https://github.com/microsoft/playwright-mcp) 服务接进 DeepSeek Harness（DSH）的 profile，让会话能开浏览器、截图、做端到端断言。只有一份配置，没有插件代码。

## 需要先准备

1. Node.js（`node` 在 PATH 里）。
2. 全局安装 `@playwright/mcp`：

```sh
npm install -g @playwright/mcp
```

## 安装

装进 DSH 的 profile，任选一种：

```sh
# 从插件索引（插件列表里选「Playwright 浏览器」→「安装」）
# 索引源：https://cdn.jsdelivr.net/gh/syncaus/dsh-longtask-relay@main/market/index.json

# 或从这个仓库直接装
dsh plugin --profile <profile 名> add git+https://github.com/syncaus/dsh-mcp-playwright.git
```

## 安装后要改一处路径

`cordis.patch.yml` 里写的是打包机器上的绝对路径（`node` 可执行文件和全局安装的 `@playwright/mcp` 入口）。装好后把它改成**你自己机器**上的路径，否则启动 MCP 服务会失败：

```yaml
command: C:\Program Files\nodejs\node.exe        # 改成你的 node
args:
  - C:\Users\<you>\AppData\Roaming\npm\node_modules\@playwright\mcp\cli.js   # 改成你的 cli.js
```

改完重载插件即可。

## 装好之后

会话里就能用 Playwright 的浏览器工具（打开页面、点击、填表、截图、断言）。适合在改完前端后自己跑一遍页面，或在长任务里做端到端验收。

## 边界

- 这个包不含任何 JS 逻辑，只是把 MCP 服务声明成 bundle；MCP 服务本身由 `@playwright/mcp` 提供，装不上就是上面那两处路径或 Node 版本的问题。
- 浏览器是真实启动的进程，第一次跑可能需要下载 Chromium（`npx playwright install chromium`）。

## 许可

[Apache License 2.0](LICENSE)
