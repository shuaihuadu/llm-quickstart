
# [Ollama](https://ollama.com)

## Docker部署

1. docker拉取镜像 ``` docker pull ollama/ollama ```
2. 本地CPU运行 CPU only: 
   ``` docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama ```
3. 运行模型 Run a model 

    ```
    docker exec -it ollama ollama run llama2
    docker exec -it ollama ollama run llama3
    docker exec -it ollama ollama run phi3
    docker exec -it ollama ollama run mistral
    ```

[查看支持的模型](https://ollama.com/library)

环境变量：

OLLAMA_HOST : 可以修改默认端口号

譬如，设置默认端口号为19090：

```
OLLAMA_HOST=0.0.0.0:19090
```

OLLAMA_MODELS的设置目前有Bug，只能在每次run model的时候手动设置一下

```
setx OLLAMA_MODELS "E:\LocalLLM\Models\Ollama"

setx OLLAMA_MODELS "E:\LocalLLM\Models\Ollama" & ollama run mistral

```


## 本地部署

Windows 下载地址： [点击下载](https://ollama.com/download/OllamaSetup.exe)

### 安装完成之后

```  C#
// 运行模型
ollama run phi3

//启动本地API
ollama serve
```

``` C#

// 使用Semantic Kernel调用本地模型

public async Task LocalModel_ExampleAsync(string messageAPIPlatform, string url, string modelId)
{
    Console.WriteLine($"Example using local {messageAPIPlatform}");

    var kernel = Kernel.CreateBuilder()
        .AddOpenAIChatCompletion(
            modelId: modelId,
            apiKey: null,
            endpoint: new Uri(url))
        .Build();

    var prompt = @"请使用简体中文将三个```之间的文本重写为商务邮件。使用专业的语气，清晰简洁。
                设置邮件签名为：AI Assistant.
                文本: ```{{$input}}```";

    var mailFunction = kernel.CreateFunctionFromPrompt(prompt, new OpenAIPromptExecutionSettings
    {
        TopP = 0.5,
        MaxTokens = 1000,
    });

    var response = await kernel.InvokeAsync(mailFunction, new() { ["input"] = "告诉张三，我将在本周末前完成商业计划。" });

    Console.WriteLine(response);
}

//输出内容
/*

尊敬的张三，

您好！

随函大方以此通知，我已确定了我将在本周末之前完成与您合作关于商业计划的详细报告。这份商业计划将包含所有必要的分析和建议，以确保我们能够准备好进一步的商业实施。

请在此期限内与我联系，如果有任何需求或者提议，以便我可以及时调整计划方向和内容。

衷心地期盼能够一起成功开展这个项目。


此致

敬礼，

AI Assistant

 */
```