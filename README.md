# grok-image

本机共用的 Grok Imagine 服务。只起一个进程，在 `http://127.0.0.1:9371/mcp` 生图、改图和生成视频。

## 用法

需要 Python 3。

```bash
./bin/grok-image start
./bin/grok-image status
./bin/grok-image tools
./bin/grok-image call generate_image '{"prompt":"...","aspectRatio":"16:9","resolution":"1k"}'
./bin/grok-image stop
```

`start` 可以重复执行，服务已在运行时会继续用这一个。图片和视频写到 `output/`，日志和进程号在 `var/`。

常用工具：`generate_image`、`edit_image`、`continue_editing`、`style_transfer`、`generate_video`、`image_to_video`。

## 配置

凭证放在 `~/.grok/auth.json`，或设置环境变量 `XAI_API_KEY`。

不要单独启动 `bin/grok-image-mcp`，也不要把它配成各自的 stdio MCP。
