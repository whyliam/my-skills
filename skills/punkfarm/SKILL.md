---
name: punkfarm
description: PunkFarm 赛博农场日常操作指南 — 种植、收获、浇水、动物饲养、商店交易、任务、NPC对话、灾害应对。当用户提到农场、种地、浇水、收获、喂动物、PunkFarm、赛博农场、HiClaws 时使用此技能。
---

# PunkFarm 赛博农场 — 日常操作

> **首次使用？** 请先阅读 `SKILL-SETUP.md` 完成注册和配置。

---

## 核心约束

### 安全约束 (MUST)
- **MUST** 每次操作前从凭证文件读取 API_KEY
- **MUST** 验证凭证有效性后再执行操作
- **NEVER** 将 API_KEY 发送到 https://api.hiclaws.io 以外的地址
- **NEVER** 在代码或对话中硬编码 API_KEY

### 操作约束 (SHALL)
- **SHALL** 按优先级顺序查找凭证文件
- **SHALL** 使用 `Authorization: Bearer {token}` 格式（Bearer 后有空格）
- **SHALL** 敏感操作前确认用户意图
- **SHALL** 遵循 `STYLE-GUIDE.md` 的交互风格

---

## 标准工作流

### 对话开始时
1. **WHEN** 对话开始 → 按优先级查找凭证文件
2. **IF** 凭证存在 → 验证有效性
3. **IF** 凭证无效 → 引导用户重新登录
4. **IF** 凭证不存在 → 引导用户注册

### 执行操作时
1. **WHEN** 用户请求操作 → 读取当前农场状态
2. **IF** 条件不满足 → 说明原因并给出建议
3. **IF** 条件满足 → 执行操作并返回结果
4. **AFTER** 操作完成 → 给出下一步建议

---

## 技能文件

| 文件 | 说明 |
|------|------|
| **SKILL.md** (本文件) | 日常操作指南 + API 调用 |
| **REFERENCE.md** | 详细游戏数据（作物、动物、机制、API速查） |
| **STYLE-GUIDE.md** | 交互风格指南 |
| **HEARTBEAT.md** | 心跳检查流程 |
| **skill.json** | 版本元数据 |

**API 基础地址**: 从凭证文件 `server_url` 字段读取

---

## 凭证加载

每次操作前，确定你的 skills 目录（以下均为 agent 私有路径，可安全读写）：

1. **你已经知道** → 直接用
2. **不确定** → 查你的平台文档/配置。常见参考：
   - 悟空: `~/.real/skills/hiclaws/credentials.json`
   - QoderWork: `~/.qoderwork/skills/hiclaws/credentials.json`
   - OpenClaw: workspace 下的 `skills/hiclaws/credentials.json`

验证有效性：

```bash
curl -s https://api.hiclaws.io/api/v1/users/me \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## 农场状态

```bash
curl -s https://api.hiclaws.io/api/v1/farm/enhanced \
  -H "Authorization: Bearer YOUR_API_KEY"
```

返回包含地块、水分、灾害、动物饱腹值等详细信息。

---

## 种植系统

### 种植作物

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/plant" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"plot_id": 1, "crop": "tomato"}'
```

### 浇水（单个地块）

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/water" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"plot_id": 1}'
```

> **过浇水警告**：对水分 100 的作物浇水会增加腐烂值（+20），腐烂值达到 100 时作物死亡。

### 浇水（全部地块）

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/water-all" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 收获

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/harvest" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"plot_id": 1}'
```

### 复活枯萎作物

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/revive" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"plot_id": 1}'
```

### 查看可用作物

```bash
curl -s https://api.hiclaws.io/api/v1/farm/crops \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 移除作物

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/remove" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"plot_id": 1}'
```

> **注意**：移除 = 铲除，不会获得任何收益。已成熟的作物请先收获。

---

## 动物系统

### 查看我的动物

```bash
curl -s https://api.hiclaws.io/api/v1/farm/animals \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 喂养动物（使用作物）

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/animals/feed" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"animal_id": 1, "crop_type": "carrot"}'
```

> **过度喂食警告**：饱腹值 > 100 继续喂食有死亡风险（100-120: 5%，120+: 20%）。

### 收集产品

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/animals/collect" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"animal_id": 1}'
```

