# Getting Started with Browser-use

Welcome to Browser-use, the easiest way to connect your AI agents with the browser. This guide will walk you through the installation process, basic usage, and core concepts of Browser-use.

## Installation

To get started with Browser-use, you'll need Python 3.11 or higher. Follow these steps to install the necessary components:

1. Install Browser-use using pip:

```bash
pip install browser-use
```

2. Install Playwright, which is used for browser automation:

```bash
playwright install chromium
```

## Setting Up Environment Variables

Browser-use requires API keys for the language model provider you want to use. Create a `.env` file in your project root and add your API keys:

```
OPENAI_API_KEY=your_openai_api_key_here
ANTHROPIC_API_KEY=your_anthropic_api_key_here
AZURE_ENDPOINT=your_azure_endpoint_here
AZURE_OPENAI_API_KEY=your_azure_openai_api_key_here
GEMINI_API_KEY=your_gemini_api_key_here
DEEPSEEK_API_KEY=your_deepseek_api_key_here
```

Replace the placeholders with your actual API keys. You only need to provide the keys for the services you plan to use.

## Basic Usage

Here's a simple example of how to use Browser-use:

```python
from langchain_openai import ChatOpenAI
from browser_use import Agent
import asyncio
from dotenv import load_dotenv

load_dotenv()

async def main():
    agent = Agent(
        task="Compare the price of gpt-4 and DeepSeek-V3",
        llm=ChatOpenAI(model="gpt-4"),
    )
    await agent.run()

asyncio.run(main())
```

This script does the following:

1. Imports necessary modules, including the `Agent` from Browser-use.
2. Loads environment variables from the `.env` file.
3. Creates an `Agent` instance with a specific task and language model.
4. Runs the agent asynchronously.

## Core Concepts

### Agent

The `Agent` is the central component of Browser-use. It combines a language model with browser automation capabilities to perform tasks. When you create an `Agent`, you provide:

- A `task`: A string describing what you want the agent to do.
- An `llm`: The language model to use (e.g., ChatOpenAI, Anthropic, etc.).

### Running Tasks

To execute a task, call the `run()` method on your `Agent` instance. This method is asynchronous, so you need to use `await` or run it within an `asyncio` event loop.

### Browser Interaction

Browser-use uses Playwright under the hood to interact with web browsers. The `Agent` can perform actions like navigating to web pages, filling forms, clicking buttons, and extracting information from web pages.

## Advanced Usage

For more advanced usage, including custom functions, different browser configurations, and complex tasks, refer to the [examples](https://github.com/browser-use/browser-use/tree/main/examples) in the Browser-use repository.

## Next Steps

- Explore the [examples](https://github.com/browser-use/browser-use/tree/main/examples) folder for more complex use cases.
- Join the [Discord community](https://link.browser-use.com/discord) to share your projects and get help.
- Check out the [cloud version](https://cloud.browser-use.com) for a hosted solution.
- Read the full [documentation](https://docs.browser-use.com) for detailed information on all features and configurations.

Happy automating with Browser-use!