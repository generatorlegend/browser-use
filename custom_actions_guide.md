# Custom Actions Guide

This guide provides a step-by-step approach to creating and registering custom actions in Browser-use. Custom actions allow you to extend the functionality of the agent by defining new behaviors that can be executed during task completion.

## Table of Contents

1. [Understanding Custom Actions](#understanding-custom-actions)
2. [Defining Action Parameters](#defining-action-parameters)
3. [Implementing Action Logic](#implementing-action-logic)
4. [Registering Custom Actions](#registering-custom-actions)
5. [Best Practices](#best-practices)
6. [Examples](#examples)

## Understanding Custom Actions

Custom actions in Browser-use are user-defined functions that can be called by the agent to perform specific tasks. These actions can interact with the browser, manipulate data, or perform any other custom logic you need for your use case.

## Defining Action Parameters

To create a custom action, you first need to define its parameters. This is done using Pydantic models, which provide type checking and validation for your action inputs.

1. Import the necessary modules:

```python
from pydantic import BaseModel
from typing import Optional
```

2. Create a Pydantic model for your action parameters:

```python
class MyCustomActionParams(BaseModel):
    param1: str
    param2: int
    optional_param: Optional[bool] = False
```

## Implementing Action Logic

Next, you'll implement the logic for your custom action. This is done by creating an asynchronous function that takes the action parameters and performs the desired operations.

1. Define your action function:

```python
async def my_custom_action(params: MyCustomActionParams, browser: BrowserContext):
    # Your action logic here
    result = f"Executed custom action with {params.param1} and {params.param2}"
    return ActionResult(extracted_content=result, include_in_memory=True)
```

2. Make sure to return an `ActionResult` object with the appropriate fields set.

## Registering Custom Actions

To make your custom action available to the agent, you need to register it with the Controller.

1. In your main script, create a Controller instance:

```python
controller = Controller()
```

2. Use the `@controller.action` decorator to register your custom action:

```python
@controller.action(
    "Description of what your custom action does",
    param_model=MyCustomActionParams
)
async def my_custom_action(params: MyCustomActionParams, browser: BrowserContext):
    # Your action logic here
    # ...
```

## Best Practices

1. **Clear Descriptions**: Provide a clear and concise description of what your action does when registering it. This helps the LLM understand when and how to use the action.

2. **Type Hinting**: Always use type hints for your action parameters and return values. This improves code readability and helps catch errors early.

3. **Error Handling**: Implement proper error handling within your custom actions to gracefully handle unexpected situations.

4. **Modular Design**: Keep your custom actions focused on a single task or functionality. This makes them easier to maintain and reuse.

5. **Documentation**: Add docstrings to your custom actions to explain their purpose, parameters, and return values.

## Examples

Here's an example of a custom action that performs a specific search on a website:

```python
from pydantic import BaseModel
from browser_use.controller import Controller
from browser_use.browser.context import BrowserContext
from browser_use.agent.views import ActionResult

class CustomSearchParams(BaseModel):
    query: str
    site: str

controller = Controller()

@controller.action(
    "Perform a custom search on a specific website",
    param_model=CustomSearchParams
)
async def custom_search(params: CustomSearchParams, browser: BrowserContext):
    page = await browser.get_current_page()
    await page.goto(f"https://{params.site}")
    await page.fill('input[name="q"]', params.query)
    await page.press('input[name="q"]', "Enter")
    await page.wait_for_load_state()
    
    result = f"Performed custom search for '{params.query}' on {params.site}"
    return ActionResult(extracted_content=result, include_in_memory=True)
```

This custom action navigates to a specified website, enters a search query, and performs the search. You can now use this action in your agent's workflow by calling it with the appropriate parameters.

By following this guide, you can create and register custom actions to extend the functionality of your Browser-use agent and tailor it to your specific needs.