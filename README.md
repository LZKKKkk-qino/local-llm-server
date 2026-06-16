# Local LLM Server

本地大模型私有化部署方案，提供 OpenAI 兼容 API 和 Web 交互界面。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c.svg)](https://pytorch.org/)
[![Transformers](https://img.shields.io/badge/Transformers-4.35+-orange.svg)](https://huggingface.co/docs/transformers/index)
[![vLLM](https://img.shields.io/badge/vLLM-0.2+-red.svg)](https://github.com/vllm-project/vllm)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com/)
[![Gradio](https://img.shields.io/badge/Gradio-4.0+-ff9f0c.svg)](https://gradio.app/)
[![OpenAI](https://img.shields.io/badge/OpenAI-1.0+-white.svg)](https://github.com/openai/openai-python)
[![Pydantic](https://img.shields.io/badge/Pydantic-2.0+-E92063.svg)](https://docs.pydantic.dev/)
[![Uvicorn](https://img.shields.io/badge/Uvicorn-0.24+-009688.svg)](https://www.uvicorn.org/)

## 项目简介

本项目提供了一个**本地私有化部署**的完整大模型服务解决方案：

- 🔒 **隐私安全**：本地运行，数据无需上传云端
- 🔄 **OpenAI 兼容 API**：完全兼容 OpenAI Chat Completions API，支持流式和非流式输出
- 🌐 **Web 交互界面**：基于 Gradio 的交互式聊天机器人
- 🛠️ **工具调用支持**：支持 Function Calling 功能
- 🤖 **多模型支持**：支持 Qwen、GLM 等多种开源模型

## 核心特性

- ✅ **OpenAI API 格式兼容** (`/v1/chat/completions`) - 无缝替换 OpenAI 接口
- ✅ **流式输出支持** (SSE) - 实时响应体验
- ✅ **工具调用支持** (Function Calling) - 扩展模型能力
- ✅ **Web 聊天界面** (Gradio) - 零代码部署体验
- ✅ **命令行客户端示例** - 快速集成开发
- ✅ **GPU 加速推理** - 高性能模型运行
- ✅ **自动内存清理** - 优化资源占用

## 环境要求

- Python 3.8+
- CUDA 11.8+ (GPU 支持)
- 8GB+ RAM
- 支持 CUDA 的 NVIDIA GPU

## 安装

### 1. 克隆仓库

```bash
git clone https://github.com/LZKKKkk-qino/local-llm-server
cd employment
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

或手动安装：

```bash
pip install torch transformers vllm fastapi uvicorn gradio openai sse-starlette pydantic
```

### 3. 下载模型

从 HuggingFace 下载预训练模型：

```bash
# Qwen 模型
pip install modelscope
python -c "from modelscope import snapshot_download; snapshot_download('Qwen/Qwen2.5-0.5B-Instruct', cache_dir='./models')"

# 或使用 huggingface-cli
huggingface-cli download Qwen/Qwen2.5-0.5B-Instruct --local-dir ./models/Qwen2.5-0.5B-Instruct
```

## 使用方法

### 启动 API 服务器

```bash
# 设置模型路径
export MODEL_PATH="/path/to/your/model"

# 启动服务
python scripts/model_api_server.py $MODEL_PATH
```

服务将在 `http://0.0.0.0:6006` 启动

### 使用 Web 聊天界面

```bash
export MODEL_PATH="/path/to/your/model"
python scripts/chat_robot.py
```

浏览器将自动打开 `http://127.0.0.1:6006`

## 为什么选择本地私有化部署？

| 对比项 | 云端 API (OpenAI/通义等) | 本地私有部署 |
|--------|------------------------|-------------|
| 数据隐私 | 数据需上传到第三方服务器 | 🔒 数据完全本地处理 |
| 持续成本 | 按使用量计费，长期成本高 | 💰 一次性硬件投入，无 API 费用 |
| 网络依赖 | 需要稳定网络连接 | 🌐 内网运行，无网络依赖 |
| 模型定制 | 受限于提供商 | 🛠️ 自由切换/微调模型 |
| 并发限制 | 受限于提供商额度 | 🚀 自由扩展并发能力 |

### OpenAI 兼容 API 调用示例

无需修改现有代码，只需更改 `base_url` 即可替换 OpenAI 接口：

```python
from openai import OpenAI

client = OpenAI(api_key="EMPTY", base_url="http://127.0.0.1:6006/v1/")

response = client.chat.completions.create(
    model="Qwen2.5-0.5B-Instruct",
    messages=[
        {"role": "system", "content": "你是一个乐于助人的AI助手。"},
        {"role": "user", "content": "请介绍一下你自己。"}
    ],
    max_tokens=256,
    temperature=0.7
)

print(response.choices[0].message.content)
```

### 流式输出示例

```python
from openai import OpenAI

client = OpenAI(api_key="EMPTY", base_url="http://127.0.0.1:6006/v1/")

stream = client.chat.completions.create(
    model="Qwen2.5-0.5B-Instruct",
    messages=[{"role": "user", "content": "写一首关于春天的诗"}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

## OpenAI API 兼容性

本项目实现了完整的 OpenAI Chat Completions API 规范，包括：

- ✅ 请求/响应格式完全兼容
- ✅ 支持流式输出 (Server-Sent Events)
- ✅ 支持 Function Calling
- ✅ 支持所有标准参数 (temperature, top_p, max_tokens, etc.)

**这意味着您可以直接替换 OpenAI 的 `base_url`，无需修改现有代码！**

## API 端点

### `GET /health`
健康检查端点

### `GET /v1/models`
获取可用模型列表

### `POST /v1/chat/completions`
聊天补全接口，支持以下参数：

| 参数 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| model | string | 必填 | 模型名称 |
| messages | array | 必填 | 消息列表 |
| temperature | float | 0.8 | 采样温度 |
| top_p | float | 0.8 | Top-P 采样 |
| max_tokens | int | 1024 | 最大生成token数 |
| stream | bool | False | 是否流式输出 |
| tools | array | None | 工具列表 |
| tool_choice | string/dict | "none" | 工具选择策略 |
| repetition_penalty | float | 1.1 | 重复惩罚 |

## 项目结构

```
employment/
├── scripts/
│   ├── chat_robot.py          # Gradio Web 聊天界面
│   ├── cli.py                 # 命令行客户端示例
│   ├── model_api_server.py    # API 服务器主程序
│   ├── server_run.py          # 服务器启动脚本
│   ├── test1.py               # 异步流测试
│   ├── test2.py               # ID 生成测试
│   └── test3.py               # Pydantic 模型测试
├── README.md
└── LICENSE
```

## 支持的模型

- Qwen/Qwen2.5-0.5B-Instruct
- Qwen/Qwen2.5-7B-Instruct
- Qwen/Qwen3-8B
- THUDM/glm-4-9b-chat
- 其他支持 transformers 的模型

## 配置参数

在 `model_api_server.py` 中可以配置以下参数：

```python
MAX_MODEL_LENGTH = 4096      # 最大上下文长度
gpu_memory_utilization = 0.6  # GPU 内存使用率
max_num_seqs = 32            # 最大并发请求数
```

## 常见问题

### CUDA 相关错误

确保 CUDA 路径正确设置：

```python
os.environ['CUDA_PATH'] = r"D:\CUDA"  # Windows
# 或
os.environ['CUDA_HOME'] = '/usr/local/cuda'  # Linux
```

### 内存不足

减小 `gpu_memory_utilization` 或 `max_num_seqs` 参数：

```python
engine_args = AsyncEngineArgs(
    ...
    gpu_memory_utilization=0.4,
    max_num_seqs=16
)
```

### Windows 下性能问题

设置 `enforce_eager=True` 以避免 CUDA 图优化问题。

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。

## 贡献

欢迎提交 Issue 和 Pull Request！

## 致谢

- [Qwen](https://github.com/QwenLM/Qwen) - 通义千问开源模型
- [vLLM](https://github.com/vllm-project/vllm) - 高性能 LLM 推理引擎
- [FastAPI](https://github.com/tiangolo/fastapi) - 现代化的 Web 框架
- [Gradio](https://github.com/gradio-app/gradio) - ML 演示界面

## 联系方式

如有问题或建议，欢迎通过以下方式联系：

- GitHub Issues: [https://github.com/LZKKKkk-qino/local-llm-server/issues](https://github.com/your-username/employment/issues)

---

**免责声明**: 本项目仅供学习和研究使用，请遵守相关模型的使用许可协议。