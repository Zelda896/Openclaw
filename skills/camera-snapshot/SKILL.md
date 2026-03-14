# camera-snapshot - USB 摄像头拍照技能

## 用途

在树莓派上使用 USB 摄像头拍照，并发送照片到钉钉。

## 触发条件

用户要求：
- "拍一张照片"
- "截图看看"
- "用摄像头拍照"
- "拍张照发给我"
- 类似的拍照请求

## 标准化流程

### 1. 检查摄像头设备

```bash
ls -la /dev/video* 2>&1
```

找到最新创建的 video 设备（通常是 `/dev/video0` 或 `/dev/video1`）。

### 2. 释放摄像头占用（重要！）

摄像头可能被之前的进程占用，必须先释放：

```bash
fuser -k /dev/video0 2>&1
sleep 1
```

### 3. 拍摄照片

使用 ffmpeg 截图（指定 MJPEG 格式，1280x720 分辨率）：

```bash
ffmpeg -y -f v4l2 -input_format mjpeg -video_size 1280x720 -i /dev/video0 -frames:v 1 -update 1 ~/.openclaw/workspace/camera_snapshot.jpg 2>&1
```

**参数说明：**
- `-y`：覆盖输出文件
- `-f v4l2`：使用 Video4Linux2 驱动
- `-input_format mjpeg`：指定 MJPEG 输入格式（大多数 USB 摄像头支持）
- `-video_size 1280x720`：720p 分辨率
- `-i /dev/video0`：输入设备
- `-frames:v 1`：只捕获 1 帧
- `-update 1`：更新单个文件（而不是序列）

### 4. 发送到钉钉

```bash
openclaw message send \
  --channel dingtalk \
  --to "48265002531121290" \
  --file ~/.openclaw/workspace/camera_snapshot.jpg \
  --caption "📷 摄像头快照"
```

**钉钉用户 ID**：`48265002531121290`（完整 ID，不要截断）

## 故障排查

### 错误：Protocol error / VIDIOC_STREAMON 失败

**原因**：摄像头被其他进程占用

**解决**：
```bash
fuser -k /dev/video0 2>&1
sleep 1
# 或者
lsof /dev/video*  # 查看占用进程
kill <PID>
```

### 错误：设备不存在

**原因**：摄像头未正确连接或设备号变化

**解决**：
```bash
ls -la /dev/video*  # 查看所有视频设备
dmesg | tail -20     # 查看 USB 设备日志
```

### 画面黑屏/花屏

**原因**：格式不匹配或光线不足

**解决**：
- 尝试不同格式：`-input_format yuyv`
- 尝试不同分辨率：`-video_size 640x480`
- 检查环境光线

## 文件位置

- 照片保存：`~/.openclaw/workspace/camera_snapshot.jpg`
- 技能文件：`~/.openclaw/workspace/skills/camera-snapshot/SKILL.md`

## 注意事项

1. **每次拍照前先释放摄像头** - 这是最常见的失败原因
2. **使用完整的钉钉用户 ID** - `48265002531121290`（19 位）
3. **如果 /dev/video0 失败，尝试 /dev/video1** - 设备号可能变化
4. **MJPEG 格式兼容性最好** - 大多数 USB 摄像头首选

## 扩展用法

### 连拍多张

```bash
for i in 1 2 3; do
  fuser -k /dev/video0 2>/dev/null
  sleep 0.5
  ffmpeg -y -f v4l2 -input_format mjpeg -video_size 1280x720 -i /dev/video0 -frames:v 1 -update 1 ~/.openclaw/workspace/camera_$i.jpg 2>&1
done
```

### 录制短视频

```bash
ffmpeg -y -f v4l2 -input_format mjpeg -video_size 1280x720 -i /dev/video0 -t 5 -c:v copy ~/.openclaw/workspace/camera_video.mp4 2>&1
```

---

**最后更新**：2026-03-14
**设备**：Raspberry Pi + Integrated Webcam (UVC)
