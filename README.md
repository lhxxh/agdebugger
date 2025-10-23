# Setup

1. autogen推荐使用以下版本，否则代码有许多兼容问题，已经在pyproject.toml更新

```sh
autogen-agentchat  0.4.8
autogen-core       0.4.8
autogen-ext        0.4.8
```

# Test

1. 测试代码在tests文件夹下面， 测试serialization 

```sh
python tests/test_serialization.py
```

2. 测试backend, 需要 pytest-asyncio,

```sh
pip install pytest-asyncio
```

安装pytest之后，测试命令

```sh
pytest tests/test_backend.py # run all tests
pytest tests/test_backend.py::test_edit_message_queue # Run specific test
pytest tests/test_backend.py -v # With verbose output
```

# Examples

1. examples文件夹下提供使用例子， 拿local-agents举例

```sh
cd examples/local-agents 
```

2. 打开frontend

```sh
agdebugger scenario:get_agent_team
```

3. 在左上角messages启动聊天，先设置direct message 去 group_chat_manager, message 如下

```sh
0 
```

点击右侧绿色arrow，会被放在下方的message queue中处于待处理

4. 按红色按钮开始处理message queue

5. 处理过的message会被放在message history，overview会在右侧显示




# AGDebugger

AGDebugger is an interactive system to help you debug your agent teams. It offers interactions to:

1. Send and step through agent messages
2. Edit previously sent agent messages and revert to earlier points in a conversation
3. Navigate agent conversations with an interactive visualization

![screenshot of AGDebugger interface](.github/screenshots/agdebugger_sc.png)

## Local Install

You can install AGDebugger locally by cloning the repo and installing the python package.

```sh
# Install & build frontend
cd frontend
npm install
npm run build
# Install & build agdebugger python package
cd ..
pip install .
```

## Usage

AGDebugger is built on top of [AutoGen](https://microsoft.github.io/autogen/stable/). To use AGDebugger, you provide a python file that exposes a function that creates an [AutoGen AgentChat](https://microsoft.github.io/autogen/stable/user-guide/agentchat-user-guide/index.html) team for debugging. You can then launch AgDebugger with this agent team.

For example, the script below creates a simple agent team with a single WebSurfer agent.

```python
# scenario.py
from autogen_agentchat.teams import MagenticOneGroupChat
from autogen_agentchat.ui import Console
from autogen_ext.agents.web_surfer import MultimodalWebSurfer
from autogen_ext.models.openai import OpenAIChatCompletionClient

async def get_agent_team():
    model_client = OpenAIChatCompletionClient(model="gpt-4o")

    surfer = MultimodalWebSurfer(
        "WebSurfer",
        model_client=model_client,
    )
    team = MagenticOneGroupChat([surfer], model_client=model_client)

    return team
```

We can then launch the interface with:

```sh
 agdebugger scenario:get_agent_team
```

Once in the interface, you can send a GroupChatStart message to the start the agent conversation and begin debugging!

## Citation

See our [CHI 2025 paper](https://arxiv.org/abs/2503.02068) for more details on the design and evaluation of AGDebugger.

```bibtex
@inproceedings{epperson25agdebugger,
    title={Interactive Debugging and Steering of Multi-Agent AI Systems},
    author={Will Epperson and Gagan Bansal and Victor Dibia and Adam Fourney and Jack Gerrits and Erkang Zhu and Saleema Amershi},
    year={2025},
    publisher = {Association for Computing Machinery},
    booktitle = {Proceedings of the 2025 CHI Conference on Human Factors in Computing Systems},
    series = {CHI '25}
}
```
