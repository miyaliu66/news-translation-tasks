---
title: 如何使用 AWS Lambda 构建应用程序
date: 2025-02-03T12:50:57.535Z
author: Ijeoma Igboagu
authorURL: https://www.freecodecamp.org/news/author/Ijay/
originalURL: https://www.freecodecamp.org/news/how-to-build-an-application-with-aws-lambda/
posteditor: ""
proofreader: ""
---

AWS Lambda 是亚马逊网络服务（AWS）的一项服务，它允许您在响应事件时运行代码，而无需管理服务器。这是一种简单且可扩展的构建应用程序的方法。

<!-- more -->

在本教程中，我将向您展示如何将 AWS Lambda 与其他三个服务结合使用：

-   **Amazon S3** 用于存储文件、图像和视频
    
-   **Amazon Simple Notification Service (SNS)** 用于发送通知
    
-   **Amazon EventBridge** 用于安排消息
    

我们将逐步进行每个步骤。

到最后，通过与其他服务的集成，您将构建一个目标实现语录应用，它会发送随机的励志信息，以保持您的动力和专注于目标。

### 前提条件

-   一个 AWS 账户：如果您还没有账户，请在[这里][1]注册。
    
-   一个 GitHub 仓库：用于存储您的源代码。如果您没有 GitHub 账户，可以在[这里][2]创建一个。
    
-   集成开发环境 (IDE)，例如 [Visual Studio Code][3] 或 [Sublime Text][4]。
    
-   基本的 Web 开发知识和您选择的任何编程语言。我在本教程中使用了 Python。
    
-   [Zenquote 随机 API][5]
    

### 您将学到什么

-   如何创建一个 Amazon S3 存储桶
    
-   如何使用 Amazon Simple Notification Service (SNS)
    
-   如何使用 Amazon Lambda
    
-   如何使用 Amazon EventBridge
    

## **目录**

1.  [步骤 1：设置您的开发环境][6]
    
2.  [步骤 2：创建 Amazon Simple Storage Service (S3)][7]
    
3.  [步骤 3：创建 Amazon Simple Notification Service (SNS)][8]
    
4.  [步骤 4：创建一个 IAM 策略][9]
    
5.  [步骤 5：创建一个 Amazon Lambda 函数][10]
    
6.  [步骤 6：创建一个 EventBridge][11]
    
7.  [步骤 7：上传您的代码][12]
    
8.  [结论][13]
    

