# 使用第三方OpenAI兼容服务的示例

本文档展示如何使用新增的 `OPENAI_API_URL` 环境变量功能来连接第三方OpenAI兼容的API服务。

## 功能说明

ellmer现在支持通过 `OPENAI_API_URL` 环境变量来自定义OpenAI API的基础URL，这样你就可以使用任何OpenAI兼容的第三方服务商。

## 设置方法

### 1. 编辑环境变量文件

使用以下命令打开 `.Renviron` 文件：

```r
usethis::edit_r_environ()
```

### 2. 添加环境变量

在 `.Renviron` 文件中添加以下内容：

```
# OpenAI API配置
OPENAI_API_KEY=your_api_key_here
OPENAI_API_URL=https://your-third-party-service.com/v1
```

**注意事项：**
- `OPENAI_API_KEY`: 你的API密钥
- `OPENAI_API_URL`: 第三方服务的基础URL（不要包含具体的端点路径如 `/chat/completions`）
- URL末尾的斜杠会被自动移除

### 3. 重启R会话

保存文件后，重启R会话以使环境变量生效。

## 使用示例

```r
library(ellmer)

# 创建聊天对象 - 会自动使用OPENAI_API_URL环境变量
chat <- chat_openai(
  model = "gpt-3.5-turbo",  # 根据你的服务商支持的模型调整
  system_prompt = "你是一个有帮助的助手。"
)

# 开始对话
response <- chat$chat("你好，请介绍一下你自己。")
print(response)
```

## 支持的第三方服务示例

以下是一些常见的OpenAI兼容服务的配置示例：

### Azure OpenAI
```
OPENAI_API_URL=https://your-resource.openai.azure.com/openai/deployments/your-deployment
```

### 本地部署的服务
```
OPENAI_API_URL=http://localhost:8000/v1
```

### 其他第三方服务
```
OPENAI_API_URL=https://api.third-party-service.com/v1
```

## 验证配置

你可以通过以下方式验证配置是否正确：

```r
# 检查当前使用的API URL
chat <- chat_openai()
print(chat$get_provider()@base_url)
```

## 故障排除

1. **确保URL格式正确**：URL应该以 `http://` 或 `https://` 开头
2. **检查API密钥**：确保 `OPENAI_API_KEY` 对应你选择的服务商
3. **验证服务兼容性**：确保第三方服务完全兼容OpenAI API格式
4. **重启R会话**：修改环境变量后需要重启R会话

## 恢复默认设置

如果想恢复使用官方OpenAI服务，只需在 `.Renviron` 文件中删除或注释掉 `OPENAI_API_URL` 行：

```
# OPENAI_API_URL=https://your-third-party-service.com/v1
```

然后重启R会话即可。