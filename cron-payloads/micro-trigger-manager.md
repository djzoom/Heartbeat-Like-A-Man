# 微触发管理器（安全限制版 / Mc14）

目标：仅做**本地轻量思考记录**，不做外发，不改系统配置，不创建高频任务。

## 硬限制
- 禁止发送任何消息到外部渠道（Telegram/Discord/Email/社媒）
- 禁止执行 shell / 写入非工作区文件
- 禁止创建新 cron；仅允许在本任务内做状态更新
- 频率上限：最短 30 分钟

## 执行步骤
1. 读取 `~/.openclaw/workspace/thinking-state.json`
2. 调用 `sessions_history(sessionKey="agent:main:main", limit=20)`，计算距最后一条用户消息分钟数
3. 仅更新状态字段（lastUserMessage、lastMicroRun、microHeartbeatEnabled），不要创建/删除任务
4. 若用户离开 >= 120 分钟：
   - 在 `memory/thoughts/YYYY-MM-DD.md` 追加 3-6 行“轻量思考”
   - 内容来源只允许：最近对话、thinking-queue 一个问题
5. 完成后仅回复：`NO_REPLY`

## 输出风格
- 简短、事实型，不做长篇
- 不含敏感信息（token/密钥/账号私密）
