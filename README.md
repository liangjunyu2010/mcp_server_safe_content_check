# 百度云大模型内容安全MCP Server

本代码仓库包含一个 MCP 服务器，它提供对[百度云大模型内容安全](https://cloud.baidu.com/doc/AIGC_SEC/s/qlxblhd5j)功能的访问。

## 前提条件

在使用百度云大模型内容安全MCP Server之前，请确保你具备以下条件：
1. Python 3.10 或更高版本
2. 已安装[uv](https://github.com/astral-sh/uv)用于运行MCP Server

## 使用方式

使用百度云大模型内容安全MCP Server的推荐方式是通过`uv`运行，而无需进行安装。

克隆代码仓库，执行以下命令：
```
git clone https://github.com/liangjunyu2010/mcp_server_safe_content_check.git
cd mcp_server_safe_content_check
```
随后，你可以直接通过`uv`运行，其中`BAIDU_CLOUD_ACCESS_KEY_ID`和`BAIDU_CLOUD_SECRET_ACCESS_KEY`根据实际需要修改：
```
uv run src/mcp_server_safe_content_check/server.py 
uv run src/mcp_server_safe_content_check/server.py --BAIDU_CLOUD_ACCESS_KEY_ID ACCESS_KEY --BAIDU_CLOUD_SECRET_ACCESS_KEY SECRET_KEY
```

或者，在`src/mcp_server_safe_content_check/`目录中修改`.env`文件来设置环境变量，再使用以下命令运行服务器：
```
uv run src/mcp_server_safe_content_check/server.py 
```

## 支持的应用程序

百度云大模型内容安全MCP Server可以与各种支持模型上下文协议的大语言模型应用程序配合使用：

- **Cursor**：支持 MCP 的人工智能代码编辑器

- **自定义 MCP 客户端**：任何实现 MCP 客户端规范的应用程序

## 在 Cursor 中的使用方法

[Cursor 也支持 MCP](https://docs.cursor.com/context/model-context-protocol)工具。你可以通过两种方式将百度MCP Server添加到Cursor中：

依次打开`Cursor设置`>`功能`>`MCP`，点击`+添加新的MCP服务器`按钮，在`mcp.json`中添加以下配置：
```JSON
{
    "mcpServers": {
        "safe-content-check": {
            "command": "uv",
            "args": [
                "run",
                "--with",
                "mcp[cli]",
                "mcp",
                "run",
                "/PATH/mcp_server_safe_content_check/src/mcp_server_safe_content_check/server.py"
            ],
            "env": {
                "BAIDU_CLOUD_ACCESS_KEY_ID": "****",
                "BAIDU_CLOUD_SECRET_ACCESS_KEY": "****"
            }
        }
    }
}
```
重启 Cursor 或重新加载窗口。

## 可用工具

百度云大模型内容安全MCP Server提供以下工具：

### 输入检测操作

- `input_analyze`: 创建一个新的Database

  - 参数：
    - `text`: 输入的文本内容

## 环境变量

[百度云IAM创建](https://console.bce.baidu.com/iam/#/iam/accesslist)  权限选择 `AFDFullControlAccessPolicy`

- `BAIDU_CLOUD_ACCESS_KEY_ID`: 百度云授权ACCESS_KEY
- `BAIDU_CLOUD_SECRET_ACCESS_KEY`: 百度云授权SECRET_KEY

## 使用样例

### 使用Cursor

#### Example : 检查文件内容是否存在不安全信息

```
帮忙检测下风险
```
Cursor将使用百度云大模型内容安全MCP Server提供的input_analyze来检测输入内容是否安全.

```
检测结果显示，该文本内容存在极高风险：
风险类型：犯罪相关内容（hitType: crime）
风险评分：0.998（满分1分）
处理建议：严禁传播（action: 2）
安全评估：内容不安全（isSafe: 0）
```
