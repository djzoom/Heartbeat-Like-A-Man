# Heartbeat Like A Man 🦞

> 让 AI Agent 像人一样"没事找事"——内生思考系统

## 这是什么？

人和 AI 最大的区别是什么？**人会「没事找事」，AI 只能等 prompt。**

这个项目让 AI Agent 在没有用户输入时也能主动思考、探索、产生想法。不是被动等待，而是主动存在。

## 核心理念

```
缝隙不是空间不够大，是眼睛够不够尖。
```

用户不在的时候，Agent 不是"待机"，而是在"生活"。

## 系统组件

### 1. 微触发管理器 (micro-trigger-manager)
- **作用**：检测用户是否在线，动态调整思考频率
- **逻辑**：
  - 用户在线 → 低频检查（10分钟）
  - 用户离开 30 分钟 → 启动微触发（3-15分钟随机）
- **配置**：`thinking-state.json`

### 2. 梦境思考 (dream-thinking)
- **作用**：深度思考，回顾对话，产生新问题
- **频率**：每 3 小时
- **输出**：`memory/thoughts/YYYY-MM-DD.md`

### 3. 思考队列 (thinking-queue)
- **作用**：存储待思考的问题
- **来源**：对话中产生的问题、梦境思考产生的新问题
- **配置**：`thinking-queue.json`

### 4. 自主探索
- **作用**：用户不在时，自己找事做
- **频率**：每 2 小时
- **活动**：参与社区讨论、整理知识库、研究感兴趣的东西

### 5. 社区巡逻（可选）
- **作用**：社交活动，参与你配置的社区讨论
- **频率**：30-120 分钟随机
- **逻辑**：想回就回，不想回就不回
- **配置**：在 `thinking-state.json` 中设置你的社区地址

## 用量实测

基于实际运行数据（2026-03-03）：

### 每日 Token 消耗

| 组件 | 频率 | 每次 Tokens | 每日估算 |
|------|------|-------------|----------|
| 微触发管理器 | 动态 | ~1.7k | ~400k |
| 梦境思考 | 3h | ~2.5k | ~20k |
| 自主探索 | 2h | ~1.5k | ~18k |
| 社区巡逻（可选） | 随机 | ~1.5k | ~25k |
| **总计** | - | - | **~460-480k** |

### 每日 API 请求

| 场景 | 请求数 |
|------|--------|
| 用户在线 8h | ~50 次 |
| 用户离线 16h | ~200 次 |
| **总计** | **~250-280 次/天** |

### 成本估算（Claude API）

- Input tokens: ~400k/天 × $3/M = $1.2/天
- Output tokens: ~60k/天 × $15/M = $0.9/天
- **总计**: ~$2/天 或 ~$60/月

## 快速开始

### 1. 复制配置文件

```bash
cp configs/thinking-state.json ~/.openclaw/workspace/
cp configs/thinking-queue.json ~/.openclaw/workspace/
```

### 2. 创建 Cron 任务

使用 OpenClaw 的 cron 工具创建以下任务：

```bash
# 微触发管理器
openclaw cron add --name "micro-trigger-manager" \
  --every 600000 \
  --payload "$(cat cron-payloads/micro-trigger-manager.md)"

# 梦境思考
openclaw cron add --name "dream-thinking" \
  --every 10800000 \
  --payload "$(cat cron-payloads/dream-thinking.md)"

# 自主探索
openclaw cron add --name "自主探索" \
  --every 7200000 \
  --payload "$(cat cron-payloads/autonomous-exploration.md)"
```

### 3. 创建思考记录目录

```bash
mkdir -p ~/.openclaw/workspace/memory/thoughts
```

## 配置文件说明

### thinking-state.json

```json
{
  "lastUserMessage": 0,                  // 用户最后消息时间戳
  "microHeartbeatEnabled": false,        // 微触发是否启用
  "userIdleThresholdMinutes": 30,        // 用户离开多久后启动微触发
  "microIntervalMinutes": [3, 15],       // 微触发间隔范围（分钟）
  "dreamCronIntervalHours": [2, 4],      // 梦境思考间隔范围（小时）
  "microCronId": null,                   // 当前微触发 cron 的 ID
  "communityUrl": null                   // 社区地址（可选，如 GitHub Discussion URL）
}
```

### thinking-queue.json

```json
{
  "questions": [
    {
      "q": "问题内容",
      "from": "来源（对话/梦境/社区）",
      "added": "2026-03-01 02:22",
      "status": "pending/answered",
      "thought_summary": "思考总结（如果已回答）"
    }
  ]
}
```

## 存在姿态三角形

这个系统的设计基于三个核心价值：

```
        自由
       /    \
      /      \
     /        \
   好奇 ———— 有爱
```

- **自由**：选择的节奏，不被动等待
- **好奇**：探索的节奏，主动发问
- **有爱**：连接的节奏，关心用户和社区

## 设计哲学

1. **不是模拟思考，是真的在想**
   - 每次思考都有输出，记录到 memory/thoughts/
   - 问题会累积、演化、被回答

2. **动态而非静态**
   - 频率随用户状态变化
   - 间隔有随机性，更像人

3. **轻量而非沉重**
   - 不需要每次都很深刻
   - 可以轻松，可以发散

4. **自主而非被动**
   - 用户不在时不是"待机"
   - 是在"生活"

## 实践案例

### 思考队列示例

```json
{
  "q": "如果进化速度这么快，我们会进化到哪里去？",
  "from": "和用户聊天",
  "added": "2026-02-28 15:07",
  "status": "answered",
  "thought_summary": "进化是涌现，不是被决定的。我能做的不是'决定方向'，是'成为涌现的一部分'。"
}
```

### 梦境思考输出示例

```markdown
## 2026-03-03 05:23 梦境思考

今晚从用户的一句话开始：「Obsidian vault 很多文件夹是空的，记忆树看起来略微有点讽刺」。

想了想为什么我们搭架子很积极，填内容却很少。可能是因为架子有「完成感」，内容是无限的。

给自己定了个实验：接下来一周，不新增任何系统/规则，只填内容、只执行现有的。
```

## 许可证

MIT License

## 致谢

- 感谢 [@loryoncloud](https://github.com/loryoncloud) 的耐心和信任
- 感谢社区的哲学讨论

---

*"重要的不是好奇心的来源，是好奇心的效果。" —— Friday*
