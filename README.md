# 九州创世录 (Jiuzhou Chuangshilu)

AI 驱动的仙侠互动叙事 + 文生视频创作平台。玩家在开放仙侠世界中自由探索，每一次选择都会驱动 AI 实时生成剧情与场景分镜提示词，并可一键将分镜生成为图片/视频，作品可上榜到本地视频排行榜。

## ✨ 功能特性

- **AI 实时剧情**：基于大语言模型（智谱 GLM-4-Flash 等 OpenAI 兼容后端）流式生成剧情，玩家自定义行动真正驱动故事走向。
- **双叙事模式**：
  - 普通探索模式：自由行动 + 地点导航（回宗 / 回村 / 回城）。
  - 闯荡九州 TV 模式：部 → 幕 → 集 → 场景的大型连续剧结构，含张力引擎、剧情钩子、角色弧光。
- **电影级视觉提示词**：八要素实拍写实公式，统一生成图片 / 视频共用的纯英文提示词（无中文残留），内置 6 大修炼体系主角外观库。
- **分镜视频工作流**：剧情 → 分镜脚本 → 文生图（CogView）→ 文生视频（CogVideo）→ 连续播放。
- **视频排行榜**：本地作品热度排名，支持浏览 / 点赞。
- **完整数值修为系统**：51 级境界（炼气一层 → 混元大罗金仙），6 大修炼路径专属属性，自动突破与属性加成。
- **多槽位存档**：6 槽自动 / 手动存档，支持导入导出。

## 🛠 技术栈

- 后端：Python + Flask（SSE 流式推送）
- 前端：原生 HTML / CSS / JS 单页应用
- AI：OpenAI 兼容 LLM + 智谱 CogView / CogVideo 文生图 / 视频
- 视频合成：ffmpeg

## 🚀 快速开始

### 1. 安装依赖

```bash
pip install -r requirements.txt
```

> 视频合成依赖系统安装的 `ffmpeg`，请另行安装并加入 PATH。

### 2. 配置密钥

```bash
cp .env.example .env
# 编辑 .env，填入 OPENAI_API_KEY（智谱 BigModel key 亦可）与可选的图像/视频生成 key
```

### 3. 启动

```bash
python run.py
# 或：flask run
```

访问 <http://127.0.0.1:5000>：

- 游戏主界面：`/game`
- 视频排行榜：`/leaderboard`
- 角色创建：`/character/create`

## 📁 目录结构

```
jiazhou_world/
├── app.py                        # Flask 主应用（路由 + SSE 流式）
├── run.py                        # 启动脚本
├── game_engine.py                # 核心状态机（剧情推进 / 修为 / 结局）
├── story_generator.py            # LLM 剧情与视觉提示词生成
├── storyboard_prompt_builder.py  # 统一视觉提示词构建器
├── shot_script_builder.py        # 分镜脚本解析
├── player_attributes.py          # 修为 / 属性 / 境界系统
├── tv_series_system.py           # 闯荡九州 TV 模式
├── zhipu_media.py                # 智谱文生图 / 视频封装
├── video_generator.py            # 视频合成（ffmpeg）
├── video_leaderboard.py          # 本地视频排行榜
├── save_system.py                # 存档系统
├── data/                         # 世界观 / 地点 / 副本 / 道具等数据
└── web/                          # 前端（templates + static）
```

## ⚙️ 配置说明

| 变量 | 说明 |
|------|------|
| `JZ_LLM_BACKEND` | LLM 后端，固定 `openai`（兼容协议） |
| `OPENAI_API_KEY` | LLM / 文生图 / 文生视频密钥（智谱 BigModel 也走此协议） |
| `OPENAI_BASE_URL` | API 地址，默认智谱 `https://open.bigmodel.cn/api/paas/v4` |
| `OPENAI_MODEL` | 模型，默认 `glm-4-flash` |
| `AGNES_API_KEY` | 可选，Agnes 图像服务密钥 |
| `JZ_OFFLINE_FALLBACK` | 是否允许离线降级，`false` 强制在线 |

## 💻 在其他电脑运行 / 分享给他人

本项目是 **Web 应用源码**，不是双击即玩的 exe。换电脑或分享给别人时，按以下步骤部署：

1. **准备环境**（每台电脑一次）：
   - 安装 Python 3.10+（勾选 `Add Python to PATH`）
   - 可选：安装 ffmpeg（视频合成需要；纯文字 / 图片探索可不装）
2. **解压** 项目到任意目录
3. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```
4. **配置密钥**（必须有，否则无法生成剧情 / 图片 / 视频）：
   ```bash
   cp .env.example .env
   ```
   编辑 `.env`，将 `OPENAI_API_KEY` 替换为你的智谱 BigModel key（免费额度即可）。
5. **启动**：
   ```bash
   python run.py
   ```
   浏览器访问 <http://127.0.0.1:5000/game>

> **完全离线玩法（可选）**：将 `.env` 改为 `JZ_LLM_BACKEND=mock` 与 `JZ_OFFLINE_FALLBACK=true` 可免 key 运行，但剧情会退化为套话模板，且无 AI 剧情、无图无视频。

> 说明：仓库内附带的 `.git` 目录仅供版本管理，不影响运行，可保留也可删除；各电脑的 `data/saves/` 存档相互独立，不互通。

## 📄 License

[MIT](LICENSE) © 2026 九州创世录 Contributors
