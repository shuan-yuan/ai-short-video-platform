# AI短视频制作平台

基于 [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) 的中文短视频制作平台，面向抖音和微信视频号。项目保留上游完整 Git 历史和 MIT License。

## V1 能力

- 中文 WebUI：输入主题或关键词，自动生成脚本与素材关键词。
- 六类内容预设：知识、情感、财经、故事、资讯、解说。
- 竖屏优先：默认 `9:16`，适用于抖音、视频号和快手。
- 素材匹配：Pexels、Pixabay、Coverr、本地素材，以及可选 AI 视频素材。
- 配音、字幕、BGM、转场、MP4 导出均由上游生产管线完成。
- 扩展接口：保留 `openai_image`、`wavespeed`、`ofox` 等素材源；其中 OFox 可配置 Wan 等 text-to-video 模型，ComfyUI 可通过 OpenAI-compatible image endpoint 接入。

## DeepSeek 配置

首次运行时在 WebUI 的“大模型设置”中选择 **DeepSeek**，填写你自己的 API Key。密钥只保存在本机 `config.toml`，不要提交到 GitHub。

建议设置：

```toml
[app]
llm_provider = "deepseek"
deepseek_api_key = "在本机填写"
deepseek_base_url = "https://api.deepseek.com"
deepseek_model_name = "deepseek-v4-pro"

[ui]
video_aspect_pexels = "9:16"
subtitle_enabled = true
subtitle_position = "bottom"
```

> 模型名称会随 DeepSeek 发布策略变化；以 DeepSeek 控制台中可用的模型为准。不要将真实 API Key 写入示例配置、提交记录或问题反馈。

## 六类内容的主题写法

在“视频主题”中加上内容类型和对象，脚本质量会更稳定：

| 类型 | 示例主题 |
| --- | --- |
| 知识类 | `知识类：用 60 秒解释为什么彩虹是弧形，面向中学生` |
| 情感类 | `情感类：写给异地恋伴侣的一段温柔晚安文案，45 秒` |
| 财经类 | `财经类：解释指数基金定投的基本原理，不构成投资建议，60 秒` |
| 故事类 | `故事类：一个快递员帮助迷路小孩回家的反转故事，90 秒` |
| 资讯类 | `资讯类：根据已核实资料概述一项科技新闻，标明信息日期，45 秒` |
| 解说类 | `解说类：用轻松口吻解说一部电影的开场剧情，避免剧透结局，60 秒` |

财经与资讯内容应由发布者自行核实来源、日期与合规要求；平台不会替代专业意见或新闻核验。

## 运行

需要 Python 3.11+、FFmpeg，以及可选 Docker。安装依赖后执行：

```bash
uv sync
uv run streamlit run webui/Main.py
```

也可沿用上游的 Docker Compose 与 `webui.bat` / `webui.sh` 启动方式。生成完成后在任务列表中下载 MP4。

## 画面生成扩展

- **ComfyUI**：将 `openai_image_base_url` 指向提供 `/images/generations` 兼容接口的 ComfyUI 网关，选择 `video_source = "openai_image"`。
- **Wan 等视频模型**：选择对应 AI 视频素材源并配置模型 ID、API Key 与费用确认选项。
- **本地素材优先**：选择 `video_source = "local"`，可避免第三方素材源的不确定性。

每种外部素材或模型服务均有独立账号、费用、许可证和内容审核规则，启用前请单独确认。