# AI 工具详细安装步骤（分系统）

> 每个工具提供 macOS / Windows / Linux 三平台方案，优先最简单的方式

---

## 一、AI 编码 CLI 工具

### 1. Claude Code 安装教程

#### macOS / Linux（官方一键脚本，推荐）
```bash
curl -fsSL https://claude.ai/install.sh | bash
```
安装位置：`~/.local/bin/claude`

#### macOS（Homebrew 方式）
```bash
brew install --cask claude-code
```

#### Windows（PowerShell）
```powershell
irm https://claude.ai/install.ps1 | iex
```

#### npm 方式（全平台，已弃用但可用，需 Node.js ≥ 18）
```bash
npm install -g @anthropic-ai/claude-code
```

**验证安装：**
```bash
claude --version
```

**启动：** 在项目目录下输入 `claude` 回车

---

### 2. Codex CLI 安装教程

#### macOS / Linux（官方一键脚本，推荐）
```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

#### macOS（Homebrew）
```bash
brew install codex
```

#### Windows（PowerShell）
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

#### npm 方式（全平台）
```bash
npm install -g @openai/codex
```

**验证安装：**
```bash
codex --version
```

**启动：** 在 git 项目目录输入 `codex` 回车（必须是 git 仓库）

---

### 3. Cursor 编辑器安装

#### macOS
- 官网下载：https://cursor.com/download
- 或 Homebrew：`brew install --cask cursor`

#### Windows
- 官网下载 .exe 安装包
- 或 WinGet：`winget install Cursor`

#### Linux
- 官网下载 .AppImage 或 .deb

---

## 二、AI 桌面客户端

### 4. ChatGPT 桌面端

#### macOS
- 官网：https://chatgpt.com/download
- Homebrew：`brew install --cask chatgpt`

#### Windows
- 官网下载
- WinGet：`winget install OpenAI.ChatGPT`

---

### 5. Claude 桌面端

#### macOS
- 官网：https://claude.ai/download
- Homebrew：`brew install --cask claude`

#### Windows
- 官网下载 .exe

---

## 三、本地大模型工具

### 6. Ollama 安装（最简单的本地大模型方案）

#### macOS
- 官网下载：https://ollama.com/download
- 或 Homebrew：`brew install ollama`

#### Windows
- 官网下载 .exe 安装包

#### Linux
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**使用方法：**
```bash
# 拉取并运行 Qwen2.5（阿里开源中文模型，推荐）
ollama run qwen2.5

# 拉取 Llama 3.1
ollama run llama3.1

# 拉取视觉模型
ollama run llava
```

安装后浏览器访问 http://localhost:11434 可查看状态

---

### 7. LM Studio 安装（图形界面版）

- 官网：https://lmstudio.ai
- macOS / Windows / Linux 全平台桌面应用
- 打开后搜索模型 → 下载 → 一键运行，无需命令行

---

## 四、本地 AI 图像工具

### 8. ComfyUI 安装（新手推荐桌面版）

#### 方式一：ComfyUI 桌面版（最简单，推荐新手）
- 官网：https://comfy.org
- macOS / Windows 都有桌面版安装包
- 下载安装，双击即用，自带 Python 和依赖

#### 方式二：Stability Matrix 管理器（推荐）
- 官网：https://lykos.ai
- 一键安装 ComfyUI / SD WebUI / Fooocus 等
- 自动处理依赖、模型下载、版本管理
- 支持 Windows / macOS / Linux

#### 方式三：手动安装（全平台，适合进阶）

**前置条件：Python 3.10 或 3.11（不要用 3.12+）、Git**

```bash
# 1. 克隆仓库
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI

# 2. 创建虚拟环境（推荐）
python -m venv venv

# 3. 激活环境
# macOS/Linux:
source venv/bin/activate
# Windows:
# venv\Scripts\activate

# 4. 安装依赖
pip install -r requirements.txt

# 5. 启动
python main.py
```

启动后浏览器打开 http://127.0.0.1:8188

**模型下载：** 把模型文件放到 `ComfyUI/models/checkpoints/` 目录

---

### 9. Fooocus 安装

#### Windows（便携版，推荐）
- GitHub Releases 下载便携包
- 解压后双击 `run.bat` 即可

#### 手动安装（全平台）
```bash
git clone https://github.com/lllyasviel/Fooocus.git
cd Fooocus
pip install -r requirements_versions.txt
python entry_with_update.py
```

---

## 五、前置依赖工具安装

### 10. Homebrew 安装（macOS 必备）

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**国内镜像加速版：**
```bash
/bin/bash -c "$(curl -fsSL https://gitee.com/ineo6/homebrew-install/raw/master/install.sh)"
```

安装完成后按提示执行两行命令添加到 PATH

---

### 11. Node.js 安装

#### macOS（Homebrew）
```bash
brew install node
```

#### Windows
- 官网下载：https://nodejs.org
- 或 WinGet：`winget install OpenJS.NodeJS.LTS`

#### nvm 方式（推荐，多版本管理）
```bash
# macOS/Linux
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install --lts
```

---

### 12. Python 安装（注意版本！）

⚠️ **重要：本地 AI 工具推荐 Python 3.10 或 3.11，不要装 3.12+，很多依赖不兼容**

#### macOS（Homebrew）
```bash
brew install python@3.11
```

#### Windows
- 官网下载 3.10.x 或 3.11.x：https://python.org/downloads
- 安装时勾选「Add Python to PATH」

#### conda 方式（推荐数据科学/AI 场景）
- 下载 Miniconda：https://docs.conda.io/en/latest/miniconda.html

---

### 13. Git 安装

#### macOS
```bash
# 装了 Xcode 命令行工具就自带
xcode-select --install
# 或 Homebrew
brew install git
```

#### Windows
- 官网下载：https://git-scm.com/download/win
- 或 WinGet：`winget install Git.Git`

---

### 14. FFmpeg 安装

#### macOS
```bash
brew install ffmpeg
```

#### Windows
- 官网下载：https://ffmpeg.org/download.html
- 或 WinGet：`winget install Gyan.FFmpeg`

---

## 六、安装后验证清单

| 工具 | 验证命令 |
|------|---------|
| Claude Code | `claude --version` |
| Codex | `codex --version` |
| Node.js | `node --version` |
| npm | `npm --version` |
| Python | `python --version` 或 `python3 --version` |
| Git | `git --version` |
| Homebrew | `brew --version` |
| Ollama | `ollama --version` |
| FFmpeg | `ffmpeg -version` |
