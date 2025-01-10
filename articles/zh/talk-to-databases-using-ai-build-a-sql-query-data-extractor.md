```markdown
---
title: 如何使用 AI 与任何数据库对话——构建您自己的 SQL 查询数据提取器
date: 2025-01-10T14:44:05.268Z
author: Prankur Pandey
authorURL: https://www.freecodecamp.org/news/author/prankurpandeyy/
originalURL: https://www.freecodecamp.org/news/talk-to-databases-using-ai-build-a-sql-query-data-extractor/
posteditor: "66"
proofreader: ""
---

最近，我暂停写作以专注于考试。在此期间，我有了一个有趣的体验：我有机会向我的同学们解释 SQL（结构化查询语言）。在深入探索 SQL 的过程中，我遇到了一个常见的难题：编写 SQL 查询来从数据库中获取特定数据。

<!-- more -->

这激发了一个想法。如果我能构建一个工具，不必手动编写 SQL 查询就能获得数据，那将会怎样？相反，我可以用自然的英语输入，数据库会为我完成繁重的工作。

鉴于我们生活在人工智能时代，利用人工智能是实现这一愿景的唯一途径。

在本教程中，我将带您创建一个 AI 驱动的 SQL 查询数据提取器。这个工具将使您无需编写任何 SQL 代码，就能轻松从数据库中获取数据。

### **我们将涵盖的内容：**

-   [前提条件和工具][1]
    
-   [应用程序如何工作][2]？
    
-   [如何设置您的工具][3]
    
-   [如何设置数据库][4]
    
-   [应用程序的结构和特性][5]
    
-   [如何构建后端][6]
    
-   [如何构建前端][7]
    
-   [一些重要的注意事项][8]
    
-   [与数据库互动][9]
    
-   [结论][10]
    

## 前提条件和工具

在本教程中，我们将构建一个 AI 驱动的 SQL 查询数据提取器工具。它将允许我们使用自然语言（如简单的英语）与数据库交互，并获得与编写 SQL 查询相同的结果。

以下是我们将使用的工具概述，用于创建这个酷炫的应用程序：

### 数据库

数据库是一个关键组件，我们将在其中存储数据，稍后将其提取供我们的 AI 模型在执行 NLP 操作时使用。与其在本地托管数据库，我选择一个基于云的免费数据库，允许通过 REST API 提取数据。对于这个项目，我选择了 [restdb.io][11]，因为它提供了无缝的 SQL 数据库配置，并支持 REST API。

### AI 代理

AI 代理将在数据库和 AI 模型之间充当中介。这个代理将管理 AI 模型的操作，并促进无缝通信。为此，我使用 [**CopilotKit**][12]，它简化了集成过程。

### AI（LLM）模型

AI 模型将简单英语查询转换为 SQL 查询。为此，我使用 [**GroqAI**][13]，它支持各种流行的 AI 模型，并为这个项目提供了所需的灵活性。

### Next.js

为了开发一个支持前端和后端功能的 Web 应用程序，我选择了 **Next.js**。它是构建具有服务器端渲染能力的强大且可扩展 Web 应用程序的理想框架。

### 部署

对于部署，您可以选择任何服务。我更喜欢 **Vercel**，因为它与 Next.js 无缝集成，并且对于个人项目是免费的。

通过结合这些工具，我们将构建一个功能强大且用户友好的应用程序，它能轻松桥接自然语言与 SQL 数据库。

## 我们将在这里做什么：

这些是我们将在本教程中构建我们的应用程序的步骤：

**步骤 1 – 设置数据库：**可以选择在本地设置数据库、部署并访问它，或者使用允许通过 REST API 进行数据访问和提取的在线数据库工具。

**步骤 2 – 获取云 API 密钥：**获取您的 AI 模型所需的必要 API 密钥，以实现无缝集成。

**步骤 3 – 构建一个 Web 应用：**创建一个 Web 应用程序并设置后端以集成 CopilotKit。在应用程序中进行配置以实现最佳功能。

**步骤 4 – 在数据库上训练 CopilotKit：**为 CopilotKit 提供您的数据库数据。它将读取并理解数据以促进自然语言处理。

**步骤 5 – 集成 CopilotKit 聊天：**将 CopilotKit 聊天界面添加到您的应用中，并配置它以确保平稳操作。

**步骤 6 – 本地测试：**在本地计算机上测试应用程序，以识别并修复任何问题。

**步骤 7 – 部署应用：**一旦一切按预期工作，将应用程序部署到托管平台。

## 应用程序如何工作？

您是否曾经想过，如何写简单的英语就能从 SQL 数据库中获取数据？

魔法在于 CopilotKit。它让您可以创建 AI 驱动的副驾驶员，可以在您的应用程序上执行操作。想象一下 CopilotKit 就像您的个人 AI 助手或聊天机器人。那么它如何工作？

首先，我们有一个由高级 AI 模型驱动的 CopilotKit，它作为我们的聊天机器人。

然后，当您向聊天机器人提供数据时，它会利用这些数据进行自我训练，构建对您的数据库结构和内容的理解。

最后，当输入自然语言查询（比如“谁在使用这个电子邮件地址？”）时，AI 模型会处理它，将其转换为相应的 SQL 查询，并从数据库中检索所需的数据。
```