![让我们开始 🚀](https://cdn.hashnode.com/res/hashnode/image/upload/v1736791948488/38dfe402-1050-410d-869b-0cef2797b792.png)

## 步骤 1：设置您的开发环境

在此步骤中，您将完成所有设置。首先登录到您的 AWS 帐户，然后安装 [Python][14]，如果您的 IDE 中尚未安装。

## **步骤 2：创建 Amazon Simple Storage Service (S3)**

在开始创建 S3 存储桶之前，让我们首先了解一下什么是 Amazon S3：

**Amazon S3（简单存储服务）** 是亚马逊的一项服务，它允许您在需要时存储和访问任意数量或类型的数据，如照片、视频、文档和备份。

现在您已经了解了 Amazon S3 的基础知识，让我们回到教程。

### 创建一个 S3 存储桶

有几种方法来创建 S3 存储桶，但对于本教程，我们将使用 [Ubuntu 命令行 (CMD)][15]、您的终端或 **Amazon CloudShell**，取决于您更习惯哪种方式。

-   在网络搜索栏中输入 boto3 s3 以查看相关文档列表。
    
-   点击第一个结果。
    

![常规 Google 搜索](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792137101/5f38b4ec-fa23-41b3-b108-ca7fc7b390ba.png)

-   打开文档后，复制您看到的第一个命令。

![boto3 命令](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792202800/5647c731-734f-4134-a558-9d66eee47734.png)

-   将其粘贴到您选择的 CMD 或终端中——但在此之前，请记得 "**cd**" 进入正确的目录。

![将命令从文档粘贴到您的编辑器](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792298332/d3384fc3-e31c-4d37-8e17-40ad7e77df28.png)

-   在文档中，向下滚动并点击 "create_bucket"。

![0cd59a14-b037-464b-8193-7ec515c4772e](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792399748/0cd59a14-b037-464b-8193-7ec515c4772e.png)

-   打开后，向下滚动至 "Request Syntax"。复制 **存储桶名称** 和 **存储桶配置**。
    
-   请求语法中列出的其他变量是可选的。
    

![文档中的请求语法](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792898846/eea0f8c4-d153-4bc8-8c78-346fc5bf6a04.png)

-   完成后，确保保存。

![所有命令](https://cdn.hashnode.com/res/hashnode/image/upload/v1736793004865/a1c9739c-2d12-4e2d-b057-f09bc61e16a3.png)

-   返回并调用脚本：

```
#python3 your file name
```

-   运行脚本会自动在您的 Amazon S3 中创建一个 S3 存储桶。

![自动创建](https://cdn.hashnode.com/res/hashnode/image/upload/v1736793086405/9c0b4671-ea07-4ad7-b1d7-0d785aafa954.png)

-   现在您可以到控制台查看是否已创建：

![亚马逊控制台](https://cdn.hashnode.com/res/hashnode/image/upload/v1736692453693/320318d4-bdf3-4be3-a709-6cff18459c9c.png)

### 上传文件

创建好存储桶后，我们现在可以通过控制台上传文件。我相信还有一种编程方式可以上传文件和测试，但我还没有探索文档中的所有方法。

![上传页面](https://cdn.hashnode.com/res/hashnode/image/upload/v1736692660862/355f828f-f83f-4501-a960-f0068cf9d977.png)

点击**上传按钮**上传文件。记住，我们正在创建一个目标显化引用应用程序。

现在我们已经设置了一个存储桶：

-   打开 Google Drive、MS Word、WPS 或任何其他文档编辑器。
    
-   写下你想实现的目标。
    
-   将文件保存为 PDF 或 DOCX 格式。
    
-   将文档上传到你的 Amazon S3。
    

![上传文档](https://cdn.hashnode.com/res/hashnode/image/upload/v1736693525955/765b6c3a-ae68-4cbc-9df7-e896a1d63cbb.png)

要验证是否是正确的文件：

-   导航到**权限**选项卡。
    
-   向下滚动到**阻止公共访问**。
    
-   点击**编辑**并取消选中该框。
    

![阻止访问](https://cdn.hashnode.com/res/hashnode/image/upload/v1736738796036/6ab41bc4-72a8-4874-a491-35bcdda49938.png)

如上所示，目前设置为“开”。取消选中以将其关闭。

-   在同一存储桶设置页面，修改策略。
    
-   向下滚动，你会看到一个自动生成的存储桶策略。
    
-   继续复制该策略。
    

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

-   返回存储桶策略编辑器并粘贴该策略。

完成这些步骤后，你的对象将具有公共访问权限。

返回到**对象**选项卡并点击下面提供的对象 URL：

![3b36b380-912d-4a2a-a8c5-bee61bd42765](https://cdn.hashnode.com/res/hashnode/image/upload/v1736740454730/3b36b380-912d-4a2a-a8c5-bee61bd42765.png)

通过此 URL，你的上传现在可见。

## **步骤3：创建一个 Amazon 简单通知服务 (SNS)**

**SNS** 是 AWS 提供的完全托管的消息服务。它通过发送通知在应用程序之间或直接与用户沟通。

若要创建 SNS，请按以下步骤：

#### **1\. 登录 AWS 管理控制台**

然后转到 Amazon SNS。导航到 SNS 仪表板并从左侧菜单中选择**主题**。

要创建主题：

-   点击**创建主题**。
    
-   选择一个**主题类型**：标准（默认）或 FIFO（用于有序消息）。
    
-   输入你的主题的**名称**。（例如，`MyFirstSNSTopic`）。
    
    ![sns 主题创建](https://cdn.hashnode.com/res/hashnode/image/upload/v1736743311856/fa51ecb9-935a-4567-829c-b84ee3c1bee0.png)
    
-   配置可选设置，如加密、交付重试策略或标签。
    
-   点击**创建主题**。
    

#### **2\. 添加订阅：**

主题创建后，点击它以打开详细信息页面。选择**订阅**选项卡。

点击**创建订阅**并选择：

-   **协议**可以是电子邮件、SMS、HTTP/S、Lambda 或 SQS。
    
-   **端点**如电子邮件地址、电话号码或 URL。
    

点击**创建订阅**。

![订阅已创建](https://cdn.hashnode.com/res/hashnode/image/upload/v1736743374208/8005dc71-68ed-4d26-b79b-7b418959ab6c.png)

#### **3\. 确认订阅：**

如果你选择了电子邮件或 SMS，将会向提供的端点发送一个确认链接或代码。请按照说明确认订阅。

![来自 amazon sns 的确认消息](https://cdn.hashnode.com/res/hashnode/image/upload/v1736743525222/90c3b61d-eeb2-450d-a397-253b9e3c15db.png)

既然我们已经完成了这些操作，让我们创建一个 Amazon Lambda 函数，该函数将触发 SNS，以便消息将发送到你的邮件中。

## **步骤4：创建一个 IAM 策略**

此策略创建用于授权 Amazon Lambda 触发事件，并确保 CloudWatch 自动触发以监控应用程序的事件。

要创建策略，请按以下步骤：

#### **1\. 登录 AWS 管理控制台。**

在左侧菜单中，选择**策略**。然后：

-   点击**创建策略**。
    
-   选择**可视化**标签，然后选择**SNS**服务。
    
-   然后，点击**选择**标签以创建自定义策略。
    

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sns:Publish",
            "Resource": "arn:aws:sns:REGION:ACCOUNT_ID:goal_topic"
        }
    ]
}
```

然后，用你的信息替换以下占位符：

-   `region`：你的 AWS 地区（例如，`us-east-1`）。
    
-   `account-id`：你的 AWS 账户 ID。
    
-   `topic-name`：你的 SNS 主题名称。
    

#### **2\. 查看并创建策略：**

你可以通过以下步骤来做到这一点：

-   点击审查按钮。
    
-   给你的策略命名（例如，`LambdaSNSPolicy`），以及可选的描述。
    
-   点击**创建策略**。
    

#### **3\. 将策略附加到 Lambda 执行角色**

现在，你需要将策略附加到你的 Lambda 执行角色。要做到这一点，请按以下步骤：

-   转到 IAM 控制台中的**角色**部分。
    
-   搜索并选择执行角色。
    

-   接下来，搜索你刚刚创建的策略并选择它。
    
-   点击**附加策略**。
    

两个策略将会自动附加。

## **步骤5：创建一个Amazon Lambda函数**

Amazon Lambda是AWS提供的一项服务，允许你在无需管理服务器的情况下运行代码。你上传代码，当需要时，Lambda会自动运行并扩展代码。

按照以下步骤创建一个Amazon Lambda函数：

#### **1\. 登录AWS管理控制台**：

导航到AWS Lambda。

#### **2\. 创建一个函数**：

点击**创建函数**并选择选项**从头开始编写**。

填写详细信息：

-   **函数名称**：输入一个唯一名称（例如，`SNSLambdaFunction`）。
    
-   **运行时**：选择运行时（例如，Python、Node.js、Java等）。
    

![creating a function](https://cdn.hashnode.com/res/hashnode/image/upload/v1736751631488/fe254e56-89f1-4938-b2a9-e59b24e54a04.png)

-   **角色**：选择或创建一个角色。如果你已经有一个角色，选择**使用现有角色**。否则，选择**创建具有基本Lambda权限的新角色**。

![role choosing](https://cdn.hashnode.com/res/hashnode/image/upload/v1736751816631/97475623-10dc-4427-87ab-7e8789526e9e.png)

-   点击**创建函数按钮**。

#### **3\. 粘贴代码**：

在Lambda函数页面，进入**配置**选项卡：

![configuration tab](https://cdn.hashnode.com/res/hashnode/image/upload/v1736753706928/b0351665-c506-454e-9c71-b4a52fce5a0a.png)

记住，我们是尝试获取一个报价。我会在此处添加我们创建的话题的ARN，并包含我的API密钥。但在本教程中，我将直接使用API来获取数据。

![1322e207-5c51-45f1-979f-05f2388e9557](https://cdn.hashnode.com/res/hashnode/image/upload/v1736754071465/1322e207-5c51-45f1-979f-05f2388e9557.png)

#### **4\. 编写Lambda代码：**

进入Lambda函数中的**代码**选项卡。然后从你的IDE中编写或粘贴代码以处理传入的SNS消息。

示例：

![Testing code](https://cdn.hashnode.com/res/hashnode/image/upload/v1736754319755/60224578-53c2-43f6-9bf1-8cba6afaa1a3.png)

以下是代码：

```
import os
import json
import urllib.request
import boto3

def fetch_random_quote():
    """
    从ZenQuotes API获取随机报价。
    """
    api_url = "https://zenquotes.io/api/random"
    try:
        with urllib.request.urlopen(api_url) as response:
            data = json.loads(response.read().decode())
            if data and isinstance(data, list):
                # 格式化报价和作者
                quote = data[0].get("q", "没有可用的报价")
                author = data[0].get("a", "未知作者")
                return f'"{quote}" - {author}'
            else:
                return "没有可用的报价。"
    except Exception as e:
        print(f"获取随机报价时出错：{e}")
        return "无法获取报价。"

def lambda_handler(event, context):
    """
    AWS Lambda处理函数，用于获取随机报价并将其发布到SNS主题。
    """
    # 从环境变量获取SNS主题ARN
    sns_topic_arn = os.getenv("SNS_TOPIC_ARN")
    sns_client = boto3.client("sns")

    # 获取随机报价
    quote = fetch_random_quote()
    print(f"获取到的报价：{quote}")

    # 将报价发布到SNS
    try:
        sns_client.publish(
            TopicArn=sns_topic_arn,
            Message=quote,
            Subject="每日随机报价，帮助你保持动力，激励你实现目标",
        )
        print("报价成功发布到SNS。")
    except Exception as e:
        print(f"发布到SNS时出错：{e}")
        return {"statusCode": 500, "body": "发布到SNS时出错"}

    return {"statusCode": 200, "body": "报价已发送到SNS"}
```

#### **5\. 保存：**

点击部署按钮保存。

![8ec83b6b-874c-47c3-a1ad-8d8b85cd6d48](https://cdn.hashnode.com/res/hashnode/image/upload/v1736756768348/8ec83b6b-874c-47c3-a1ad-8d8b85cd6d48.png)

#### **6\. 测试你的Lambda函数**：

进入**测试**选项卡并创建一个新的测试事件。

![TEST EVENT](https://cdn.hashnode.com/res/hashnode/image/upload/v1736757103169/834c5b6c-8796-4a52-bcc3-cbd8009b01da.png)

然后保存并运行测试。如果成功，将会发送一条消息：

![success msg](https://cdn.hashnode.com/res/hashnode/image/upload/v1736757309807/d97f9648-0000-4ec8-b287-df5250b3be0a.png)

这意味着消息已为你创建。

最后，根据你在本教程中使用的终端，检查你的电子邮件或SMS。在我的例子中，我使用了电子邮件。

![email notification](https://cdn.hashnode.com/res/hashnode/image/upload/v1736757884384/d5b949d4-0804-4694-9674-77fc0265e2e8.png)

## **步骤6：创建一个EventBridge**

Amazon EventBridge是一项服务，可以帮助你连接应用程序和AWS服务，例如Amazon SNS和Amazon Lambda。

要创建一个Amazon EventBridge规则，请按照以下步骤操作：

#### **1\. 导航到EventBridge**：

在搜索栏中输入**EventBridge**并在服务列表中选择它。

#### **2\. 创建一个规则：**

在EventBridge控制台中，点击左侧面板上的**规则**。然后点击**创建规则**按钮。

-   **名称**: 输入规则的唯一名称。
    
-   **描述 (可选)**: 添加描述以解释规则的作用。
    

#### **4\. 选择事件总线**：

选择 **默认事件总线**（或另一个您创建的事件总线）。

#### **5\. 定义事件模式或计划**：

**对于事件模式**：

-   选择一个 **AWS 服务** 作为事件源。
    
-   选择具体的 **事件类型**（例如，S3 文件上传或 EC2 实例状态变更）。
    

**对于计划**：

-   选择 **计划** 选项以在固定间隔运行规则（例如，每 5 分钟）。
  
![rule details](https://cdn.hashnode.com/res/hashnode/image/upload/v1736759371221/ca28dcf2-061a-4c8f-8191-32bdf8380111.png)

-   单击继续。这将带您进入具体的细节页面：

![schedule page](https://cdn.hashnode.com/res/hashnode/image/upload/v1736759696183/44ba44e8-2a1b-4cd8-928b-6fe87cc35c4e.png)

-   向下滚动并点击 cron 调度器。cron 调度器指定消息发送的时间。
    
-   为灵活时间窗口选项选择 **"关闭"**。
    
-   检查规则细节以确认一切正确无误。
    
-   点击 **"下一步"** 按钮，进入 **目标** 页面。
    
    ![cron scheduler ](https://cdn.hashnode.com/res/hashnode/image/upload/v1736760213450/c4f6cccf-7f89-485d-802e-051284c89f9a.png)
    
    上图展示了消息发送的时间。
    
    - 在目标页面，选择 **AWS Lambda** 以调用您的函数。

![the target page](https://cdn.hashnode.com/res/hashnode/image/upload/v1736761288843/e35a7153-48d7-4b34-86ce-9cba68bd5161.png)

-   向下滚动以调用并选择您创建的函数。

![invoke the function](https://cdn.hashnode.com/res/hashnode/image/upload/v1736761539389/4faa1dbc-5635-4de3-8087-15a444298b2e.png)

-   点击“下一步”按钮继续。这将带您进入设置页面。在权限部分，选择“使用现有规则”。

![setting page](https://cdn.hashnode.com/res/hashnode/image/upload/v1736782488383/ffbbcc4e-6379-4c9c-af16-f4c017e7426c.png)

-   最后，进入审核并创建计划：

![2d0e7ff9-bb7d-462d-a644-23517f47e53e](https://cdn.hashnode.com/res/hashnode/image/upload/v1736783029778/2d0e7ff9-bb7d-462d-a644-23517f47e53e.png)

-   下一页会显示所有的细节：

![4545d2da-5020-46c3-a265-5499e6b4e74f](https://cdn.hashnode.com/res/hashnode/image/upload/v1736783558777/4545d2da-5020-46c3-a265-5499e6b4e74f.png)

使用 EventBridge 为用户创建一个调度器。

## **步骤 7: 上传您的代码**

最后，将您的代码上传到 GitHub，并包含适当的文档以帮助解释代码的工作原理。

如果您不知道如何操作，请查看此文档：[上传项目到 GitHub][16]。

## 结论

如果您按照所有这些步骤操作，您将创建一个使用 AWS Lambda、Amazon S3、Amazon SNS 和 Amazon EventBridge 的目标实现语录应用程序。这个应用程序会获取激励语录并按计划发送给订阅者。

您可以在 [这里][17] 找到仓库链接。

如果您有任何问题，请随时分享您的进展或询问问题。

如果您觉得这篇文章对您有帮助，请与他人分享。

通过关注我的 [Twitter][18]、[LinkedIn][19] 和 [GitHub][20]，随时关注我的项目。

感谢您的阅读 💖。

**免责声明：**  
本文中展示的资源，包括 S3 存储桶及其 ARN，已被删除且不再存在。截图中的详情仅用于演示目的。

[1]: https://aws.amazon.com/
[2]: https://github.com/
[3]: https://code.visualstudio.com/
[4]: https://www.sublimetext.com/download
[5]: https://zenquotes.io/
[6]: #heading-step-1-set-up-your-development-environment
[7]: #heading-step-2-create-an-amazon-simple-storage-service-s3
[8]: #heading-step-3-create-an-amazon-simple-simple-notification-service
[9]: #heading-step-4-create-an-iam-policy
[10]: #heading-step-5-create-an-amazon-lambda-function
[11]: #heading-step-6-create-an-eventbridge
[12]: #heading-step-7-upload-your-code
[13]: #heading-conclusion
[14]: https://www.python.org/downloads/
[15]: https://ubuntu.com/download
[16]: https://docs.github.com/en/get-started/start-your-journey/uploading-a-project-to-github
[17]: https://github.com/ijayhub/goal-manifestation-quote
[18]: https://https//twitter.com/ijaydimples
[19]: https://www.linkedin.com/in/ijeoma-igboagu/
[20]: https://github.com/ijayhub

