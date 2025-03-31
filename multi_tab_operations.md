# Multi-Tab Operations with Browser-use

Browser-use provides powerful capabilities for managing multiple tabs within a browser context. This guide will walk you through the concepts and usage of multi-tab operations, allowing you to create, switch between, and manage multiple tabs efficiently.

## Table of Contents

1. [Creating New Tabs](#creating-new-tabs)
2. [Switching Between Tabs](#switching-between-tabs)
3. [Managing Multiple Browser Contexts](#managing-multiple-browser-contexts)
4. [Examples of Multi-Tab Tasks](#examples-of-multi-tab-tasks)

## Creating New Tabs

To create a new tab in Browser-use, you can use the `create_new_tab` method. This method allows you to open a new tab and optionally navigate to a specific URL.

```python
async def create_new_tab(self, url: str | None = None) -> None:
```

Example usage:

```python
# Create a new tab without navigating to a specific URL
await agent.browser.create_new_tab()

# Create a new tab and navigate to a specific URL
await agent.browser.create_new_tab("https://example.com")
```

## Switching Between Tabs

Browser-use allows you to switch between different tabs using the `switch_to_tab` method. Each tab is identified by a unique `page_id`.

```python
async def switch_to_tab(self, page_id: int) -> None:
```

To switch to a specific tab:

```python
# Switch to the tab with page_id 2
await agent.browser.switch_to_tab(2)
```

## Managing Multiple Browser Contexts

Browser-use uses the concept of browser contexts to manage multiple independent browsing sessions. Each context can have its own set of tabs, cookies, and storage states.

To create and manage multiple browser contexts:

1. Initialize multiple `BrowserContext` objects.
2. Use the `__aenter__` and `__aexit__` methods to manage the lifecycle of each context.

Example:

```python
async with BrowserContext(browser, config) as context1, BrowserContext(browser, config) as context2:
    # Perform operations in context1
    await context1.create_new_tab("https://example1.com")
    
    # Perform operations in context2
    await context2.create_new_tab("https://example2.com")
```

## Examples of Multi-Tab Tasks

Here are some examples of tasks that involve working with multiple tabs simultaneously:

### 1. Compare Information Across Multiple Websites

```python
async def compare_websites(urls):
    for url in urls:
        await agent.browser.create_new_tab(url)
    
    results = []
    for i, url in enumerate(urls):
        await agent.browser.switch_to_tab(i)
        # Extract and store information from the current tab
        results.append(await extract_info())
    
    return compare_results(results)
```

### 2. Parallel Form Submission

```python
async def submit_forms(form_data):
    for data in form_data:
        await agent.browser.create_new_tab(data['url'])
    
    tasks = []
    for i, data in enumerate(form_data):
        await agent.browser.switch_to_tab(i)
        tasks.append(asyncio.create_task(fill_and_submit_form(data)))
    
    await asyncio.gather(*tasks)
```

### 3. Monitor Multiple Live Feeds

```python
async def monitor_feeds(feed_urls):
    for url in feed_urls:
        await agent.browser.create_new_tab(url)
    
    while True:
        for i, url in enumerate(feed_urls):
            await agent.browser.switch_to_tab(i)
            updates = await check_for_updates()
            if updates:
                process_updates(updates)
        
        await asyncio.sleep(60)  # Check every minute
```

By leveraging these multi-tab operations, you can create powerful and efficient browser automation scripts that can handle complex scenarios involving multiple websites or parallel processing of web tasks.

Remember to manage your tabs and contexts responsibly, closing them when they are no longer needed to conserve system resources.