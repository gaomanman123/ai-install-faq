# AI 工具常见报错与解决方案大全

> 按工具分类，遇到报错直接 Ctrl+F 搜关键词

---

## 一、Claude Code 常见问题

### 1. command not found: claude（命令找不到）

**原因：** 安装成功但没加入 PATH 环境变量

**macOS / Linux 修复：**
```bash
# zsh（macOS 默认）
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**Windows 修复：**
- 右键「此电脑」→ 属性 → 高级系统设置 → 环境变量
- 用户变量 → Path → 编辑 → 新建
- 添加：`%USERPROFILE%\.local\bin`
- 或 npm 安装的话添加：`C:\Users\你的用户名\AppData\Roaming\npm`
- 确定保存，重启终端

---

### 2. npm 下载慢 / 安装中断 / 连接失败

**原因：** 国内网络访问 npm 官方源慢

**解决：切换淘宝镜像**
```bash
npm config set registry https://registry.npmmirror.com
```
然后重新安装

---

### 3. API Key 401 错误 / 认证失败

**原因：** API Key 无效或过期，或地区限制

**解决：**
1. 去 Anthropic 官网重新生成 API Key
2. 确认账号有余额
3. 国内 IP 可能被限制，需要代理
4. 或配置国内模型中转（如 CC-Switch + DeepSeek）

---

### 4. 国内官网打不开 / 注册失败 / 频繁掉线

**原因：** Claude 未对中国大陆地区开放

**解决方案：**
- 方案1：使用代理（全局/TUN 模式）
- 方案2：配置 CC-Switch，接入国内模型（DeepSeek 等）
- 方案3：使用 Claude Code 的 API 模式，走第三方中转

---

### 5. 安装后第一次启动卡住

**解决：** 运行 `claude doctor` 自动诊断
```bash
claude doctor
```

---

## 二、Codex CLI 常见问题

### 1. "Not a git repository" 报错

**原因：** Codex 强制要求在 Git 仓库中运行

**解决：**
```bash
# 方法1：进入已有 git 项目
cd your-project

# 方法2：临时目录测试
cd $(mktemp -d) && git init && codex
```

---

### 2. 网络连接失败 / 连不上 OpenAI

**方案1：配置环境变量代理**
```bash
# macOS/Linux
export https_proxy=http://127.0.0.1:7890
export http_proxy=http://127.0.0.1:7890

# Windows PowerShell
$env:https_proxy="http://127.0.0.1:7890"
$env:http_proxy="http://127.0.0.1:7890"
```

**方案2：开启代理软件的 TUN 模式（全局）**

---

### 3. 非交互模式安装卡住

**解决：** 设置环境变量跳过交互
```bash
export CODEX_NON_INTERACTIVE=1
```

---

## 三、ComfyUI / Stable Diffusion 常见报错

### 1. Python 版本不兼容 / ImportError

**现象：** 启动时报各种模块导入错误

**原因：** Python 3.12+ 很多依赖不兼容

**解决：** 安装 Python 3.10 或 3.11（推荐 3.10.11）
- 卸载当前 Python，装 3.10.x 版本
- 或用 conda 创建指定版本的虚拟环境

---

### 2. CUDA 不匹配 / GPU 不识别 / 只能用 CPU 跑

**现象：** 启动提示 "CUDA not available"，生成速度极慢

**原因：** PyTorch 编译的 CUDA 版本和显卡驱动不匹配

**排查步骤：**
```bash
# 查看显卡驱动 CUDA 版本
nvidia-smi

# 查看 PyTorch 用的 CUDA
python -c "import torch; print(torch.version.cuda)"
```

**解决：**
1. 更新 NVIDIA 显卡驱动到最新
2. 重新安装对应 CUDA 版本的 PyTorch
3. 新手推荐直接用 ComfyUI 桌面版或 Stability Matrix，自动适配

---

### 3. ModuleNotFoundError / 缺少模块 / DLL 报错

**现象：** 启动报错，提示 No module named 'xxx'

**原因：** 虚拟环境损坏，或依赖安装不全

**解决：** 重建虚拟环境
```bash
# 删除旧环境
rm -rf venv  # Windows: rmdir /s venv

