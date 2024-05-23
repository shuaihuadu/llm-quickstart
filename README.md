
# [Ollama](https://ollama.com)

## Docker部署

1. docker拉取镜像 docker pull ollama/ollama
2. 本地CPU运行 CPU only: docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
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


## 本地部署

Windows 下载地址： [点击下载](https://ollama.com/download/OllamaSetup.exe)

