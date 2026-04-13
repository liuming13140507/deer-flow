# 工作流说明

本文档记录工作流修改过程及常用命令。

## 环境配置

### Agent 安装

**当前选择：方案四**

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


### 运行
检查是否模型安装成功
```
ollama list
```
运行
```
cd /Users/liuming/proj/deer-flow
make stop
make dev
http://localhost:2026/
```