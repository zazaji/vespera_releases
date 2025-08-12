<!-- README_zh.md -->
<div align="center">
  <img src="https://raw.githubusercontent.com/zazaji/vespera/main/src-tauri/icons/128x128@2x.png" alt="Vespera Logo" width="128">
  <h1>Vespera | 荧简</h1>
  <p><strong>你的私密、加密、云原生数字圣殿。</strong></p>
  <p>
    中文 | <a href="./README.md">English</a>
  </p>
</div>

[![Release](https://github.com/zazaji/vespera/actions/workflows/release.yml/badge.svg)](https://github.com/zazaji/vespera/actions/workflows/release.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Vespera**  Vespera | 荧简 是一款基于隐私和性能优先原则构建的、极简主义、云原生、零知识的笔记应用。

本项目为那些寻求一个真正不受侵扰的数字空间，用以思考、书写和整理思绪的人而打造。专注于绝对的隐私保护，不提供服务器，数据库、对象存储都需要你自己来实现，只在需要时临时从数据库中直接读取，离开设备就已通过加密技术进行保存，笔记在本地不落地。数据服务提供商、其他人无法读取它们，任何人都不能。

Vespera是您在静谧时分，用以记录和守护内心微光的私人简牍，构建在**由你完全掌控**的基础设施之上。

你可以在任何一台电脑，下载打开这个应用，然后输入自己的加密字符串，开始自己的笔记，不用担心文件泄漏。

Vespera | 荧简。入夜，结束了一天的繁忙，在一个属于自己的角落，打开一份承载着珍贵思想的竹简，上面有萤火般微弱而温暖的光芒，只为你一人闪耀。


---

### 💡 核心理念

Vespera 的构建基于几个核心原则：

*   🔐 **绝对隐私**: 你的数据通过只有你才知道的密钥进行端到端加密 (E2EE)，存储到S3存储也通过了加密。所有的加密和解密操作都在你的本地设备上完成。
*   ☁️ **数据归你所有**: Vespera 没有自己的服务器。它直接连接到**你个人**的、拥有免费套餐的云基础设施，例如长期免费 Cloudflare R2 (用于 S3 兼容存储) 和 Supabase (用于 PostgreSQL 数据库)。你拥有完全的控制权。
*   🚀 **现代且快速**: 基于 Rust, Tauri, Vue 3 和 TypeScript 构建，为 Windows, macOS 和 Linux 用户提供原生、快速和响应灵敏的体验。
*   👐 **开放与透明**: 客户端完全开源。你可以审查代码、自行构建，并对你运行的程序充满信心。

### ✨ 功能特性

*   **多账户支持**: 管理多组相互隔离的笔记（例如，个人、工作）。
*   **强大的 Markdown 编辑器**: 支持实时预览、分屏模式和丰富的格式化工具栏。支持latex。
*   **文件与文件夹组织**: 熟悉的树状结构，支持拖放操作。
*   **高级标签系统**: 使用标签组织笔记，并按标签筛选。
*   **颜色标记**: 为笔记和文件夹分配颜色，实现可视化管理。
*   **有限搜索**: 通过支持中日韩语言的强大搜索引擎，即时找到笔记。规定每个文件的第一行是不加密，存到数据库，通过关键词只检索这一行。可以在每个文件第一行配置自己觉得检索方便的关键词。
*   **图片上传**: 只需将图片粘贴到编辑器中，它们就会被自动上传到你的 S3 存储桶。
*   **增强安全性**:
    *   **应用锁**: 使用密码保护整个应用。
    *   **文件夹锁**: 使用独立的密码加密特定文件夹。
    *   **账户自动锁定**: 在一段时间无操作后自动锁定应用。
*   **数据可移植性**: 轻松将所有笔记导出为 JSON 文件，或从备份中导入。
*   **主题化**: 提供浅色、深色、Nord 和 Solarized 主题以适应你的风格。
*   **缺点**：涉及到本地不落地，本地没有任何存储，所有数据都是临时从对象存储加载，所以效率不及其他笔记软件。

### 🏗️ 工作原理：隐私的架构

Vespera 的安全模型旨在让你高枕无忧。

1.  **设置**: 你提供自己的 PostgreSQL 数据库（来自 Supabase）和 S3 兼容对象存储（来自 Cloudflare）的凭据。你还需要设置一个唯一的**加密密钥**。
2.  **本地存储**: 你的账户配置信息被加密后存储在你的本地机器上。你的主加密密钥永远不会以未加密的形式被存储。
3.  **数据存储**:
    *   **笔记内容**: 当你编写笔记时，内容会使用你的加密密钥在本地设备上加密，然后上传到你的 S3 存储桶。
    *   **元数据**: 文件结构、标签和笔记标题存储在你的 PostgreSQL 数据库中。这使得快速搜索和渲染目录树成为可能，而无需下载和解密每一篇笔记。
4.  **同步**: 当你在另一台设备上使用 Vespera 时，可以导出账户加密字符串复制到另一台设备（需要你设定aes密钥），它会遵循相同的流程，连接到你的云服务并在本地解密数据。


### 🚀 开始使用

要使用 Vespera，你需要设置自己的免费云后端。这大约需要 10 分钟。

#### 1. 设置 Supabase (PostgreSQL 数据库)

1.  访问 [Supabase](https://supabase.com/) 并创建一个新项目。
2.  项目就绪后，导航到 **Project Settings** > **Database**。
3.  在 **Connection string** 下，找到格式为 `postgresql://postgres:[YOUR-PASSWORD]@[HOST]:[PORT]/postgres` 的 `URI`。选择端口为5432的字符串，不要选择6543的字符串。（我曾在这儿纠结了3天没有任何进展）
4.  **这就是你的数据库 URL。** 复制它。

#### 2. 设置 Cloudflare R2 (S3 兼容存储)

1.  访问你的 [Cloudflare 仪表板](https://dash.cloudflare.com/)。
2.  导航到 **R2** > **Create bucket**。给它一个名字（例如 `vespera-notes`）。
3.  进入 R2 概览页面，点击右侧的 **Manage R2 API Tokens**。
4.  点击 **Create API Token**。
5.  给令牌一个名字，选择 **Object Read & Write** 权限，然后点击 **Create API Token**。
6.  页面会显示你的 **Access Key ID** 和 **Secret Access Key**。**请立即复制它们**，因为密钥只会显示一次。
7.  返回到你的 R2 存储桶页面。**S3 Endpoint** 会列在那里。它的格式类似于 `https://[ACCOUNT_ID].r2.cloudflarestorage.com`。

#### 3. 配置 Vespera

1.  下载并打开最新版本的 Vespera。
2.  在设置界面，输入你刚刚收集到的凭据：
    *   **Account Name**: 为这个账户起个名字（例如，“我的个人笔记”）。
    *   **Database URL**: 你的 Supabase 连接字符串。
    *   **S3 Endpoint**: 你的 Cloudflare R2 端点。
    *   **S3 Region**: 输入 `auto`。
    *   **S3 Bucket Name**: 你创建的存储桶的名称。
    *   **S3 Access Key ID / Secret Access Key**: 来自 Cloudflare 的 API 令牌凭据。
    *   **Encryption Key**: **创建一个强大且唯一的密码。** 这是你笔记的主密钥。**如果丢失，你的数据将无法恢复。**
3.  测试连接，保存，然后就可以开始使用了！

### 🛠️ 从源码构建

如果你想自己构建 Vespera：

1.  **先决条件**:
    *   Node.js 和 Yarn/npm
    *   Rust 和 Cargo
    *   适用于你操作系统的 [Tauri 先决条件](https://tauri.app/v1/guides/getting-started/prerequisites)。

2.  **克隆仓库**:
    ```sh
    git clone https://github.com/zazaji/vespera.git
    cd vespera
    ```

3.  **安装前端依赖**:
    ```sh
    npm install
    ```

4.  **在开发模式下运行**:
    ```sh
    npm run tauri dev
    ```

### ❤️ 贡献

欢迎贡献！请随时提交 issue 或 pull request。

### 📄 许可证

该项目采用 MIT 许可证。详情请见 [LICENSE](LICENSE) 文件。