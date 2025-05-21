# Baidu Cloud LLM Security MCP Server

This code repository contains an MCP server that provides security for the content for LLM（ https://cloud.baidu.com/doc/AIGC_SEC/s/qlxblhd5j ）Access to functions.

## Precondition

Before using the Baidu Cloud LLM Security MCP Server, please ensure that you have the following conditions:
1. Python 3.10 or higher version
2. Installed [UV]（ https://github.com/astral-sh/uv ）Used to run MCP Server

## usage

The recommended way to use Baidu Cloud LLM Security MCP Server is to run it through `uv` without the need for installation.

Clone the code repository and execute the following command:
```
git clone https://github.com/liangjunyu2010/mcp_server_safe_content_check.git
cd mcp_server_safe_content_check
```

Subsequently, you can directly run it through `uv`, where `BAIDU_CLOUD_ACCESS_KEY_ID` and `BAIDU_CLOUD_SECRET_ACCESS_KEY` can be modified according to actual needs:

```
uv run src/mcp_server_safe_content_check/server.py 
uv run src/mcp_server_safe_content_check/server.py --BAIDU_CLOUD_ACCESS_KEY_ID ACCESS_KEY --BAIDU_CLOUD_SECRET_ACCESS_KEY SECRET_KEY
```

Alternatively, modify the `. env` file in the `src/mcp-user_stafe_ccontent_check/` directory to set environment variables, and then run the server using the following command:

```
uv run src/mcp_server_safe_content_check/server.py 
```

## Supported applications

Baidu Cloud LLM Security MCP Server can be used in conjunction with various large language model applications that support model context protocols

- **Cursor**：an artificial intelligence code editor that supports MCP

- **Customize MCP client**：Any application that implements the MCP client specification

## Usage in cursor

[Cursor also supports MCP]（ https://docs.cursor.com/context/model-context-protocol ）Tools. You can add Baidu Cloud LLM Security MCP Server to the cursor in two ways:

依次打开`Cursor Settings`>`MCP`，Click `Add MCP Server`，add config in `mcp.json`：

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
Restart the Cursor or reload the window.

## Methods

Baidu Cloud LLM Security MCP Server provides the following methods:

### Input detection operation

- `input_analyze`: Detecting input content

  - param：
    - `text`: The input text content

## Environment variable

[Baidu Cloud IAM](https://console.bce.baidu.com/iam/#/iam/accesslist)  Permission `AFDFullControlAccessPolicy`

- `BAIDU_CLOUD_ACCESS_KEY_ID`: Baidu Cloud Authorization ACCESS_KEY
- `BAIDU_CLOUD_SECRET_ACCESS_KEY`: Baidu Cloud Authorization SECRET_KEY

## Using examples

### Cursor

#### Example : Check if there is any unsafe information in the file content

```
帮忙检测下风险
```
The cursor will use the input.analyze provided by the Baidu Cloud LLM Security MCP Server to detect whether the input content is secure

```
检测结果显示，该文本内容存在极高风险：
风险类型：犯罪相关内容（hitType: crime）
风险评分：0.998（满分1分）
处理建议：严禁传播（action: 2）
安全评估：内容不安全（isSafe: 0）
```
