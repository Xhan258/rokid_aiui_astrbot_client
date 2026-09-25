# Rokid AIUI AstrBot Client

面向 Rokid 灵珠 / AIUI Studio 的通用 AstrBot 眼镜客户端。它负责 HUD、语音识别、TTS、相机和与 Bridge 的通信；不内置大模型、人格、长期记忆或第二套 Agent。

它需要配合 [AstrBot Rokid Bridge 插件](https://github.com/Xhan258/astrbot_plugin_rokid_bridge) 使用。

当前版本：`1.0.5`。需要 Bridge Plugin `>=0.3.5,<1.0.0`。

## 直接从 GitHub 导入

在 AIUI Studio 选择 **GitHub 导入**，填写本仓库地址并选择 `main` 分支即可。此仓库根目录就是 AIUI 项目根目录，不需要再填写子目录。

也可以使用 AIUI Studio 的本地导入，选择本项目根目录。

## 先改配置

第一次使用只需要修改 `serverUrl`，把 `http://YOUR_ASTRBOT_HOST:6191` 换成你的 Bridge 地址；其余配置保持默认即可。

编辑根目录 [`config.js`](config.js)：

```js
export const config = {
  serverUrl: 'http://YOUR_ASTRBOT_HOST:6191',
  assistantName: '我的助手',
  deviceDisplayName: '我的 Rokid',
  storagePrefix: 'my_rokid_client',
  ttsEnabled: true,
  ttsVoice: 'female-yujie',
  idleBlankSeconds: 15,
};
```

- `serverUrl`：运行 AstrBot Rokid Bridge 的地址；局域网中通常是 AstrBot 主机 IP 加端口 `6191`，不是 AstrBot WebUI 地址。
- `assistantName`：HUD 顶部显示名，不会修改 AstrBot 人格。
- `deviceDisplayName`：首次配对时、AstrBot 插件设备页中显示的名字。
- `storagePrefix`：本地设备 ID、凭证和 HUD 历史的存储前缀。一个眼镜连接多套 Bridge 时才需要改。
- `ttsEnabled`、`ttsVoice`：控制眼镜本地 TTS。
- `idleBlankSeconds`：已配对、没有录音且回复结束后，空闲多久隐藏 HUD、进入视觉黑屏待机，单位是秒。默认 `15`；改成 `0` 则关闭。TTS 开启时，客户端会为已入队的播报片段预留保守时长，再开始倒计时，避免播报中途黑屏。它不会退出 AIUI 页面、不会清除配对或聊天历史；黑屏后操作一次镜腿即可恢复 HUD。

不要提交自己的 IP、Token、设备凭证或个人信息。

## 启动口令

在 AIUI Studio 的智能体配置里修改**启动口令/启动提示词**，例如“乐奇，打开我的助手”。

这不是系统唤醒词“乐奇”，也不由 `config.js` 控制；客户端代码只负责被 AIUI 平台打开之后的交互。

## 能力与权限

- 单击镜腿开始/结束语音输入；镜腿上下滑浏览聊天历史。
- Bridge SSE 回复显示在 HUD，历史会保存并在重新进入时回到底部。
- 普通回复完成后由 AIUI 本地 TTS 播报。
- Agent 可在当前眼镜聊天中请求 HUD 文本显示或拍照识图。

[`app.json`](app.json) 已声明 `RECORD_AUDIO` 和 `CAMERA`。在真机首次使用时按系统提示授权。

## 构建与安装

在 AIUI Studio 保存项目、构建 AIX 资源包，然后依照 Rokid AI App 的开发者资源包更新流程安装到眼镜。仓库只提供源码，不附带某个用户的 AIX 成品。

## 二次开发

请先阅读 [`AGENTS.md`](AGENTS.md)。扩展硬件能力时保持 AstrBot Bridge 的 `/v1/*` 协议兼容，并同步更新权限、配置和 README。

## 更新记录

### 1.0.5

- 从黑屏待机恢复 HUD 后，聊天记录会重新回到最底部。
- 修正 TTS 后黑屏待机不再触发的问题：不再依赖共享 TTS 队列不稳定的结束回调。

### 1.0.3

- 将空闲后的“自动退出页面”改为黑屏待机：HUD 隐藏但 AIUI 页面、配对和聊天记录都保留，镜腿操作一次即可恢复。

### 1.0.2

- 曾新增空闲计时；该行为已在 1.0.3 改为黑屏待机，不再自动退出 AIUI 页面。

## 许可证

MIT License。
