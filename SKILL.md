---
name: grok-image
description: >-
  Starts the single local Grok Imagine server and generates or edits images
  and videos through it. Use when the user asks to generate, edit, or restyle
  an image, make a video, or mentions grok-image, grok-image-mcp, Grok Imagine,
  生图, 出图, 改图, or 视频生成.
---

# Grok Image

本仓库是唯一的 Grok 生图服务，供所有本地 agent 共用。先确定本 `SKILL.md` 所在目录，记为 `ROOT`，再运行 `$ROOT/bin/grok-image`。不要另外启动 `grok-image-mcp`，也不要在 Claude Code、Antigravity、Cursor 或其他工具里把它配成各自的 stdio MCP。

## 服务

- 命令：`$ROOT/bin/grok-image`
- 地址：`http://127.0.0.1:9371/mcp`（只监听本机）
- 图片和视频目录：`$ROOT/output`
- 日志和进程号：`$ROOT/var`
- 默认不运行。需要生图时如果没在跑，再启动。`start` 是幂等的，已经在跑就继续用这一个。
- 只有用户明确要求停止时才执行 `stop`。

## 步骤

1. 运行 `$ROOT/bin/grok-image status`。
2. `running` 不是 `true` 时，运行 `$ROOT/bin/grok-image start`。
3. 不确定工具名或参数时，运行 `$ROOT/bin/grok-image tools`。
4. 调用工具，参数是 JSON 对象：

```bash
"$ROOT/bin/grok-image" call generate_image <<'EOF'
{"prompt":"...","aspectRatio":"16:9","resolution":"1k"}
EOF
```

把命令输出里的本地文件路径交给用户。工具没有输出路径参数，文件写到 `$ROOT/output`。

当前 agent 能连接 HTTP MCP 时，服务启动后连接 `http://127.0.0.1:9371/mcp`。不能连接时用上面的 `call` 命令。

常用工具：`generate_image`、`edit_image`（要 `imagePath`）、`continue_editing`、`style_transfer`、`generate_video`、`image_to_video`、`get_configuration_status`。

`continue_editing` 使用这个共享进程里的上一张图，不区分来自哪个 agent。要改某一张已有文件时用 `edit_image`。

## 配置

凭证使用运行这台机器上的 `~/.grok/auth.json`，或者环境变量 `XAI_API_KEY`。认证方式和模型写在 `bin/grok-image` 的 `MCP_ENV` 里。
