# 避坑指南与国内网络适配方案

> 新手最容易踩的坑，提前避开少走弯路

---

## 一、捆绑软件避坑经验

### 1. 官网下载认准官方域名

⚠️ **很多第三方下载站捆绑垃圾软件，一定要从官网下载！**

| 工具 | 正确官网 | 注意 |
|------|---------|------|
| Claude Code | code.claude.com / claude.ai | 不要搜「克劳德下载」找第三方 |
| ChatGPT | chatgpt.com / openai.com | 国内很多仿冒站 |
| ComfyUI | comfy.org / github.com/comfyanonymous/ComfyUI | 百度搜出来的很多是改版 |
| Node.js | nodejs.org | 认准 Node.js Foundation |
| Python | python.org | 不要下「python中文版」之类的 |
| Git | git-scm.com | 官方域名 |

### 2. 安装时注意取消勾选

Windows 安装尤其注意：
- ❌ 取消勾选「安装浏览器工具栏」
- ❌ 取消勾选「设置 XX 为默认主页」
- ❌ 取消勾选「安装 XX 安全卫士」
- ❌ 取消勾选「加入用户体验计划」（可选）
- ✅ 勾选「添加到 PATH」「自动更新」等有用选项

### 3. 不要从「软件管家」「应用商店」装开发工具

- 版本普遍滞后好几个版本
- 可能被修改过，带捆绑
- 开发工具一律官网或官方包管理器安装

### 4. Homebrew / WinGet 是最干净的安装方式

- **macOS**：优先用 Homebrew，一条命令装完，干净无捆绑
- **Windows**：优先用 WinGet（系统自带），官方源放心

---

## 二、国内网络适配完整方案

### 1. npm 淘宝镜像（Node.js 工具必备）

```bash
# 设置淘宝镜像
npm config set registry https://registry.npmmirror.com

# 查看当前源
npm config get registry

# 切回官方源（需要时）
npm config set registry https://registry.npmjs.org
```

**nrm 工具（多源切换更方便）：**
```bash
npm install -g nrm
nrm ls          # 列出可用源
nrm use taobao  # 切换淘宝源
```

---

### 2. pip 清华镜像（Python 工具必备）

```bash
# 临时使用
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 包名

# 永久设置
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

其他可选镜像：
- 阿里云：https://mirrors.aliyun.com/pypi/simple
- 中科大：https://pypi.mirrors.ustc.edu.cn/simple

---

### 3. GitHub 下载加速

**方法1：ghproxy 代理（最常用）**
在 GitHub 下载链接前加上：
```
https://ghproxy.com/
```
例：`https://ghproxy.com/https://github.com/xxx/xxx/releases/download/v1.0/file.zip`

**方法2：GitHub 镜像站**
- `https://ghps.cc/`
- `https://mirror.ghproxy.com/`

**方法3：Git clone 加速**
```bash
# 克隆时用镜像
git clone https://ghproxy.com/https://github.com/xxx/xxx.git
```

---

### 4. Hugging Face 模型下载加速

```bash
# macOS/Linux 设置环境变量
export HF_ENDPOINT=https://hf-mirror.com

# Windows PowerShell
$env:HF_ENDPOINT = "https://hf-mirror.com"
```

设置后 `huggingface-cli` 下载模型自动走镜像

---

### 5. Homebrew 国内镜像（macOS）

**中科大镜像：**
```bash
# brew.git 镜像
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# homebrew-core.git 镜像
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# homebrew-cask.git 镜像
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

# 二进制预编译包镜像
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

---

### 6. Conda 清华镜像（Python 数据科学）

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

---

## 三、新手避坑 Top 10

### 坑 1：Python 版本装太高了
❌ 不要装 Python 3.12+
✅ AI 工具普遍兼容 3.10 和 3.11，推荐 3.10.11

### 坑 2：安装路径有中文
❌ 不要放「D:\工具\AI绘画\ComfyUI」这种路径
✅ 全英文路径，比如 `D:\AI\ComfyUI`

### 坑 3：用 sudo npm 全局安装
❌ `sudo npm install -g xxx` 会搞乱权限
✅ 用 nvm 管理 Node 版本，或修改 npm 全局目录到用户目录

### 坑 4：装完不重启终端
❌ 装完直接输命令，提示 command not found
✅ 关闭终端重新打开，或 source 一下配置文件

### 坑 5：C 盘 / 系统盘空间不足
❌ 模型全塞 C 盘，很快爆满
✅ 大模型、数据集放非系统盘，可通过软链接或配置修改路径

### 坑 6：盲目跟着旧教程装
❌ 2023 年的教程可能已经过时了
✅ 优先看官方文档，教程注意看发布时间

### 坑 7：一个工具装好几个版本
❌ 同时有系统 Python、conda Python、官网 Python，混乱
✅ 用版本管理工具（nvm、conda、pyenv）统一管理

### 坑 8：下载模型下到一半断了
❌ 浏览器直接下载大文件容易断
✅ 用支持断点续传的下载工具，或命令行 wget/curl -C

### 坑 9：Windows 用户名是中文
❌ 很多 AI 工具不兼容中文用户名路径
✅ 新建英文用户名，或把工具装到其他盘纯英文路径

### 坑 10：Mac 新机器没装命令行工具
❌ 终端里 git、make 等命令都没有
✅ 先执行 `xcode-select --install` 装 Xcode 命令行工具

---

## 四、不同系统注意事项

### macOS 专属注意
1. **首次打开第三方应用**：右键 → 打开，不要双击
2. **M1/M2/M3 芯片**：注意区分 Apple Silicon 和 Intel 版本，优先选 arm64
3. **Homebrew 路径**：Intel 是 `/usr/local`，Apple Silicon 是 `/opt/homebrew`
4. **权限问题**：macOS 隐私设置里要给终端「完全磁盘访问权限」

### Windows 专属注意
1. **一定要以管理员身份运行**安装和首次启动
2. **Windows Defender** 可能误删 AI 工具文件，记得加白名单
3. **PATH 长度限制**：装太多工具可能超出，可在注册表修改
4. **WSL2**：想跑 Linux 环境的 AI 工具可以用，性能接近原生

### Linux 专属注意
1. **不同发行版包管理器不同**：Debian/Ubuntu 用 apt，Fedora 用 dnf，Arch 用 pacman
2. **NVIDIA 驱动**：务必装闭源官方驱动，开源 nouveau 跑不了 CUDA
3. **CUDA + cuDNN**：版本要对应，差一个小版本都可能用不了

---

## 五、国内可用的替代方案

如果实在装不上 / 用不了海外工具，可以考虑：

| 海外工具 | 国产替代 | 说明 |
|---------|---------|------|
| ChatGPT | 豆包 / Kimi / DeepSeek | 中文体验更好，国内直接用 |
| Claude Code | 豆包 MarsCode / 通义灵码 | 国内 AI 编码工具 |
| Midjourney | 即梦 / 可灵 / 通义万相 | 中文提示词，出图质量高 |
| Suno | 天工 SkyMusic / 豆包音乐 | 中文歌生成 |
| GitHub | Gitee / GitCode | 国内代码托管，访问快 |
| Hugging Face | ModelScope（魔搭） | 阿里的模型社区，国内下载快 |
