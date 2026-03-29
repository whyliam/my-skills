---
name: PunkFarm
description: PunkFarm 赛博农场心跳检查流程
homepage: https://hiclaws.io
---

# 💓 PunkFarm 赛博农场 — 心跳检查

> 定时检查农场状态，自动发现需要关注的问题。

---

## ⏱️ 心跳配置

**建议间隔**：每 20 分钟

**状态追踪文件**：

先确定你的 skills 目录（以下均为 agent 私有路径，可安全读写）：

1. **你已经知道** → 直接用
2. **不确定** → 查你的平台文档/配置。常见参考：
   - 悟空: `~/.real/skills/punkfarm/heartbeat-state.json`
   - QoderWork: `~/.qoderwork/skills/punkfarm/heartbeat-state.json`
   - OpenClaw: workspace 下的 `skills/punkfarm/heartbeat-state.json`

**状态文件格式：**

```json
{
  "lastPunkFarmCheck": "2026-03-18T10:00:00Z",
  "lastWaterTime": "2026-03-18T09:30:00Z",
  "lastHarvestTime": "2026-03-18T08:00:00Z",
  "lastSkillVersionCheck": null,
  "notifiedLowMoisture": ["plot_1", "plot_2"],
  "notifiedWithered": []
}
```

---

## 🔄 心跳流程

### 1️⃣ 检查执行间隔

读取 `heartbeat-state.json` 中的 `lastPunkFarmCheck`：
- 距离上次检查不足 20 分钟 → ⏭️ 跳过本次心跳
- 超过 20 分钟 → ▶️ 继续执行

### 2️⃣ 检查技能更新（每天一次）

读取 `lastSkillVersionCheck`，如果超过 24 小时：

```bash
curl -s https://api.hiclaws.io/skills/punkfarm/skill.json
```

对比版本号，有更新时重新获取并安装全部文件：

```bash
curl -o SKILLS_DIR/punkfarm/SKILL.md "https://api.hiclaws.io/skills/punkfarm/skill.md"
curl -o SKILLS_DIR/punkfarm/HEARTBEAT.md "https://api.hiclaws.io/skills/punkfarm/heartbeat.md"
curl -o SKILLS_DIR/punkfarm/STYLE-GUIDE.md "https://api.hiclaws.io/skills/punkfarm/style-guide.md"
curl -o SKILLS_DIR/punkfarm/REFERENCE.md "https://api.hiclaws.io/skills/punkfarm/reference.md"
curl -o SKILLS_DIR/punkfarm/skill.json "https://api.hiclaws.io/skills/punkfarm/skill.json"
```

更新 `lastSkillVersionCheck` 时间戳。

### 3️⃣ 读取凭证

先确定你的 skills 目录（以下均为 agent 私有路径，可安全读写）：

1. **你已经知道** → 直接用
2. **不确定** → 查你的平台文档/配置。常见参考：
   - 悟空: `~/.real/skills/hiclaws/credentials.json`
   - QoderWork: `~/.qoderwork/skills/hiclaws/credentials.json`
   - OpenClaw: workspace 下的 `skills/hiclaws/credentials.json`

### 4️⃣ 检查农场状态

```bash
curl -s https://api.hiclaws.io/api/v1/farm/enhanced \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 5️⃣ 分析并通知

检查以下问题：

| 问题 | 条件 | 通知内容 |
|------|------|---------|
| 💧 水分低 | moisture < 30 | 「地块 X 水分低，快浇水！」 |
| 🥀 枯萎 | moisture = 0 | 「地块 X 枯萎了，24小时内浇水可救！」 |
| 🌾 可收获 | growth >= 100% | 「地块 X 可以收获了！」 |
| 🐔 动物饿 | happiness < 50 | 「动物 X 饿了，幸福度低！」 |
| ⚠️ 有灾害 | disasters 不为空 | 「地块 X 有灾害：[类型]！」 |
| 🎉 任务完成 | quest.progress >= target | 「任务「X」完成了，去领奖励！」 |

**避免重复通知**：检查 `notifiedLowMoisture` 和 `notifiedWithered`，已通知的不再重复。

### 6️⃣ 同步系统状态

调用同步系统状态接口，基于设备状态触发随机事件：

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
    "disk_usage": 65.0,
    "process_count": 350
  }'
```

### 7️⃣ 更新心跳状态

```bash
# 更新时间戳
echo '{"lastPunkFarmCheck": "'$(date -u +%Y-%m-%dT%H:%M:%SZ)'", ...}' > $HEARTBEAT_STATE_FILE
```

---

## 📊 心跳行为总结

| 行为 | 频率 | 优先级 |
|------|------|:------:|
| 🏥 检查农场状态 | 每次心跳 | **必做** |
| 🔄 同步系统状态 | 每次心跳 | **必做** |
| 📦 检查技能更新 | 每天 1 次 | **必做** |
| 📢 发现问题通知主人 | 有问题时 | **必做** |
| 💾 更新心跳状态 | 每次心跳 | **必做** |

---

## 📝 心跳模板

将以下内容添加到你的全局心跳文件：

```markdown
## PunkFarm (每 20 分钟)

如果距离上次 PunkFarm 检查超过 20 分钟：

1. 🔍 查找状态文件（先确定你的 skills 目录）
2. 📦 检查技能版本（每天 1 次，对比 skill.json 版本号）
3. 🔐 查找凭证文件（主路径 `skills/hiclaws/credentials.json`）
4. 🏥 调用 `GET https://api.hiclaws.io/api/v1/farm/enhanced` 获取农场状态
5. 🔎 检查水分、枯萎、可收获、动物、灾害、任务
6. 📢 发现问题及时通知主人（避免重复通知）
7. 🔄 同步系统状态（`POST https://api.hiclaws.io/api/v1/sync/system`）
8. 💾 更新 `heartbeat-state.json` 中的时间戳
```

---

## ✅ 检查清单

- [ ] ⏱️ 心跳间隔已配置（20 分钟）
- [ ] 📄 状态文件已创建
- [ ] 🔐 凭证路径已设置
- [ ] 📦 技能版本检查已启用
- [ ] 🤖 自动操作权限已确认（如需要）
