[English](./dsh_session_export.md) | [简体中文](./dsh_session_export.zh-CN.md) · [← 返回](../README.zh-CN.md)

# 接入 dsh-session-export

dsh-session-export 是一款面向 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（DSH，DeepSeek 官方 Agent 运行时）的开源插件。它新增 `/transcript` 命令，把已结束或正在运行的会话导出为人类可读的转录，直接写入 Host 文件系统路径。官方自带导出工具走浏览器下载原始日志 ZIP，而本插件渲染的是你真正读到的内容：完整消息流、带编辑器 diff 的工具调用、子代理谱系与 token 汇总 —— 以 Markdown 和/或 JSON 输出，兼容 `ctx.sessionQuery` 之后的任意持久化后端（JSONL、SQLite……）。

#### 1. 前置条件：DeepSeek Harness 环境

本插件是 DSH 的树外插件，需要已有的 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 环境。无需额外申请 API Key 或配置模型 —— 它通过 DSH 自带的会话查询引擎读取数据，沿用你的 Harness 已配置的模型提供方。

#### 2. 安装到 profile

```
dsh plugin --profile web add dsh-session-export
```

包内声明了 `dsh.bundle`，安装后会自动加入 profile 的插件层栈 —— 无需手动改组合配置。安装后重启 `dsh web` 进程，让新层生效。

#### 3. 在会话中运行 `/transcript`

```
/transcript
```

该命令运行在 DSH 的人类命令平面上 —— 零 token 消耗，结果不会进入模型历史 —— 并在 `<会话工作目录>/dsh-transcripts/` 下生成 `transcript-<session>-<timestamp>.md`。

常用变体：

- `/transcript --json` —— 同时输出机器可读的 JSON 文档
- `/transcript --full` —— 附上 log-only 事件附录（命令生命周期、压缩标记）
- `/transcript --out <path>` —— 自定义输出路径
- `/transcript --id <sessionId>` —— 导出其他会话
