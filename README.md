# 盲人 / 低视力 AI 导航眼镜 · 开源项目合集

> 收录能给视障人士用的 AI 导航眼镜开源项目。每个仓库的星标、最后推送、许可证都经过 GitHub API 逐个核对，不是抄来的。

**核对日期**：2026-11-14 ｜ **收录**：12 个仓库 ｜ **状态**：全部存活

---

## 先看结论

这个方向上 **90% 的项目是 Arduino + 超声波传感器的课程作业**，没有 AI。真正能跑起来的只有两个：

- **[AI-FanGe/OpenAIglasses_for_Navigation](https://github.com/AI-FanGe/OpenAIglasses_for_Navigation)** —— 功能最完整的端到端框架，走盲道、过马路、找物品、语音对话全都有。代价是要独立显卡。
- **[DresdenGman/AIGlasses_for_navigation](https://github.com/DresdenGman/AIGlasses_for_navigation)** —— 纯规则引擎，开箱即跑，不接任何大模型。适合先跑通链路。

想接自己的大模型、又没有显卡的，看第二档的 Gemini Live 方案。

---

## 目录

- [总览](#总览)
- [第一档 · 能真正跑起来的](#第一档--能真正跑起来的)
- [第二档 · API-first 轻量方案](#第二档--api-first-轻量方案)
- [第三档 · 中文小项目](#第三档--中文小项目)
- [怎么选](#怎么选)
- [踩坑清单](#踩坑清单)

---

## 总览

| # | 项目 | ★ | Fork | 最后推送 | 语言 | 许可 | 定位 |
|---|---|-----:|-----:|---------|------|------|------|
| 1 | [AI-FanGe/OpenAIglasses_for_Navigation](https://github.com/AI-FanGe/OpenAIglasses_for_Navigation) | 1,406 | 413 | 2025-11-01 | Python | MIT | 端到端完整框架（天花板） |
| 2 | [DresdenGman/AIGlasses_for_navigation](https://github.com/DresdenGman/AIGlasses_for_navigation) | 400 | 31 | 2026-11-06 | Python | MIT | 规则引擎，安全优先，CPU 可跑 |
| 3 | [lpecom/GemGlasses](https://github.com/lpecom/GemGlasses) | 3 | 1 | 2026-07-13 | Kotlin | ⚠️ 无 | Gemini Live + Meta Ray-Ban |
| 4 | [dm3n/VisionClaw](https://github.com/dm3n/VisionClaw) | 0 | 0 | 2026-04-16 | — | ⚠️ 无 | Gemini Live + Meta DAT SDK |
| 5 | [jmanhype/VisionClaw-Android](https://github.com/jmanhype/VisionClaw-Android) | 3 | 2 | 2026-03-26 | Kotlin | ⚠️ 无 | 上面的 Android 移植 |
| 6 | [maosuarez/KodaVision](https://github.com/maosuarez/KodaVision) | 1 | 0 | 2026-08-25 | Python | MIT | Gemini 2.5 Flash + YOLOv8 + OCR |
| 7 | [PranavAchar01/SonarSight](https://github.com/PranavAchar01/SonarSight) | 2 | 1 | 2026-10-27 | Kotlin | MIT | Ray-Ban Meta 第一视角 + 端侧 YOLO |
| 8 | [hpcclab/Meta-SmartSight](https://github.com/hpcclab/Meta-SmartSight) | 0 | 0 | 2026-10-19 | Kotlin | ⚠️ 无 | 学术组的 Ray-Ban Meta 方案 |
| 9 | [rudra496/sightlineai](https://github.com/rudra496/sightlineai) | 4 | 1 | 2026-11-11 | Python | MIT | YOLO + ESP32 + TTS，**最活跃** |
| 10 | [haha2345/CyanBridge](https://github.com/haha2345/CyanBridge) | 2 | 0 | 2026-03-11 | Kotlin | ⚠️ 无 | HeyCyan 眼镜 SDK 开源替代 App |
| 11 | [fangfano/Blind-Assistive-Glasses](https://github.com/fangfano/Blind-Assistive-Glasses) | 3 | 0 | 2025-06-18 | Python | MIT | 中文语音问询（已停更） |
| 12 | [Xgaiguai/AI-Blind-Glasses-Server](https://github.com/Xgaiguai/AI-Blind-Glasses-Server) | 3 | 0 | 2026-06-06 | Python | ⚠️ 无 | 智慧盲人眼镜服务端（已停更） |

> ⚠️ **许可是硬约束**：标"无"的六个仓库没有 LICENSE 文件。按 GitHub 默认规则即"保留所有权利"——法律上**不能直接改成自己的产品**，只能读代码学思路。要商用只看 MIT 的那几个。

---

## 第一档 · 能真正跑起来的

### 1. AI-FanGe/OpenAIglasses_for_Navigation ★1,406

整个方向的天花板。2025-10-20 建仓，413 fork，23.6MB，中文文档齐备。

**功能**

- **盲道导航**：YOLO 分割 + Lucas-Kanade 光流稳定掩码抑制抖动；障碍物检测 + 避障路线 + 急弯预警
- **过马路**：斑马线检测与对齐引导 + 红绿灯颜色/形状判定
- **物品查找**：YOLO-E 开放词汇 + ByteTrack 追踪 + MediaPipe 手部引导（引导手伸过去并确认抓取）
- **语音**：DashScope Paraformer 实时 ASR + Qwen-Omni-Turbo 多模态（图 + 文进、语音出）
- **接口**：WebSocket 视频推流、浏览器实时监控、ESP32-CAM / IMU 接入、Three.js 姿态可视化

**为什么说它好接**：`omni_client.py` 用的是标准 OpenAI SDK，`base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"`。换 DeepSeek 或任何 OpenAI 兼容端点，改两行 + 换 model 名即可。

**代价**

- 硬性要求 NVIDIA GPU，CUDA 11.8+，建议 RTX 3060 以上；Python 3.9–3.11
- 模型不在仓库里，需从 ModelScope 下 `archifancy/AIGlasses_for_navigation`（yolo-seg / yoloe-11l-seg / shoppingbest5 / trafficlight / hand_landmarker，合计约 200MB）
- ⚠️ **源码里硬编码了一个 DashScope API Key**（`omni_client.py` 中作为 `os.getenv` 的缺省值），公开泄露。clone 后第一件事：删掉它，换成自己的。

```bash
git clone https://github.com/AI-FanGe/OpenAIglasses_for_Navigation
cd OpenAIglasses_for_Navigation
python -m venv venv && venv\Scripts\activate
pip install -r requirements.txt
set DASHSCOPE_API_KEY=你的key
```

### 2. DresdenGman/AIGlasses_for_navigation ★400

定位与上面相反：**不接任何大模型**。

- 纯 CPU 规则引擎：红绿灯状态、障碍物 ≤1.5m、斑马线、置信度 <0.70 直接弃权、3 秒去重
- FastAPI + REST / WebSocket + 设备网关（120 req/min）+ 语音/触觉客户端
- 无需 GPU、无需模型、无需 API key，装完依赖就能开面板
- 可选接 YOLOX 检测器（配 edge-tts 语音），那一步才需要模型

**要看清的地方**：作者在 `docs/one-pagers/admissions.md` 里写明这是学生作品集（Dresden Goehner，$32.29 美国 BOM / 中国 <$20，20 台 + 3 个社区）。自曝数据很诚实：

- 85 份问卷里 **51 份不是本人填写**（34 家属代填 / 14 组织者 / 3 其他）
- 可用性 3.47/5，48% 的人说不准
- 64 人中**只有 9 人还在用，35 人弃用**
- Bosch 红绿灯基准 61 帧里**弃权 57 帧**，唯一一次明确判断还把红灯读成绿灯
- 15 张实景 0 误报，但只有 1 次可执行检测，置信度 0.27–0.44（低于 0.70 门槛）
- v0.4 外壳只有 Blender 模型未制造；电池 3h15m 未跑正式测试

结论：**当"跑通链路的教学参考"很好，当产品不行。**

---

## 第二档 · API-first 轻量方案

共同点：AI 全在云端，本地只做采集和播报，换模型 = 换 API Key，不需要独立显卡。

| 项目 | 亮点 |
|---|---|
| [GemGlasses](https://github.com/lpecom/GemGlasses) | 纯 Kotlin/Compose，把 Meta Ray-Ban 当摄像头，Gemini Live API 实时对话，Cloudflare Workers 中转。三个里最干净 |
| [VisionClaw](https://github.com/dm3n/VisionClaw) + [Android 移植](https://github.com/jmanhype/VisionClaw-Android) | Gemini Live + Meta DAT SDK，有移植版说明可移植性被验证过 |
| [KodaVision](https://github.com/maosuarez/KodaVision) | Python，Gemini 2.5 Flash + YOLOv8 + Tesseract OCR，最接近"能读文字"的需求 |
| [SonarSight](https://github.com/PranavAchar01/SonarSight) | Kotlin，Ray-Ban Meta 第一视角 + 端侧 YOLO |
| [Meta-SmartSight](https://github.com/hpcclab/Meta-SmartSight) | hpcclab 学术组出品，架构参考价值 > 代码价值 |

---

## 第三档 · 中文小项目

| 项目 | 说明 |
|---|---|
| [sightlineai](https://github.com/rudra496/sightlineai) | 2026-11-11 最后推送，全表**最活跃**。YOLO + ESP32 + TTS，MIT |
| [CyanBridge](https://github.com/haha2345/CyanBridge) | HeyCyan 智能眼镜官方 SDK 的开源替代 App，适合研究"怎么连现成眼镜硬件" |
| [Blind-Assistive-Glasses](https://github.com/fangfano/Blind-Assistive-Glasses) | 中文语音问询（"前面是什么 / 这书上写的什么"），2025-06 后停更 |
| [AI-Blind-Glasses-Server](https://github.com/Xgaiguai/AI-Blind-Glasses-Server) | 智慧盲人眼镜服务端，2026-06 后停更 |

---

## 怎么选

| 你的情况 | 选它 | 理由 |
|---|---|---|
| 有 NVIDIA GPU，想改整条链路 | AI-FanGe | 唯一功能完整的端到端框架 |
| 没 GPU，想接自己的大模型 | GemGlasses / KodaVision | 云端 AI，换 key 就行 |
| 只想先看到东西跑起来 | DresdenGman | 无需 GPU / 模型 / key |
| 想拿现成眼镜当硬件 | CyanBridge / SonarSight | 直接对接商用眼镜 |
| 想避开 | 所有 "ultrasonic sensor + buzzer" 的 1★ Arduino 项目 | 那是测距仪，不是 AI |

---

## 踩坑清单

1. **先删硬编码密钥** —— AI-FanGe 的 `omni_client.py` 里那把 key 是公开的，别用
2. **先看 `pushed_at`** —— 这个领域开源仓普遍活不过半年。除 AI-FanGe（2025-11）和 sightlineai（2026-11）外基本已定格
3. **注意 LICENSE** —— 六个仓库无许可 = 默认保留所有权利，别指望直接商用
4. **模型不在仓库里** —— AI-FanGe 的模型在 ModelScope，不在 GitHub
5. **别只信 README 的效果描述** —— DresdenGman 那种把失败数据写进 admissions.md 的反而更可信

---

## 关于本项目

- 数据来源：GitHub REST API（`/repos/{owner}/{repo}`），逐条核对星标、fork、最后推送、许可证
- 机器可读版本见 [`repos.json`](repos.json)
- 排序逻辑：先按"能不能真跑起来"分档，档内按星标
- 更新方式：直接提 PR 或 issue 补充项目；星标会随时间过期，引用时请核对日期

本文档内容可自由取用。