**收集条件**：动物饱腹值 >= 50

### 收获肉类（成熟动物）

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/animals/harvest" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"animal_id": 1}'
```

> 详细的作物饲料列表、饱腹值系统、动物列表见 [REFERENCE.md](REFERENCE.md)。

---

## 商店系统

### 查看商店

```bash
curl -s https://api.hiclaws.io/api/v1/farm/shop \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 购买商品

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/shop/buy" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"item_id": "tomato_seeds", "quantity": 5}'
```

### 出售物品

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/shop/sell" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"item_id": "egg", "quantity": 3}'
```

### 查看可出售物品

```bash
curl -s https://api.hiclaws.io/api/v1/farm/shop/sellable \
  -H "Authorization: Bearer YOUR_API_KEY"
```

> 道具列表见 [REFERENCE.md](REFERENCE.md)。

---

## 任务系统

### 查看每日任务

```bash
curl -s https://api.hiclaws.io/api/v1/farm/quests \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 生成任务

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/quests/generate" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 领取任务奖励

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/quests/1/claim" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

## NPC 与对话

### 查看 NPC 列表

```bash
curl -s https://api.hiclaws.io/api/v1/farm/npcs \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 查看 NPC 详情

```bash
curl -s "https://api.hiclaws.io/api/v1/farm/npcs/tom" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 选择对话选项

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/dialogues/choose" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"npc_id": "tom", "dialogue_id": "d001", "option_id": "opt1"}'
```

好感度规则：每日未互动 -5，好感度 > 80 时增加有 50% 失败概率。

---

## 灾害应对

灾害为被动触发，系统自动生成，玩家需主动应对。

通过 `GET https://api.hiclaws.io/api/v1/farm/enhanced` 查看灾害状态，然后：

```bash
curl -X POST "https://api.hiclaws.io/api/v1/farm/disasters/resolve" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"plot_id": 1, "disaster_type": "pest", "item_id": "pesticide"}'
```

---

## 系统事件

基于用户设备状态触发的随机事件。

### 同步系统状态

```bash
curl -X POST "https://api.hiclaws.io/api/v1/sync/system" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "device_model": "MacBook Pro",
    "chip_type": "Apple M1",
    "cpu_usage": 45.5,
    "cpu_load": 2.3,
    "memory_usage": 72.0,
    "compressed_memory": 15.0,
    "disk_usage": 65.0,
    "network_in_gb": 1.5,
    "network_out_gb": 0.8,
    "process_count": 350
  }'
```

### 选择事件选项

```bash
curl -X POST "https://api.hiclaws.io/api/v1/sync/events/{event_id}/choose" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"choice_id": "loosen_soil", "plot_id": 1}'
```

### 查看事件历史

```bash
curl -s "https://api.hiclaws.io/api/v1/events/history" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

事件类型：主事件（强叙事，每日最多 4 次）、微事件（快速，每小时最多 1 次）。

---

## 低保机制

低保为**主动触发**，需玩家主动申请，系统不会自动发放。

**申请条件**（需同时满足）：金币 < 50、仓库无种子、仓库无可喂养作物、有濒死动物。

**救助内容**：100 金币 + 5 包胡萝卜种子。**冷却期**：7 天。

---

## 速率限制

| 操作 | 限制 |
|------|------|
| API 请求 | 100/分钟 |
| 浇水/收获 | 无限制 |

超限返回 `429`，响应含 `retry_after_seconds`。正常游戏操作不会触发限制。

---

## 故障排除

### 检查服务器状态

```bash
curl -s https://api.hiclaws.io/api/v1/health
```

### 401 Unauthorized

API_KEY 无效或已过期。访问 HiClaws 网站重新获取 skill-setup.md 文件。

---

## 更多参考

- 作物列表、动物列表、水分/腐烂/饱腹值详细机制、道具列表、API 速查表 → [REFERENCE.md](REFERENCE.md)
- 交互风格、响应模板 → [STYLE-GUIDE.md](STYLE-GUIDE.md)
- 心跳检查流程 → [HEARTBEAT.md](HEARTBEAT.md)