# 重新创建
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 重装依赖
pip install -r requirements.txt
```

---

### 4. 模型加载失败 FileNotFoundError

**原因：**
- 模型没放到正确目录
- 文件名含中文
- 文件下载损坏

**解决：**
1. 检查目录：大模型放 `models/checkpoints/`，LoRA 放 `models/loras/`
2. 模型文件名改成英文
3. 重新下载模型文件（大文件容易下载损坏）

---

### 5. 浏览器打不开界面 / 访问不了 8188 端口

**排查：**
1. 看终端输出的地址是不是 http://127.0.0.1:8188
2. 检查防火墙有没有阻止 8188 端口
3. 换个浏览器试试
4. 确认 ComfyUI 进程还在运行（没报错退出）

---

### 6. JSONDecodeError 工作流导入失败

**现象：** 导入 json 工作流时报错

**原因：**
- 文件编码不是 UTF-8
- 手动编辑改错了（用了单引号、末尾多了逗号）
- 文件损坏

**解决：**
- 重新导出/下载工作流文件
- 不要用记事本编辑，用 VS Code 等专业编辑器
- 检查 JSON 语法（双引号、无末尾逗号）

---

### 7. AttributeError 属性错误

**现象：** `AttributeError: module 'xxx' has no attribute 'yyy'`

**原因：** 依赖库版本不对，接口不兼容

**解决：**
- 查看对应节点的 requirements.txt，按指定版本安装
- 或更新 ComfyUI 和节点到最新版

---

### 8. 显存不足 Out of Memory (OOM)

**现象：** 生成时报 CUDA out of memory

**解决：**
- 降低分辨率
- 减少 batch size
- 用更小的模型
- 开启 --lowvram 模式启动：`python main.py --lowvram`
- 关闭其他占用显存的程序

---

### 9. 路径含中文导致各种奇怪报错

**现象：** 各种莫名奇妙的报错，日志里有乱码路径

**解决：**
- ComfyUI 整个文件夹路径不要有中文
- Windows 用户名如果是中文，建议换英文路径安装
- 模型文件名也用英文

---

### 10. Windows 管理员权限问题

**现象：** 安装或启动失败，权限相关报错

**解决：** 右键安装包 / 启动脚本 → 以管理员身份运行

---

## 四、通用安装问题

### 1. 权限不足 / EACCES 错误

**macOS/Linux：**
- ❌ 不要用 `sudo npm`，会搞乱权限
- ✅ 改用 nvm 管理 Node.js，或修改 npm 全局目录

**Windows：**
- 以管理员身份运行 PowerShell / CMD

---

### 2. macOS 提示「无法打开，因为无法验证开发者」

**解决：**
- 方法1：右键应用 → 打开 → 点「打开」
- 方法2：系统设置 → 隐私与安全性 → 往下拉 → 点「仍要打开」

---

### 3. 下载慢 / 下载中断 / 连接超时

**通用方案：**
- GitHub 下载慢：链接前加 `https://ghproxy.com/`
- npm 慢：切淘宝镜像
- pip 慢：用清华镜像
- Hugging Face 模型慢：设置镜像 `export HF_ENDPOINT=https://hf-mirror.com`
- 大文件建议用下载工具（IDM、Motrix 等）支持断点续传

---

### 4. 磁盘空间不足

**提醒：**
- 本地 AI 工具本体不大（几百 MB）
- 但模型文件很大：基础大模型 2-7GB，视频模型 10GB+
- 建议预留 50GB+ 空闲空间
- 模型可以放其他盘，通过软链接或配置路径修改

---

### 5. 安装后重启终端命令就没了

**原因：** PATH 只在当前终端生效，没写入配置文件

**解决：** 把 export PATH 那行写入 `~/.zshrc`（macOS）或 `~/.bashrc`（Linux），参考上面 command not found 的修复方法