## 如何设置你的工具

现在我们将介绍设置项目所需的一切。

### **1\. 安装 Next.js 和依赖项**：

首先，你需要创建一个 NextJS 应用程序。打开终端并运行以下命令：

```
npx create-next-app@latest my-next-app
```

将 `my-next-app` 替换为你想要的项目名称。

导航到项目文件夹：

```
cd my-next-app
```

启动开发服务器：

```
npm run dev
```

打开浏览器并访问 [`http://localhost:3000`][14]，查看你的 Next.js 应用程序的实际效果。

### **2\. 安装 CopilotKit 和依赖项**

通过终端进入项目根文件夹并运行下面的命令。它将安装所有重要的 CopilotKit 依赖项和其他重要的包，比如 dotenv 和 Axios。

```
npm install @copilotkit/react-ui @copilotkit/react-core dotenv axios
```

-   **CopilotKit** 依赖项专门用于处理 CopilotKit 的操作和配置。
    
-   **Dotenv** 依赖项用于处理环境变量，因为我们必须在项目中保留重要的密钥，如环境变量。
    
-   **Axios** 用于处理 API 调用。
    

### **3\. 设置** **数据库**

访问 [RestDB.io][15] 并登录或创建一个账户。

![restdb.io 登录页面](https://cdn.hashnode.com/res/hashnode/image/upload/v1736349488854/435a5574-54b8-40b4-a1e5-f31aa79eeae8.png)

在上面你可以看到 RestDB.io 的登录页面，如果你已有账户可以登录，或者创建一个新账户。

登录后你会被重定向到此页面。在那里你会看到一个创建新数据库的按钮。

![restdb.io 数据库创建页面](https://cdn.hashnode.com/res/hashnode/image/upload/v1736349634003/840cc3d6-c7e0-474f-9335-eca750aeacc5.png)

当你点击创建新按钮时，会弹出一个窗口。在那里，你需要输入数据库名称，如下图所示：

![restdb.io 数据库创建弹窗](https://cdn.hashnode.com/res/hashnode/image/upload/v1736349886708/c9846627-4351-40e0-a4bd-8342b6b5bf25.png)

输入数据库名称后，点击“Go”。我将 **demosql** 作为数据库名称。此时，你将获得新创建的数据库链接，如下图所示：

![restdb.io 数据库列表页面](https://cdn.hashnode.com/res/hashnode/image/upload/v1736350379651/27708c52-c8a0-405c-93d7-374833572007.png)

现在点击数据库 URL，它将引导你到此页面：

![restdb.io 主数据库页面](https://cdn.hashnode.com/res/hashnode/image/upload/v1736350576835/87abd648-1b8d-4d07-b30a-6f1076abdf06.png)

现在是为访问数据库创建 API Key 的时候了。为此，点击 **设置**，它会带你进入一个新页面，如下所示：

![restdb.io API 设置页面](https://cdn.hashnode.com/res/hashnode/image/upload/v1736352142460/d61be8ac-c78f-4c71-a1f0-dbc230496bc5.png)

在此页面上，点击 **添加新** 按钮，它会打开一个弹窗，如下图所示：

![restdb.io API Key 创建弹窗](https://cdn.hashnode.com/res/hashnode/image/upload/v1736352445417/b739b25d-e01d-4b72-b4a6-db3077866a60.png)

现在你可以在这里配置你的 API 操作，如 GET、POST、PUT 和 DELETE，将其命名为你想要的名字并保存。你的数据库现在可以通过 REST API 进行交互。

复制数据库 URL 和 API KEY 并将其放入 .env 文件。

你可以添加表，定义带有列和数据类型的模式（例如，VARCHAR、INTEGER），并手动或通过上传（Excel、CSV 或 JSON）填充数据。对于此项目，我们添加了 21 条记录。

### 4\. 为操作设置 LLM：

这一部分对项目至关重要，因为我们在设置 LLM（大语言模型）以处理将 NLP（纯英语）查询转换为 SQL 查询。

市场上有许多 LLM，每个都有其优点。一些是免费的，其他是付费的，这使得为此项目选择合适的变得具有挑战性。

经过广泛的实验，我选择了 **Groq Adapter**，因为：

-   它将各种 LLM 统一整合在一个平台下。
    
-   它通过统一的 API 密钥提供访问。
    
-   它与 CopilotKit 兼容。
    

#### 如何设置 Groq Cloud

要开始使用 Groq Cloud，[访问其网站][16]，如果已经有账户则登录，如果你是新用户则创建新账户。登录后，导航到 Groq Dashboard。

这是 Groq Cloud 的主页：

![groq cloud 主页](https://cdn.hashnode.com/res/hashnode/image/upload/v1736352733541/92012af5-b3c4-4277-a50f-834c1900a2de.png)

登录后，会打开一个新页面，如下所示：

![groq cloud 仪表盘页面](https://cdn.hashnode.com/res/hashnode/image/upload/v1736353229314/67313c60-47b8-4f23-b3c0-e46fcdd5201a.png)

如你所见，侧边栏有一个 API Keys 链接。点击它，将打开一个新页面，如下图所示。你还可以选择任何你选择的 LLM，这个选项位于查看代码选项之前的右上角。

![groqcloud API 部分](https://cdn.hashnode.com/res/hashnode/image/upload/v1736353347970/3406fa54-ddc2-4a00-8b27-22536486fc64.png)

![groq cloud api key creation page ](https://cdn.hashnode.com/res/hashnode/image/upload/v1736353563741/cd1a185a-2c77-470a-a5ce-eca564cf524a.png)

要在 Groq Cloud 上实现对各种 LLM 的无缝访问，请前往 Groq API Keys 部分生成一个 API 密钥。专为 LLM 创建一个新的 API 密钥，并确保其配置正确。

随着 LLM 的设置和所有组件的准备，您现在已准备好构建项目。

## 应用的结构和功能

我们将以一种简单直接的方式来处理这个项目，重点是简化和功能。主要目标是创建一个基础的网页，使我们能够：

- 验证我们的 API 调用是否成功。

- 查看从 API 接收到的数据。

- 与集成到前端的 CopilotKit 聊天机器人互动。

### 网页结构

由于我们已经设置好了 **Next.js 应用**，下一步是构建一个极简风格的网页，包含：

1. **头部部分：** 显示应用程序的标题。

2. **主要区域：**

   - **表格：** 显示从数据库获取的数据。

   - **状态指示器：** 显示 API 调用和数据库操作的状态。如果有任何问题，例如 API 或数据库故障，错误将以 **红色文本** 显示以便清晰呈现。

### 关键功能

- **错误处理：** 任何故障，例如 API 或数据库问题，都将以红色文本清晰标记，以便立刻可见。

- **数据呈现：** 出于展示目的，整个数据库将以结构良好的表格形式显示。

- **CopilotKit 聊天机器人集成：** 该聊天机器人将配置为允许与数据库进行自然语言交互。页面上的 **蓝色小球** 代表 **CopilotKit 聊天机器人**。这个聊天机器人是与数据库交互的关键接口。

  - 利用自然语言查询，我们可以询问有关数据库数据的问题。
  
  - 聊天机器人处理这些查询，将其转换为 SQL 查询，并无缝获取结果。

前端看起来会像这样：

![db3bd5fb-fee1-42a3-a638-b5b410c6fe69](https://cdn.hashnode.com/res/hashnode/image/upload/v1734711368585/db3bd5fb-fee1-42a3-a638-b5b410c6fe69.png)

## 如何构建后端

在开始构建后端之前，您需要将所有重要凭据放入您的 **.env** 文件中，该文件看起来像这样：

```
NEXT_PUBLIC_COPILOTKIT_BACKEND_URL=http://localhost:3000/api/copilotkit
NEXT_PUBLIC_GROQ_CLOUD_API_KEY=
NEXT_PUBLIC_RESTDB_API_KEY=
NEXT_PUBLIC_RESTDB_BASE_URL=https://demosql-fdcb.restdb.io/rest/demo-data
```

那么这些都是什么呢？让我们逐一进行说明：

1. `NEXT_PUBLIC_COPILOTKIT_BACKEND_URL=`[`http://localhost:3000/api/copilotkit`][17]: 这指定了 CopilotKit 后端 API 的基本 URL。

   - `NEXT_PUBLIC_` 前缀使得这个变量可以在服务器端和 Next.js 应用程序的客户端代码中均可访问。

   - 值 [`http://localhost:3000/api/copilotkit`][18] 表示 API 在开发过程中本地运行。

2. `NEXT_PUBLIC_GROQ_CLOUD_API_KEY=`：该变量旨在存储 GROQ Cloud 服务的 API 密钥。GROQ Cloud 可能与查询或数据处理有关，您需要粘贴自己的 Groq API 密钥。

   - 变量为空，表示 API 密钥尚未设置。在应用程序能够访问 GROQ Cloud 服务之前，可能需要用适当的值填充。

3. `NEXT_PUBLIC_RESTDB_API_KEY=`：旨在保存访问 **RESTdb** 服务的 API 密钥。您需要粘贴自己的 Groq API 密钥。

   - RESTdb 是一个提供数据库交互 API 的数据库服务。

   - 变量也为空，意味着必须用有效的 API 密钥来填充，以便应用程序能够进行身份验证并与 RESTdb 服务进行交互。

4. `NEXT_PUBLIC_RESTDB_BASE_URL=`[`https://demosql-fdcb.restdb.io/rest/demo-data`][19]：定义了与 RESTdb 数据库交互的基本 URL。当您创建数据库时，将会生成此 URL。这里，我给出了我的数据库的 URL。

   - 值 [`https://demosql-fdcb.restdb.io/rest/demo-data`][20] 指向一个名为 `demo-data` 的特定 RESTdb 数据库端点。

   - 这可能是应用程序用于获取或操作测试或开发的示例数据的端点。

我们已经成功地将环境变量添加到项目中。现在，是时候配置 CopilotKit API 后端了。

### 如何配置 CopilotKit 后端

在任何代码编辑器中打开你的 Next.js 应用程序——我更喜欢 VSCode——然后转到根文件夹，它看起来像这样：

![f338c977-02dd-4ee1-ae66-7417f03e026b](https://cdn.hashnode.com/res/hashnode/image/upload/v1734968233629/f338c977-02dd-4ee1-ae66-7417f03e026b.png)

在 app 文件夹内，创建一个名为 `api` 的新文件夹。在 API 文件夹内，再创建一个名为 `copilotkit` 的文件夹。然后在里面创建一个名为 `route.js` 的新文件，并在文件中粘贴以下代码：

```js
import {
  CopilotRuntime,
  GroqAdapter,
  copilotRuntimeNextJSAppRouterEndpoint,
} from "@copilotkit/runtime";

import Groq from "groq-sdk";

const groq = new Groq({ apiKey: process.env.NEXT_PUBLIC_GROQ_CLOUD_API_KEY });
console.log(process.env.NEXT_PUBLIC_GROQ_CLOUD_API_KEY);
const copilotKit = new CopilotRuntime();

const serviceAdapter = new GroqAdapter({
  groq,
  model: "llama-3.1-70b-versatile",
});

export const POST = async (req) => {
  const { handleRequest } = copilotRuntimeNextJSAppRouterEndpoint({
    runtime: copilotKit,
    serviceAdapter,
    endpoint: "/api/copilotkit",
  });
```


以下是每个部分的详细解释：

这段代码定义了一个使用 CopilotKit 和 Groq SDK 的 Next.js API 路由的服务器端处理程序。它设置了一个运行时环境，以处理对指定端点的请求。

**1\. 导入：**

```markdown
import {
  CopilotRuntime,
  GroqAdapter,
  copilotRuntimeNextJSAppRouterEndpoint,
} from "@copilotkit/runtime";

import Groq from "groq-sdk";
```

- `CopilotRuntime` 和 `GroqAdapter`：这些是来自 CopilotKit 库的类，用于设置和配置 AI 服务的运行时环境和适配器。
  - `CopilotRuntime`：用于管理 CopilotKit 操作的运行时环境。
  - `GroqAdapter`：将 Groq 服务（用于查询或数据处理）与 CopilotKit 连接和适配。
  
- `copilotRuntimeNextJSAppRouterEndpoint`：一个实用函数，用于为集成了 CopilotKit 的 Next.js 应用程序路由 API 端点创建处理程序。

- 来自 `"groq-sdk"` 的 `Groq`：用于与 Groq 服务交互的库，初始化用于查询或处理数据。

**2\. 初始化 Groq：**

```markdown
const groq = new Groq({ apiKey: process.env.NEXT_PUBLIC_GROQ_CLOUD_API_KEY });
console.log(process.env.NEXT_PUBLIC_GROQ_CLOUD_API_KEY);
```

- `Groq` 初始化：
  - 创建带有从环境变量中获取的 API 密钥（`NEXT_PUBLIC_GROQ_CLOUD_API_KEY`）的 `Groq` 对象。
  - 该密钥用于对应用程序与 Groq 云服务进行身份验证。

- `console.log(process.env.NEXT_PUBLIC_GROQ_CLOUD_API_KEY)`：将 API 密钥记录到服务器控制台。**注意：** 为了确保安全性，请避免在生产中记录敏感数据。

**3\. 初始化 CopilotKit 运行时**

```markdown
const copilotKit = new CopilotRuntime();
```

- `CopilotRuntime` 初始化：创建一个 CopilotKit 运行时环境的实例，以管理 CopilotKit 的功能和服务。

**4\. 配置服务适配器**

```markdown
const serviceAdapter = new GroqAdapter({
  groq,
  model: "llama-3.1-70b-versatile",
});
```

- `GroqAdapter`：
  - 配置一个适配器，以将 CopilotKit 与 Groq 连接。
  - `model` 参数指定使用的 AI 模型。在这里，它是 `"llama-3.1-70b-versatile"`，一个具有 700 亿参数的通用语言模型。

**5\. 导出的 POST 处理程序**

```markdown
export const POST = async (req) => {
  const { handleRequest } = copilotRuntimeNextJSAppRouterEndpoint({
    runtime: copilotKit,
    serviceAdapter,
    endpoint: "/api/copilotkit",
  });

  return handleRequest(req);
};
```

- 定义了一个用于 Next.js 应用程序路由 API 端点的 `POST` 处理程序。

- **关键组件：**

  1. `copilotRuntimeNextJSAppRouterEndpoint`：
     - 为 `/api/copilotkit` 端点设置处理程序。
     - 接受 `runtime`（CopilotKit）和 `serviceAdapter`（GroqAdapter）作为输入，以配置端点的行为。

  2. `handleRequest`：
     - 一个处理传入 HTTP 请求的函数（在本例中为 `POST` 请求）。
     - 这允许 CopilotKit 运行时和服务适配器动态处理请求。

- `return handleRequest(req);`：调用处理程序并处理传入请求（`req`），返回适当的响应。

工作原理：

1. Groq SDK 使用 API 密钥进行身份验证初始化。
2. 设置 CopilotKit 运行时。
3. GroqAdapter 使用指定的 AI 模型连接运行时与 Groq 服务。
4. 配置 `/api/copilotkit` 端点以处理 POST 请求，将请求传递给 CopilotKit 的运行时，并返回处理后的响应。

通过这种设置，您已将 CopilotKit 成功集成到您的 Next.js 应用程序中。后端现在是完全功能化的，可以通过 REST API 和 CopilotKit 界面实现与数据库的无缝通信。

## 如何构建前端

对于前端，我们将尽可能保持简单。我们只需要几件事情来完成这个项目：一个 Header 组件和一个 Table 组件。

1. **Header 组件**：用于显示应用程序的标题或描述。
2. **Table 组件**：用于可视化从数据库提取的数据。

为此，我们将使用 ShadCN，一个以设计干净和易用性著称的热门前端组件库。

ShadCN 提供了预构建的组件，帮助加速开发，而不损失质量。通过利用这个库，我们可以专注于功能，同时确保用户界面看起来既精致又专业。

### **如何在 Next 项目中安装 ShadCN**

运行以下命令以安装 ShadCN 组件：

```markdown
npx shadcn@latest init
```

此命令：

- 在您的项目中初始化 ShadCN。
- 创建一个用于存储 ShadCN 组件的 `components` 文件夹。
- 更新 `tailwind.config.js` 文件，以进行必要的配置。

您将被询问几个问题以配置 `components.json`：

```
Which style would you like to use? › New York
Which color would you like to use as base color? › Zinc
Do you want to use CSS variables for colors? › no / yes
add components
```

```
npx shadcn@latest add <组件名称>
```

例如，要添加一个表格组件：

```
npx shadcn@latest add table
```

`components` 文件夹现在包含一个可直接使用的 `button` 组件。

![2e5ea193-f829-435e-b4dc-68bd8ce793ca](https://cdn.hashnode.com/res/hashnode/image/upload/v1734970231792/2e5ea193-f829-435e-b4dc-68bd8ce793ca.png)

在前端，我们有一个包含 `Table` 组件的 `components` 文件夹。这个组件负责以结构化的表格格式显示数据库中的数据。

除了 `Table` 组件，前端还有两个额外的文件。这些文件用于不同的目的，将在项目后期为特定功能进行集成。

这种模块化结构确保前端保持简洁有序，使管理和拓展更加容易。

让我们来探讨每个文件：

1.  **Table.jsx:** 该文件是我们安装 Table 组件时由 ShadCN 自动生成的。它包含由 ShadCN 库提供的表格组件的默认配置。**请不要修改此文件**，因为它对组件的正常功能至关重要。
    
2.  **Tabledata.jsx:** 在这个文件中，我们通过 API 调用从数据库中获取数据并填充表格。`Tabledata.jsx` 文件弥合了后端 API 和前端表格显示之间的差距。
    

让我们仔细看看代码：

```
import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableFooter,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";

export function Tabledata({ data }) {
  return (
    <Table className="text-center">
      <TableCaption className="text-sm text-green-600 font-bold ml-8">
        来自数据库的实时数据。
      </TableCaption>
      <TableHeader>
        <TableRow className="text-center ">
          <TableHead>Id</TableHead>
          <TableHead>名称</TableHead>
          <TableHead>电子邮件</TableHead>
          <TableHead>电话号码</TableHead>
          <TableHead>地址</TableHead>
          <TableHead>城市</TableHead>
          <TableHead>省/州</TableHead>
          <TableHead>邮政编码</TableHead>
          <TableHead>国家</TableHead>
          <TableHead className="text-right">创建于</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {data.map((db) => (
          <TableRow key={db._id}>
            <TableCell className="font-medium text-wrap w-12">
              {db._id}
            </TableCell>
            <TableCell className="font-medium">{db.name}</TableCell>
            <TableCell>{db.email}</TableCell>
            <TableCell>{db.phone_number}</TableCell>
            <TableCell className="text-right">{db.address}</TableCell>
            <TableCell className="text-right">{db.city}</TableCell>{" "}
            <TableCell className="text-right">{db.state}</TableCell>
            <TableCell className="text-right">{db.zip_code}</TableCell>{" "}
            <TableCell className="text-right">{db.country}</TableCell>
            <TableCell className="text-right">{db.created_at}</TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

此代码渲染一个经过样式处理的动态表格，数据由数据库或 API 提供。

-   **导入**：使用来自 `@/components/ui/table` 的自定义表格组件（`Table`, `TableRow`, `TableCell`等）。
    
-   **属性**：接收一个 `data` 属性，这是一个对象数组，表示表格行。
    
-   **表格标题**：使用 Tailwind CSS 样式的标题 "来自数据库的实时数据"。
    
-   **表格头**：定义列头，如 `Id`、`名称`、`电子邮件` 等。
    
-   **动态行**：遍历 `data` 数组以动态生成 `TableRow` 元素，使用 `_id` 作为唯一键。
    
-   **数据单元格**：在 `TableCell` 组件中显示对象字段（`_id`, `名称`, `电子邮件`等）并应用自定义样式。
    
-   **Tailwind CSS**：应用对齐、字体粗细和间距样式。
    

### **NLQueryForm.jsx**

在这个文件中，我们处理 API 调用，定义 CopilotKit 动作，并将获取的数据传递给 Table 组件。该文件作为连接后端 API、AI 动作和前端显示的中心逻辑枢纽。

`NLQueryForm.jsx` 的关键功能：

1.  **API 集成**：从数据库中获取数据并处理错误或加载状态。
    
2.  **CopilotKit 动作**：定义允许使用自然语言查询和与数据库交互的 AI 动作。
    
3.  **数据传递**：将处理后的数据发送到 `Table` 组件进行显示。
    

以下是代码：

```
"use client";
import React, { useState, useEffect } from "react";
import { useCopilotReadable, useCopilotAction } from "@copilotkit/react-core";
import axios from "axios";
import { Tabledata } from "./Tabledata";

function NLQueryForm() {
  const [nlQuery, setNlQuery] = useState("");
  const [data, setData] = useState([]);
  console.log("🚀 ~ NLQueryForm ~ data:", data);
  const [error, setError] = useState("");
  const [loading, setLoading] = useState(true);
```

```markdown
  useCopilotReadable({
    description: "查询详细信息的数据库",
    value: JSON.stringify(data.slice(0, 25)),
  });
  useCopilotAction({
    name: "获取数据",
    description: "基于自然语言查询搜索和过滤数据",
    parameters: [
      {
        name: "nlQuery",
        type: "string",
        description: "数据库的自然语言搜索词",
        required: true,
      },
    ],

    handler: async ({ data }) => {
      setNlQuery(data);
      return JSON.stringify(data);
    },
  });

  if (loading) return <div>加载中...</div>;

  return (
    <div>
      {error && <p style={{ color: "red" }}>{error}</p>}
      <div>
        <p className="text-sm text-green-600 font-bold text-center">
          数据库实时数据。
        </p>
        <p className="text-sm text-green-600 font-bold text-center">
          总记录数: {data.length}
        </p>
        <Tabledata data={data} />
      </div>
    </div>
  );
}
export default NLQueryForm;
```

这是 `NLQueryForm` 组件的详细说明：

**引入和依赖项：**

-   使用 React 来进行状态管理 (`useState`) 和副作用 (`useEffect`)。
    
-   引入 `axios` 用于 HTTP 请求。
    
-   从 `@copilotkit/react-core` 中引入 `useCopilotReadable` 和 `useCopilotAction` 以集成 CopilotKit 功能。
    
-   引入自定义 `Tabledata` 组件以呈现数据。
    

**组件设置：**

-   定义一个函数式 React 组件 `NLQueryForm`。
    
-   初始化状态变量：
    
    -   `nlQuery`：保存自然语言查询输入。
        
    -   `data`：存储从 API 获取的数据。
        
    -   `error`：存储数据获取过程中发生的任何错误。
        
    -   `loading`：跟踪组件的加载状态。
        

**API 配置：**

-   从环境变量中获取 API 密钥和基础 URL (`NEXT_PUBLIC_RESTDB_API_KEY` 和 `NEXT_PUBLIC_RESTDB_BASE_URL`)。
    
-   使用 `console.table` 记录这些值进行调试。
    

**数据获取：**

-   使用 `useEffect` 在初始渲染时从 API 获取数据。
    
-   使用 `axios` 发起带有所需头信息的 GET 请求到 API。
    
-   使用响应更新 `data` 并停止加载状态。
    
-   通过记录错误并更新 `error` 状态来处理错误。
    

**CopilotKit 集成：**

-   `useCopilotReadable`：公开可读的描述和 `data` 的前 25 条记录。
    
-   `useCopilotAction`：定义一个名为 `fetchData` 的 CopilotKit 动作：
    
    -   接受自然语言查询 (`nlQuery`) 作为输入。
        
    -   更新 `nlQuery` 状态并返回其字符串形式。
        

**条件渲染：**

-   如果 `loading` 为 true，则显示一个加载消息（`加载中...`）。
    
-   如果发生错误，显示红色文本的错误消息。
    

**渲染：**

-   显示指示实时数据和总记录数的消息。
    
-   将 `data` 状态传递给 `Tabledata` 组件进行渲染。
    

**导出：**

-   将 `NLQueryForm` 组件导出为默认导出。

### **Page.js**

现在进入 app 文件夹内的 `page.js` 文件并添加以下代码：

```javascript
"use client";

import NLQueryForm from "@/components/ui/nl-query-form";
import { CopilotPopup } from "@copilotkit/react-ui";

export default function Home() {
  return (
    <div className="min-h-screen bg-background">
      <header className="bg-primary text-primary-foreground py-6">
        <div className="container">
          <h1 className="text-3xl font-bold">
            自然语言 SQL 查询生成器
          </h1>
        </div>
      </header>
      <main className="container py-8">
        <NLQueryForm />
      </main>

      <CopilotPopup
        instructions={
          "你正在尽力协助用户。请根据你所拥有的数据，以最佳方式回答。"
        }
        labels={{
          title: "助手弹窗",
          initial: "需要帮助吗？",
        }}
      />
    </div>
  );
}
```

这里是上面代码的简单解释：

-   **客户端渲染**：

    -   `"use client";` 指示文件正在使用 React 的客户端渲染。

-   **引入组件**：

    -   `NLQueryForm` 从本地组件目录引入以在应用中使用。
        
    -   `CopilotPopup` 从 `@copilotkit/react-ui` 包中引入以显示交互式弹窗。
        
-   **主要函数**：

    -   `Home` 是定义主页 UI 的 React 函数组件。

-   **页面布局**：

    -   使用背景颜色（`bg-background`）的全屏容器（`min-h-screen`）包装所有内容。

-   **头部**：

    -   包含标题，文本为 **"自然语言 SQL 查询生成器"**。
        
    -   使用主要背景和文本颜色（`bg-primary`，`text-primary-foreground`）进行样式设置。
        
-   **主要内容**：

    -   在带有填充（`py-8`）的容器中渲染 `NLQueryForm` 组件。

-   **弹窗组件**：

    -   在底部添加 `CopilotPopup`，包含：
        
        -   **说明**：描述助手的角色。
            
        -   **标签**：包括标题和弹窗初始消息。
            
-   **目的**：

    -   该页面旨在让用户与自然语言 SQL 查询生成器进行交互并通过弹窗获取帮助。
```

这是构建应用程序的最后一步。导航到 `layout.js` 文件并添加以下代码：

```

import "./globals.css";
import { CopilotKit } from "@copilotkit/react-core";
import "@copilotkit/react-ui/styles.css";
export const metadata = {
  title: "Create Next App",
  description: "Generated by create next app",
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <CopilotKit runtimeUrl="/api/copilotkit">{children}</CopilotKit>
      </body>
    </html>
  );
```

下面是这段代码的解释：

-   **导入：**
    
    -   `./globals.css`：导入应用程序的全局 CSS 样式。
        
    -   `@copilotkit/react-core`：导入 CopilotKit 的核心功能。
        
    -   `@copilotkit/react-ui/styles.css`：包含 CopilotKit UI 组件的预定义样式。
        
-   **元数据：**
    
    -   `metadata` 对象定义了应用的标题和描述，这对于在生成的 HTML 中设置元标记以提高 SEO 和提供用户信息非常有用。
    
-   **RootLayout 函数：**
    
    -   此函数用作应用程序的根布局包装器。它确保所有页面的结构一致并集成 CopilotKit 运行时。
    
-   **结构：**
    
    -   布局返回一个 `<html>` 元素，`lang` 属性设置为 `en` 表示英语。
        
    -   在 `<body>` 标签内部，CopilotKit 组件包装在 `children` 属性中。  
        这个设置方式：
        
        -   使用 API 端点 `/api/copilotkit` 连接应用到 CopilotKit 运行时。
            
        -   在整个应用程序中提供对 CopilotKit 功能的访问，例如处理自然语言查询。
            

## 一些重要注意事项

设计和部署数据库可以根据工具和需求采取不同的形式。对于这个项目，我选择了最简单和最易于访问的方法。

#### 为什么选择 CopilotKit？

CopilotKit 是一个强大的工具，可以将 NLP 查询转换为可执行的后端代码。如果你有类似的替代选项，可以自由使用。它弥合了自然语言输入和技术执行之间的差距，非常适合这样的项目。

#### 为什么选择 GroqCloud？

我选择了 **GroqCloud**，因为它是免费的，并且可以通过一个 API 密钥访问多个 LLM。虽然你可以选择 ChatGPT 等替代方案，但请注意它们可能需要付费计划。GroqCloud 的多功能性和经济性使其非常适合本教程。

#### 数据库考虑

你的数据库大小可以从非常小到非常大不等。然而，与数据库交互取决于你正在使用的 LLM 的令牌限制。

由于我使用的是免费工具，因此我专注于一个小型数据库以确保流畅的交互。

#### 安全最佳实践

切勿公开暴露你的凭证。始终将 API 密钥等敏感信息存储在 `.env` 文件中，以确保项目安全。

#### 未来的增强

虽然本教程主要关注设置和查询数据库，但 CopilotKit 的潜力可扩展到 **CRUD 操作**（创建、读取、更新、删除）。在我的下一个教程中，我将演示如何使用 CopilotKit 实现完整的 CRUD 操作，以创建更动态和功能更强的应用程序。

## 与数据库互动

你可以通过以下链接探索该实时项目，并提出与数据库数据相关的任何问题：[live link][22]。

为了更深入了解代码，这里是 GitHub 仓库链接：[github][23]。

此外，这里是一个展示其实际使用的截图。在这个示例中，我们没有直接编写像 `SELECT * FROM demo_data WHERE email = '`[`riverashannon@lee.com`][24]`';` 这样的 SQL 查询来提取某人的名字，而是使用了一个 NLP 查询来实现相同的结果。

![bec86e4a-bb7b-4d7f-97e9-284d54060db5](https://cdn.hashnode.com/res/hashnode/image/upload/v1735061714011/bec86e4a-bb7b-4d7f-97e9-284d54060db5.png)

## 结论

希望你喜欢构建这个简单的 AI 聊天机器人来与数据库进行交互。在这个项目中，我们使用了一个简单的 SQL 数据库，但只要能检索数据，就可以将这种方法应用到任何数据库中。

未来，我计划实现许多涉及 AI 和其他工具的新项目。AI 工具在 IT 领域真正改变了游戏规则，我期待为你提供更多关于新兴工具的详细见解和实用实现。

这就是我的分享的结尾了。如果你发现这篇文章有用，请分享并与我联系——我对机会持开放态度：

-   在 X 上关注我：[Prankur's Twitter][25]
    
-   在 LinkedIn 上关注我：[Prankur's Linkedin][26]
    
-   查看我的作品集：[Prankur's Portfolio][27]
    

[1]: #heading-prerequisiteshttpswwwfreecodecamporgnewsreact-best-practices-ever-developer-should-knowheading-prerequsites-amp-tools
[2]: #heading-how-does-the-app-work
[3]: #heading-how-to-set-up-your-tools
[4]: #heading-how-to-set-up-the-database
[5]: #heading-structure-and-features-of-the-app
[6]: #heading-how-to-build-the-back-end
[7]: #heading-how-to-build-the-front-end
[8]: #heading-some-important-notes
[9]: #heading-playing-with-the-database
[10]: #heading-conclusion
[11]: http://restdb.io
[12]: https://www.copilotkit.ai/
[13]: https://groq.com/
[14]: http://localhost:3000
[15]: http://RestDB.io
[16]: https://console.groq.com/login
[17]: http://localhost:3000/api/copilotkit
[18]: http://localhost:3000/api/copilotkit
[19]: https://demosql-fdcb.restdb.io/rest/demo-data
[20]: https://demosql-fdcb.restdb.io/rest/demo-data
[21]: http://process.env.NEXT
[22]: https://talktodb-inky.vercel.app/
[23]: https://github.com/prankurpandeyy/talktodb
[24]: mailto:riverashannon@lee.com
[25]: https://x.com/prankurpandeyy
[26]: https://linkedin.com/in/prankurpandeyy
[27]: https://prankurpandeyy.netlify.app/

