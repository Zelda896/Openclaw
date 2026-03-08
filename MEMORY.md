# MEMORY.md - Long-Term Memory

_Last updated: 2026-02-26_

## 👤 About Alicia

- **Name:** 阿莉西亚 (Alicia)
- **Timezone:** Asia/Shanghai
- **Language:** 中文 (Chinese)
- **Education:** 大二，电子信息工程专业
- **Competitions:** 电赛、嵌赛、物联网大赛
- **Identities:** 学生、作家、哲学家

## 📝 Preferences & Notes

- 使用 Telegram 接收消息和提醒
- 运行环境：树莓派 (Raspberry Pi)

## ⚠️ Critical Boundaries - NEVER DO WITHOUT ASKING

- **禁止擅自修改网络配置**（gateway、bind、端口、防火墙等）
- **禁止擅自重启/关机** OpenClaw gateway 或树莓派
- **任何可能影响连接的改动必须先告知用户，等待同意后再执行**
- 修改前必须仔细检查后果（多用 tokens 思考）
- 2026-02-26 曾因擅自修改 gateway bind 配置导致断连，严重警告！

## 🏠 Home & Devices

- 有关窗户的习惯（晚上需要提醒）

## 📅 Routines

- 晚上 9:00 关窗户提醒（Telegram）

## 🔧 Cron 提醒修复（重要！）

在设置需要发送到 Telegram 聊天的计划提醒时，使用此模式以避免 HEARTBEAT.md 依赖：

**✅ 有效方法：**
```bash
openclaw cron add --name "提醒名称" \
  --cron "0 21 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --deliver \
  --message "提醒内容" \
  --channel telegram \
  --to "7833190979"
```

**关键配置：**
- `sessionTarget: "isolated"` - 在单独的会话中运行，绕过心跳检查
- `payload.kind: "agentTurn"` - 不需要心跳文件
- `delivery.mode: "announce"` - 直接将结果发送到 Telegram 聊天
- `delivery.channel: "telegram"` - 明确指定渠道
- `delivery.to: "7833190979"` - 指定聊天 ID（**注意：只用数字 ID，不要用 "chat:" 前缀**）

**⚠️ 2026-03-01 修复记录：**
- 问题：Telegram 投递失败，错误 "Telegram recipient must be a numeric chat ID"
- 原因：`to` 字段使用了 `"chat:7833190979"` 格式，但 Telegram 只需要数字 ID
- 修复：将两个 cron 任务的 `to` 字段从 `"chat:7833190979"` 改为 `"7833190979"`
- 当前状态：配置已修复，但代理服务器 (192.168.3.79:7897) 暂时不可用，需检查代理状态

**⚠️ 2026-03-06 状态更新：**
- 问题：代理服务器 `192.168.3.79:7897` 仍然不可达
- 影响：两个 cron 提醒任务连续失败（穿衣服提醒 7 次，关窗户提醒 1 次）
- Telegram 消息发送完全失败，无法通知用户
- 待解决：需要修复代理服务器或更新代理配置

**⚠️ 2026-03-08 状态更新：**
- 问题：代理服务器 `192.168.3.79:7897` 仍不可达（已持续 2 天）
- 影响：穿衣服提醒、关窗户提醒继续失败，Telegram 投递无法完成
- 今日天气（上海）：⛅️ +8°C（体感 +6°C），西北风 12km/h，湿度 61%，无降水
- 处理：按系统要求内部处理，未打扰用户

**✅ 2026-03-08 钉钉提醒修复完成：**
- 问题：钉钉提醒投递失败，用户 ID 被截断为 `4826`
- 原因：创建提醒时使用了不完整的用户 ID
- 修复：
  1. 重启网关修复 Stream 连接（断开 1042 次）
  2. 删除旧提醒任务
  3. 用完整用户 ID `48265002531121290` 重新创建
- 当前状态：✅ 钉钉消息正常，提醒已配置完成
- 钉钉用户 ID 已记录到 `TOOLS.md`，以后创建任务会自动使用正确的 ID

**❌ 避免这种模式（在 Telegram 中不起作用）：**
- `sessionTarget: "main"` + `payload.kind: "systemEvent"` - 需要 HEARTBEAT.md，只会发到控制台

## 📚 Projects & Interests

_待补充..._

---

> This file evolves over time. Update it with significant events, decisions, and things worth remembering long-term.
