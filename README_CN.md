<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/Letta-logo-RGB_GreyonTransparent_cropped_small.png">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/Letta-logo-RGB_OffBlackonTransparent_cropped_small.png">
    <img alt="Letta logo" src="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/Letta-logo-RGB_GreyonOffBlack_cropped_small.png" width="500">
  </picture>
</p>

<div align="center">
<h1>Letta（前身为MemGPT）</h1>

**☄️ 新版本发布：Letta代理开发环境（_阅读更多[这里](#-access-the-ade-agent-development-environment)_） ☄️**

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot.png">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot_light.png">
    <img alt="Letta logo" src="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot.png" width="800">
  </picture>
</p>


<h3>

[主页](https://letta.com) // [文档](https://docs.letta.com) // [ADE](https://docs.letta.com/agent-development-environment) // [Letta Cloud](https://forms.letta.com/early-access)

</h3>

**👾 Letta** 是一个开源框架，用于构建有状态的LLM应用程序。您可以使用Letta构建具有高级推理能力和透明长期记忆的**有状态代理**。Letta框架是白盒且与模型无关的。

[![Discord](https://img.shields.io/discord/1161736243340640419?label=Discord&logo=discord&logoColor=5865F2&style=flat-square&color=5865F2)](https://discord.gg/letta)
[![Twitter Follow](https://img.shields.io/badge/Follow-%40Letta__AI-1DA1F2?style=flat-square&logo=x&logoColor=white)](https://twitter.com/Letta_AI)
[![arxiv 2310.08560](https://img.shields.io/badge/Research-2310.08560-B31B1B?logo=arxiv&style=flat-square)](https://arxiv.org/abs/2310.08560)

[![Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-silver?style=flat-square)](LICENSE)
[![Release](https://img.shields.io/github/v/release/cpacker/MemGPT?style=flat-square&label=Release&color=limegreen)](https://github.com/cpacker/MemGPT/releases)
[![Docker](https://img.shields.io/docker/v/letta/letta?style=flat-square&logo=docker&label=Docker&color=0db7ed)](https://hub.docker.com/r/letta/letta)
[![GitHub](https://img.shields.io/github/stars/cpacker/MemGPT?style=flat-square&logo=github&label=Stars&color=gold)](https://github.com/cpacker/MemGPT)

<a href="https://trendshift.io/repositories/3612" target="_blank"><img src="https://trendshift.io/api/badge/repositories/3612" alt="cpacker%2FMemGPT | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>

</div>

> [!IMPORTANT]
> **在寻找MemGPT？** 您来对地方了！
>
> MemGPT包和Docker镜像已重命名为`letta`，以澄清MemGPT *代理* 和Letta API *服务器* / *运行时*（将LLM代理作为*服务*运行）之间的区别。阅读更多关于MemGPT和Letta之间的关系[这里](https://www.letta.com/blog/memgpt-and-letta)。

---

## ⚡ 快速入门

_推荐使用Docker来运行Letta。要安装Docker，请参阅[Docker的安装指南](https://docs.docker.com/get-docker/)。如果在安装Docker时遇到问题，请参阅[Docker的故障排除指南](https://docs.docker.com/desktop/troubleshoot-and-support/troubleshoot/)。您也可以使用`pip`安装Letta（请参阅[下面的说明](#-quickstart-pip)）。_

### 🌖 运行Letta服务器

> [!NOTE]
> Letta代理存在于Letta服务器中，服务器将它们持久化到数据库中。您可以通过[REST API](https://docs.letta.com/api-reference) + Python / Typescript SDKs以及[代理开发环境](https://app.letta.com)（图形界面）与Letta服务器中的Letta代理进行交互。

Letta服务器可以连接到各种LLM API后端（[OpenAI](https://docs.letta.com/models/openai)、[Anthropic](https://docs.letta.com/models/anthropic)、[vLLM](https://docs.letta.com/models/vllm)、[Ollama](https://docs.letta.com/models/ollama)等）。要启用对这些LLM API提供商的访问，请在使用`docker run`时设置适当的环境变量：
```sh
# 将`~/.letta/.persist/pgdata`替换为您希望存储代理数据的位置
docker run \
  -v ~/.letta/.persist/pgdata:/var/lib/postgresql/data \
  -p 8283:8283 \
  -e OPENAI_API_KEY="您的OpenAI API密钥" \
  letta/letta:latest
```

如果您有许多不同的LLM API密钥，您也可以设置一个`.env`文件并将其传递给`docker run`：
```sh
# 使用.env文件而不是传递环境变量
docker run \
  -v ~/.letta/.persist/pgdata:/var/lib/postgresql/data \
  -p 8283:8283 \
  --env-file .env \
  letta/letta:latest
```

一旦Letta服务器运行，您可以通过端口`8283`访问它（例如，向`http://localhost:8283/v1`发送REST API请求）。您还可以将服务器连接到Letta ADE，以在Web界面中访问和管理您的代理。

### 👾 访问ADE（代理开发环境）

> [!NOTE]
> 要了解ADE的导览，请观看我们在[YouTube上的ADE演示](https://www.youtube.com/watch?v=OzSCFR0Lp5s)，或阅读我们的[博客文章](https://www.letta.com/blog/introducing-the-agent-development-environment)和[开发者文档](https://docs.letta.com/agent-development-environment)。

Letta ADE是一个图形用户界面，用于创建、部署、交互和观察您的Letta代理。例如，如果您正在运行Letta服务器来为终端用户应用程序（如客户支持聊天机器人）提供支持，您可以使用ADE来测试、调试和观察服务器中的代理。您还可以使用ADE作为与Letta代理交互的通用聊天界面。

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot.png">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot_light.png">
    <img alt="ADE screenshot" src="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot.png" width="800">
  </picture>
</p>

ADE可以连接到自托管的Letta服务器（例如，在您的笔记本电脑上运行的Letta服务器）以及Letta Cloud服务。当连接到自托管/私有服务器时，ADE使用Letta REST API与您的服务器通信。

#### 🖥️ 将ADE连接到您的本地Letta服务器
要将ADE与您的本地Letta服务器连接，只需：
1. 启动您的Letta服务器（`docker run ...`）
2. 访问[https://app.letta.com](https://app.letta.com)，您将在左侧面板中看到“本地服务器”选项

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot_agents.png">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot_agents_light.png">
    <img alt="Letta logo" src="https://raw.githubusercontent.com/letta-ai/letta/refs/heads/main/assets/example_ade_screenshot_agents.png" width="800">
  </picture>
</p>

🔐 要为您的服务器设置密码保护，请在`docker run`命令中包含`SECURE=true`和`LETTA_SERVER_PASSWORD=您的密码`：
```sh
# 如果未设置LETTA_SERVER_PASSWORD，服务器将自动生成一个密码
docker run \
  -v ~/.letta/.persist/pgdata:/var/lib/postgresql/data \
  -p 8283:8283 \
  --env-file .env \
  -e SECURE=true \
  -e LETTA_SERVER_PASSWORD=您的密码 \
  letta/letta:latest
```

#### 🌐 将ADE连接到外部（自托管）Letta服务器
如果您的Letta服务器未在`localhost`上运行（例如，您将其部署在EC2等外部服务上）：
1. 点击“添加远程服务器”
2. 输入您希望的服务器名称、服务器的IP地址和服务器密码（如果已设置）

---

## 🧑‍🚀 常见问题解答（FAQ）

> _“我需要安装Docker才能使用Letta吗？”_

不需要，您可以使用`pip`（通过`pip install -U letta`）以及从源代码（通过`poetry install`）安装Letta。请参阅下面的说明。

> _“使用`pip`安装和使用`Docker`安装有什么区别？”_

Letta通过将所有代理数据存储在数据库中来为您的代理提供持久性（它们将无限期地存在）。Letta设计为与[PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL)（世界上最流行的数据库）一起使用，但是无法通过`pip`安装PostgreSQL，因此`pip`安装的Letta默认使用[SQLite](https://www.sqlite.org/)。如果您在自己的计算机上运行PostgreSQL实例，您仍然可以通过设置环境变量`LETTA_PG_URI`将Letta（通过`pip`安装）连接到PostgreSQL。

**在使用SQLite时，Letta的数据库迁移不受官方支持**，因此如果您希望确保能够升级到最新版本的Letta并迁移Letta代理数据，请确保使用PostgreSQL作为Letta的数据库后端。完整的兼容性表如下：

| 安装方法 | 启动服务器命令 | 数据库后端 | 支持数据迁移？ |
|---|---|---|---|
| `pip install letta` | `letta server` | SQLite | ❌ |
| `pip install letta` | `export LETTA_PG_URI=...` + `letta server` | PostgreSQL | ✅ |
| *[安装Docker](https://www.docker.com/get-started/)* | `docker run ...` ([完整命令](#-run-the-letta-server)) | PostgreSQL | ✅ |

> _“如何在本地使用ADE？”_

要将ADE连接到您的本地Letta服务器，只需运行您的Letta服务器（确保您可以访问`localhost:8283`）并转到[https://app.letta.com](https://app.letta.com)。如果您想使用旧版本的ADE（在`localhost`上运行），请降级到Letta版本`<=0.5.0`。

> _“如果我将ADE连接到我的本地服务器，我的代理数据会上传到letta.com吗？”_

不会，您的Letta服务器数据库中的数据将保留在您的机器上。Letta ADE Web应用程序只是连接到您的本地Letta服务器（通过REST API），并在您的浏览器本地状态中提供一个图形界面来可视化您的本地Letta数据。

> _“我必须使用你们的ADE吗？我可以自己构建吗？”_

ADE是基于（完全开源的）Letta服务器和Letta Agents API构建的。您可以在REST API（查看[此处](https://docs.letta.com/api-reference)的文档）之上构建自己的应用程序，如ADE。

> _“我可以通过CLI与Letta代理交互吗？”_

推荐使用REST API和ADE与Letta进行交互，但您也可以通过CLI访问您的代理。

<details>
<summary>查看运行Letta CLI的说明</summary>

您可以通过Letta CLI工具（`letta run`）与您的代理聊天。如果您有一个正在运行的Letta Docker容器，您可以使用`docker exec`在容器内运行Letta CLI：
```sh
# 将`<letta_container_id>`替换为您的Letta容器的ID，通过`docker ps`找到
docker exec -it <letta_container_id> letta run
```

您还可以使用`docker ps`在命令中自动找到Letta容器的ID：
```
docker exec -it $(docker ps -q -f ancestor=letta/letta) letta run
```

在CLI工具中，您将能够创建新代理或加载现有代理：
```
🧬 创建新代理...
? 选择LLM模型：letta-free [type=openai] [ip=https://inference.memgpt.ai]
? 选择嵌入模型：letta-free [type=hugging-face] [ip=https://embeddings.memgpt.ai]
->  🤖 使用角色配置文件：'sam_pov'
->  🧑 使用人类配置文件：'basic'
->  🛠️  8个工具：send_message, pause_heartbeats, conversation_search, conversation_search_date, archival_memory_insert, archival_memory_search, core_memory_append, core_memory_replace

🎉 创建了新代理 'InspiringSpinach'

按回车键开始（将请求Letta的第一条消息）

💭 用户参与。识别为Chad。确认存在并建立联系。探索兴趣并进行有意义的讨论。
🤖 你好，Chad。很高兴认识你。我热衷于学习和探索新的想法和概念。你今天想讨论什么具体话题吗？

> 输入您的消息：我的名字是Brad，不是Chad...

💭 身份不一致。在核心记忆中反映此更改。为疏忽道歉，并纠正错误。
⚡🧠 [函数] 使用 core_memory_replace 更新记忆
         名：Chad
        → 名：Brad
💭 为识别他的名字错误道歉，并重新开始对话，这次使用正确的名字称呼他。
🤖 很抱歉疏忽了，Brad。我们重新开始。很高兴认识你，Brad。你今天想讨论什么具体话题吗？

> 输入您的消息：
```
</details>

---

## ⚡ 快速入门（pip）

> [!WARNING]
> **在使用`SQLite`时，数据库迁移不受官方支持**
>
> 当您使用`pip`安装Letta时，默认的数据库后端是`SQLite`（您仍然可以通过设置`LETTA_PG_URI`使用外部`postgres`服务与您的`pip`安装的Letta）。
>
> 我们不官方支持在使用`SQLite`后端时在Letta版本之间进行迁移，仅支持`postgres`。如果您希望在多个Letta版本中保留您的代理数据，我们强烈建议使用Docker安装方法，这是使用`postgres`与Letta的最简单方法。

<details>

<summary>查看使用pip安装的说明</summary>

您也可以使用`pip`安装Letta，这将默认使用`SQLite`作为数据库后端（而Docker将默认使用`postgres`）。

### 步骤1 - 使用`pip`安装Letta
```sh
pip install -U letta
```

### 步骤2 - 为您选择的LLM /嵌入提供商设置环境变量
```sh
export OPENAI_API_KEY=sk-...
```

对于Ollama（请参阅我们的完整[文档](https://docs.letta.com/install)，了解如何设置各种提供商的示例）：
```sh
export OLLAMA_BASE_URL=http://localhost:11434
```

### 步骤3 - 运行Letta CLI

您可以通过Letta CLI工具（`letta run`）创建代理并与它们聊天：
```sh
letta run
```
```
🧬 创建新代理...
? 选择LLM模型：letta-free [type=openai] [ip=https://inference.memgpt.ai]
? 选择嵌入模型：letta-free [type=hugging-face] [ip=https://embeddings.memgpt.ai]
->  🤖 使用角色配置文件：'sam_pov'
->  🧑 使用人类配置文件：'basic'
->  🛠️  8个工具：send_message, pause_heartbeats, conversation_search, conversation_search_date, archival_memory_insert, archival_memory_search, core_memory_append, core_memory_replace

🎉 创建了新代理 'InspiringSpinach'

按回车键开始（将请求Letta的第一条消息）

💭 用户参与。识别为Chad。确认存在并建立联系。探索兴趣并进行有意义的讨论。
🤖 你好，Chad。很高兴认识你。我热衷于学习和探索新的想法和概念。你今天想讨论什么具体话题吗？

> 输入您的消息：我的名字是Brad，不是Chad...

💭 身份不一致。在核心记忆中反映此更改。为疏忽道歉，并纠正错误。
⚡🧠 [函数] 使用 core_memory_replace 更新记忆
         名：Chad
        → 名：Brad
💭 为识别他的名字错误道歉，并重新开始对话，这次使用正确的名字称呼他。
🤖 很抱歉疏忽了，Brad。我们重新开始。很高兴认识你，Brad。你今天想讨论什么具体话题吗？

> 输入您的消息：
```

### 步骤4 - 运行Letta服务器

您可以使用`letta server`启动Letta API服务器（请参阅完整的API参考[这里](https://docs.letta.com/api-reference)）：
```sh
letta server
```
```
初始化数据库...
运行：uvicorn server:app --host localhost --port 8283
INFO:     启动服务器进程 [47750]
INFO:     等待应用程序启动。
INFO:     应用程序启动完成。
INFO:     Uvicorn运行在 http://localhost:8283 (按 CTRL+C 退出)
```
</details>

---

## 🤗 如何贡献

Letta是一个由一百多名贡献者构建的开源项目。有许多方式可以参与Letta OSS项目！

* **为项目做出贡献**：有兴趣贡献吗？请先阅读我们的[贡献指南](https://github.com/cpacker/MemGPT/tree/main/CONTRIBUTING.md)。
* **提问**：加入我们的[Discord社区](https://discord.gg/letta)，并将您的问题发送到`#support`频道。
* **报告问题或建议功能**：有问题或功能请求吗？请通过我们的[GitHub Issues页面](https://github.com/cpacker/MemGPT/issues)提交。
* **探索路线图**：对未来的发展感到好奇吗？请查看并评论我们的[项目路线图](https://github.com/cpacker/MemGPT/issues/1533)。
* **参加社区活动**：通过[活动日历](https://lu.ma/berkeley-llm-meetup)或关注我们的[Twitter账号](https://twitter.com/Letta_AI)了解最新动态。

---

***法律声明**：使用Letta和相关的Letta服务（如Letta端点或托管服务）即表示您同意我们的[隐私政策](https://www.letta.com/privacy-policy)和[服务条款](https://www.letta.com/terms-of-service)。*

---
