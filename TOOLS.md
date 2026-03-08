# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

## 📌 DingTalk 配置

**钉钉用户 ID（unionId）：** `48265002531121290`

> ⚠️ **重要：** 创建钉钉提醒时必须使用完整的用户 ID，不要用截断的版本！

### 创建钉钉提醒的标准命令：

```bash
openclaw cron add --name "提醒名称" \
  --cron "0 21 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --deliver \
  --message "提醒内容" \
  --channel dingtalk \
  --to "48265002531121290"
```

### 当前提醒任务：

| 任务 | ID | 时间 | 用户 ID |
|------|----|------|--------|
| 穿衣服提醒 | `abd81216-decc-4712-81d2-520406219624` | 每天 10:06 | 48265002531121290 |
| 关窗户提醒 | `8b10d45f-d2ef-444f-84ea-47765dd170fc` | 每天 21:00 | 48265002531121290 |
