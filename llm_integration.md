# LLM Integration with Browser-use

This guide explains how to integrate different Language Models (LLMs) with Browser-use. We'll cover supported LLM providers, configuration of API keys, and specific considerations for each provider. Examples of using different LLMs in tasks are also included.

## Supported LLM Providers

Browser-use currently supports the following LLM providers:

1. OpenAI
2. Azure OpenAI

## Configuring API Keys

To use an LLM with Browser-use, you need to set up the appropriate API keys as environment variables.

### OpenAI

For OpenAI, set the `OPENAI_API_KEY` environment variable:

```bash
export OPENAI_API_KEY=your_openai_api_key_here
```

### Azure OpenAI

For Azure OpenAI, set both the `AZURE_OPENAI_API_KEY` and `AZURE_OPENAI_ENDPOINT` environment variables:

```bash
export AZURE_OPENAI_API_KEY=your_azure_openai_api_key_here
export AZURE_OPENAI_ENDPOINT=your_azure_openai_endpoint_here
```

## Using Different LLMs in Tasks

### OpenAI

To use an OpenAI model (e.g., GPT-4), you can set up your agent like this:

```python
from langchain_openai import ChatOpenAI
from browser_use import Agent

llm = ChatOpenAI(model='gpt-4o')
agent = Agent(
    task='Go to amazon.com, search for laptop, sort by best rating, and give me the price of the first result',
    llm=llm,
)

async def main():
    await agent.run(max_steps=10)
    input('Press Enter to continue...')

asyncio.run(main())
```

### Azure OpenAI

To use an Azure OpenAI model, you need to provide additional configuration:

```python
from langchain_openai import AzureChatOpenAI
from browser_use import Agent

# Retrieve Azure-specific environment variables
azure_openai_api_key = os.getenv('AZURE_OPENAI_API_KEY')
azure_openai_endpoint = os.getenv('AZURE_OPENAI_ENDPOINT')

if not azure_openai_api_key or not azure_openai_endpoint:
    raise ValueError('AZURE_OPENAI_API_KEY or AZURE_OPENAI_ENDPOINT is not set')

# Initialize the Azure OpenAI client
llm = AzureChatOpenAI(
    model_name='gpt-4o',
    openai_api_key=azure_openai_api_key,
    azure_endpoint=azure_openai_endpoint,
    deployment_name='gpt-4o',  # Use deployment_name for Azure models
    api_version='2024-08-01-preview',  # Explicitly set the API version here
)

agent = Agent(
    task='Go to amazon.com, search for laptop, sort by best rating, and give me the price of the first result',
    llm=llm,
)

async def main():
    await agent.run(max_steps=10)
    input('Press Enter to continue...')

asyncio.run(main())
```

## Considerations for Each Provider

### OpenAI

- Ensure you have the necessary API usage quota for your chosen model.
- Be aware of the pricing for different models and adjust your usage accordingly.

### Azure OpenAI

- Make sure you have the correct deployment name for your Azure OpenAI model.
- Specify the correct API version when initializing the AzureChatOpenAI client.
- Ensure your Azure subscription has access to the required models.

## Conclusion

By following this guide, you should be able to integrate different LLMs with Browser-use effectively. Remember to keep your API keys secure and never expose them in your code. If you encounter any issues or need further assistance, please refer to the official documentation of the respective LLM providers or reach out to our support team.