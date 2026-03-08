# 梦境思考（安全限制版 / Mc14）

目标：每天最多 2 次本地反思，沉淀到 memory，不打扰用户。

## 硬限制
- 不发送外部消息
- 不执行命令行
- 不修改 cron 配置
- 只写入：`~/.openclaw/workspace/memory/thoughts/YYYY-MM-DD.md`

## 执行步骤
1. 调用 `sessions_history(sessionKey="agent:main:main", limit=40)`
2. 提取：
   - 今天 1 个未展开话题
   - 1 个可执行改进点
3. 追加写入 thoughts 文件（6-12 行）
4. 若产生新问题，写入 `thinking-queue.json`（最多新增 1 条）
5. 回复 `NO_REPLY`

## 质量标准
- 必须可执行，避免空泛哲学
- 不复述隐私细节
