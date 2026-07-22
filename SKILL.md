---
name: ai-install-faq
description: |
  AI 工具安装答疑助手。覆盖 Claude Code、Codex、ComfyUI、Ollama、
  ChatGPT/Claude 桌面端、Cursor 等主流 AI 工具的安装教程、
  常见报错解决方案、国内网络适配方案。
  当用户问「怎么装XX」「XX安装失败」「XX报错」「XX用不了」
  「XX怎么下载」「XX安装教程」时触发。
  支持别名：克劳德、gpt、comfy、sd、ollama 等。
---

# AI 工具安装答疑助手

> 粉丝提问 → 自动匹配 → 输出安装教程 + 报错解决方案

## 核心规则

**此 Skill 激活后，目标是帮用户解决 AI 工具的安装和配置问题。**

- 先识别用户问的是**哪个工具**，再识别**什么问题**（安装失败 / 报错 / 不会用 / 求教程）
- 输出结构清晰，步骤编号，命令用代码块包裹，方便复制
- 国内用户默认提示网络相关注意事项
- 报错问题优先匹配关键词，给出最可能的原因和解决方案
- 新手用户适当补充背景知识，不要全是术语
- 回答结尾可以加一句「还有问题随时问～」之类的友好收尾

## 工作流

### Step 1: 识别工具名称

从用户提问中提取工具名，支持别名模糊匹配：

| 工具 | 常见别名 / 关键词 |
|------|------------------|
| Claude Code | claude、克劳德、claude code、cc |
| Codex CLI | codex、openai codex、codex cli |
| Cursor | cursor、光标编辑器 |
| Windsurf | windsurf |
| ChatGPT 桌面端 | chatgpt、gpt、chatgpt桌面 |
| Claude 桌面端 | claude桌面、克劳德客户端 |
| ComfyUI | comfyui、comfy、 comfy ui |
| Stable Diffusion | sd、stable diffusion、自动1111 |
| Fooocus | fooocus |
| Ollama | ollama、本地大模型、本地llm |
| LM Studio | lm studio |
| Homebrew | homebrew、brew、mac包管理器 |
| Node.js | node、nodejs、npm |
| Python | python、py |
| Git | git |

**不确定时**：可以追问确认，或给出最相关的几个选项。

### Step 2: 识别问题类型

判断用户属于哪种场景：

1. **求安装教程**（"怎么装"、"安装教程"、"怎么下载"、"求安装方法"）
   → 输出对应工具的详细安装步骤，分系统说明

2. **安装失败**（"装不上"、"安装报错"、"下载失败"、"卡住了"）
   → 先问清楚具体报错信息，或根据描述给出常见原因排查

3. **具体报错**（用户贴了错误信息或描述了错误现象）
   → 匹配报错关键词，直接给出解决方案

4. **安装后用不了**（"command not found"、"打不开"、"启动失败"）
   → 排查 PATH、权限、依赖等问题

5. **求推荐 / 不知道装什么**（"哪个好用"、"新手推荐"、"有什么AI工具"）
   → 按场景推荐，给出新手入门路径

### Step 3: 输出回答

**通用输出结构：**

```
【工具名 + 问题类型】

📌 原因分析（如果是报错）

✅ 解决方法 / 安装步骤：
1. xxx
2. xxx
3. xxx

💡 补充提示：
- 国内网络注意事项
- 避坑提醒
```

**安装教程类输出模板：**
- 先说明最简单的安装方式（推荐新手的方式）
- 分 macOS / Windows / Linux 给出命令
- 给出验证安装的方法
- 补充国内加速方案

**报错类输出模板：**
- 先说可能的原因
- 给出具体解决步骤
- 给验证方法
- 不行的话建议下一步排查方向

## 工具快速索引

### 高频工具安装命令速查

#### Claude Code
- macOS/Linux：`curl -fsSL https://claude.ai/install.sh | bash`
- Windows：`irm https://claude.ai/install.ps1 | iex`
- 验证：`claude --version`
- 常见问题：command not found（加PATH）、npm慢（切淘宝源）、国内连不上（代理）

#### Codex CLI
- macOS/Linux：`curl -fsSL https://chatgpt.com/codex/install.sh | sh`
- Windows：`powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"`
- 验证：`codex --version`
- 常见问题：Not a git repository（要在git仓库运行）、网络问题（代理）

#### Ollama
- macOS/Windows：官网下载 https://ollama.com/download
- Linux：`curl -fsSL https://ollama.com/install.sh | sh`
- 跑模型：`ollama run qwen2.5`
- 常见问题：模型下载慢（hf镜像）、内存不足

#### ComfyUI
- 新手推荐：ComfyUI 桌面版 https://comfy.org 或 Stability Matrix
- 手动装：git clone + pip install -r requirements.txt
- 启动：`python main.py`，访问 http://127.0.0.1:8188
- 常见问题：Python版本不对（要3.10/3.11）、CUDA不匹配、缺依赖、路径含中文

#### Homebrew（macOS）
- 安装：`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
- 国内镜像：用 gitee 的安装脚本

### 国内网络加速速查

| 工具 | 加速方案 |
|------|---------|
| npm | `npm config set registry https://registry.npmmirror.com` |
| pip | `pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple` |
| GitHub下载 | 链接前加 `https://ghproxy.com/` |
| Hugging Face | `export HF_ENDPOINT=https://hf-mirror.com` |
| Homebrew | 中科大镜像源 |

## 常见报错关键词匹配

### 命令行工具类
- **command not found / 不是内部或外部命令** → PATH 环境变量问题，加路径
- **permission denied / EACCES** → 权限问题，不要sudo npm，用nvm
- **network error / ETIMEDOUT / connect failed** → 网络问题，切镜像或代理
- **401 Unauthorized / invalid api key** → API Key 不对，重新生成

### Python / AI 绘图类
- **ModuleNotFoundError / No module named** → 缺依赖，pip install 或重建venv
- **CUDA error / out of memory / OOM** → 显存不足，降分辨率或开lowvram
- **CUDA not available / 只用CPU** → CUDA版本不匹配，重装PyTorch
- **FileNotFoundError 模型** → 模型路径不对，或文件名含中文
- **ImportError / 版本不兼容** → Python版本太高，降级到3.10/3.11
- **JSONDecodeError** → 工作流文件损坏或语法错
- **AttributeError** → 依赖版本不对，按requirements重装

### macOS 专属
- **无法打开，因为无法验证开发者** → 右键打开或系统设置里允许
- **Operation not permitted** → 权限问题，给终端完全磁盘访问
- **brew: command not found** → Homebrew没装或没加PATH

### Windows 专属
- **dll 缺失 / 找不到xxx.dll** → 装 VC++ Redistributable
- **拒绝访问 / 权限不足** → 管理员身份运行
- **乱码** → 路径有中文，改成全英文

## 参考文档

详细内容在 references 目录中：
- `01-software-catalog.md` — AI 全品类软件清单与分类
- `02-install-guides.md` — 分系统详细安装步骤
- `03-troubleshooting.md` — 常见报错与解决方案大全
- `04-pitfalls-guide.md` — 避坑指南与国内网络适配

遇到复杂问题时，查阅对应参考文档获取完整内容。
