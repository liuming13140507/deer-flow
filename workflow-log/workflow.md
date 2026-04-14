# 工作流说明

本文档记录工作流修改过程及常用命令。

## 会话记录索引

| 日期 | 文件 | 主题 |
|------|------|------|
| 2026-04-14 | [session-summary-20260414-171031.md](session-summary-20260414-171031.md) | DeerFlow 框架深度分析、Agent 应用架构理解、Archon 对比 |

## 环境配置

### Agent 安装

**当前选择：方案五**

#### 方案一

第一阶段使用本地下载的 Ollama 模型调试，避免使用云端 API 产生费用。

#### 方案二

国内拉取 `docker-ollama` 较慢，改为本机安装 Ollama，但是这种安装的下载很慢，而且实际使用后不能启动GPU加速，导致模型调用长期无返回值。

```bash
ollama pull qwen2.5:7b
```

体积约 4.7 GB，首次拉取耗时较长。

#### 方案三

尝试通过包管理安装，如果要使用tools模型的模版，需要另外增加配置。模型页：[ngxson/Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF · ModelScope](https://modelscope.cn/models/ngxson/Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF)

**pip / modelscope CLI**

```bash
pip install modelscope

modelscope download --model ngxson/Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF

modelscope download --model ngxson/Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF README.md --local_dir ./dir
```

**Python 下载**

```python
from modelscope import snapshot_download

model_dir = snapshot_download("ngxson/Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF")
```

**Git LFS 克隆**

```bash
brew install git-lfs
git lfs install
git clone https://www.modelscope.cn/ngxson/Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF.git
```

跳过 LFS 大文件（仅克隆元数据）：

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://www.modelscope.cn/ngxson/Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF.git
```

本地 GGUF 路径示例（Modelfile / 自定义镜像时引用）：

```text
./Qwen2.5-7B-Instruct-1M-Q4_K_M-GGUF/qwen2.5-7b-instruct-1m-q4_k_m.gguf
```

运行本地模型名示例：

```bash
ollama run qwen2.5-7b-local
```

#### 方案四

放弃本地 Ollama 方案（Mac 上无法启用 GPU 加速，推理速度太慢），改用 Google Gemini 云端 API。

- 模型：`gemini-2.5-flash`
- SDK：`langchain_google_genai:ChatGoogleGenerativeAI`（原生 SDK）
- 配置文件：`config.yaml` models 部分
- API Key：在 `.env` 中设置 `GEMINI_API_KEY`
- 获取 Key：[Google AI Studio](https://aistudio.google.com/apikey)

### 运行

```bash
cd /Users/liuming/proj/deer-flow
make stop
make dev
```

#### 方案五
Gimini可以使用，但是因为一次请求loop会调用多次，API KEY模式计费规则不明确，请求了2次就提示超出限额，因为切换到navidia llama，只需要注册后绑定手机号就会生成API_KEY，用于dev，有效期1年，也没有地域限制

访问 [http://localhost:2026/](http://localhost:2026/)