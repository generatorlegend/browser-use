---
title: Supported Actions in Browser-use
description: A comprehensive list of all supported actions in Browser-use, including descriptions, required parameters, and example usage.
---

# Supported Actions in Browser-use

This document provides a comprehensive list of all supported actions in Browser-use. For each action, you'll find a description, required parameters, and example usage. These actions can be used in tasks given to the agent to interact with web pages and perform various operations.

## Table of Contents

1. [Navigation Actions](#navigation-actions)
2. [Element Interaction Actions](#element-interaction-actions)
3. [Tab Management Actions](#tab-management-actions)
4. [Content Actions](#content-actions)
5. [Utility Actions](#utility-actions)

## Navigation Actions

### search_google

Search for a query in Google in the current tab.

**Parameters:**
- `query` (str): The search query to be entered in Google.

**Example:**
```python
{
    "search_google": {
        "query": "best restaurants in New York"
    }
}
```

### go_to_url

Navigate to a specific URL in the current tab.

**Parameters:**
- `url` (str): The URL to navigate to.

**Example:**
```python
{
    "go_to_url": {
        "url": "https://www.example.com"
    }
}
```

### go_back

Navigate back to the previous page.

**Parameters:** None

**Example:**
```python
{
    "go_back": {}
}
```

## Element Interaction Actions

### click_element_by_index

Click an element on the page by its index.

**Parameters:**
- `index` (int): The index of the element to click.

**Example:**
```python
{
    "click_element_by_index": {
        "index": 5
    }
}
```

### click_element_by_selector

Click an element on the page by its CSS selector.

**Parameters:**
- `css_selector` (str): The CSS selector of the element to click.

**Example:**
```python
{
    "click_element_by_selector": {
        "css_selector": "#submit-button"
    }
}
```

### click_element_by_xpath

Click an element on the page by its XPath.

**Parameters:**
- `xpath` (str): The XPath of the element to click.

**Example:**
```python
{
    "click_element_by_xpath": {
        "xpath": "//button[contains(text(), 'Submit')]"
    }
}
```

### click_element_by_text

Click an element on the page by its text content.

**Parameters:**
- `text` (str): The text content of the element to click.
- `nth` (int, optional): The nth occurrence of the element with the specified text.
- `element_type` (str, optional): The type of element to look for (e.g., "button", "a", "div").

**Example:**
```python
{
    "click_element_by_text": {
        "text": "Add to Cart",
        "nth": 1,
        "element_type": "button"
    }
}
```

### input_text

Input text into an interactive element.

**Parameters:**
- `index` (int): The index of the element to input text into.
- `text` (str): The text to input.

**Example:**
```python
{
    "input_text": {
        "index": 3,
        "text": "John Doe"
    }
}
```

### select_dropdown_option

Select an option from a dropdown menu.

**Parameters:**
- `index` (int): The index of the dropdown element.
- `text` (str): The text of the option to select.

**Example:**
```python
{
    "select_dropdown_option": {
        "index": 2,
        "text": "Option 2"
    }
}
```

## Tab Management Actions

### switch_tab

Switch to a different tab.

**Parameters:**
- `page_id` (int): The ID of the tab to switch to.

**Example:**
```python
{
    "switch_tab": {
        "page_id": 1
    }
}
```

### open_tab

Open a new tab with a specified URL.

**Parameters:**
- `url` (str): The URL to open in the new tab.

**Example:**
```python
{
    "open_tab": {
        "url": "https://www.example.com"
    }
}
```

### close_tab

Close an existing tab.

**Parameters:**
- `page_id` (int): The ID of the tab to close.

**Example:**
```python
{
    "close_tab": {
        "page_id": 2
    }
}
```

## Content Actions

### extract_content

Extract specific information from the current page.

**Parameters:**
- `goal` (str): The extraction goal, describing what information to extract.
- `should_strip_link_urls` (bool): Whether to strip link URLs from the extracted content.

**Example:**
```python
{
    "extract_content": {
        "goal": "Extract all company names mentioned on the page",
        "should_strip_link_urls": true
    }
}
```

### save_pdf

Save the current page as a PDF file.

**Parameters:** None

**Example:**
```python
{
    "save_pdf": {}
}
```

### save_html_to_file

Save the raw HTML content of the current page to a local file.

**Parameters:** None

**Example:**
```python
{
    "save_html_to_file": {}
}
```

## Utility Actions

### wait

Wait for a specified number of seconds.

**Parameters:**
- `seconds` (int, optional): The number of seconds to wait. Default is 3.

**Example:**
```python
{
    "wait": {
        "seconds": 5
    }
}
```

### wait_for_element

Wait for an element to become visible on the page.

**Parameters:**
- `selector` (str): The CSS selector of the element to wait for.
- `timeout` (int, optional): The maximum time to wait in milliseconds.

**Example:**
```python
{
    "wait_for_element": {
        "selector": "#loading-spinner",
        "timeout": 5000
    }
}
```

### scroll_down

Scroll down the page.

**Parameters:**
- `amount` (int, optional): The number of pixels to scroll. If not specified, scrolls down one page.

**Example:**
```python
{
    "scroll_down": {
        "amount": 500
    }
}
```

### scroll_up

Scroll up the page.

**Parameters:**
- `amount` (int, optional): The number of pixels to scroll. If not specified, scrolls up one page.

**Example:**
```python
{
    "scroll_up": {
        "amount": 300
    }
}
```

### done

Complete the task and return the final result.

**Parameters:**
- `success` (bool): Whether the task was completed successfully.
- `text` (str): The final result or message.

**Example:**
```python
{
    "done": {
        "success": true,
        "text": "Task completed successfully. Found 5 restaurants matching the criteria."
    }
}
```

## Using Actions in Tasks

When giving tasks to the agent, you can use these actions to create a sequence of operations. For example, to search for a product on an e-commerce site and add it to the cart, you might use a combination of actions like this:

```python
[
    {
        "go_to_url": {
            "url": "https://www.example-shop.com"
        }
    },
    {
        "input_text": {
            "index": 1,
            "text": "smartphone"
        }
    },
    {
        "click_element_by_text": {
            "text": "Search",
            "element_type": "button"
        }
    },
    {
        "wait_for_element": {
            "selector": ".search-results",
            "timeout": 5000
        }
    },
    {
        "click_element_by_text": {
            "text": "Add to Cart",
            "nth": 1
        }
    },
    {
        "extract_content": {
            "goal": "Extract the name and price of the added product",
            "should_strip_link_urls": true
        }
    },
    {
        "done": {
            "success": true,
            "text": "Product successfully added to cart."
        }
    }
]
```

This sequence of actions navigates to a website, searches for a product, waits for the results to load, adds the first product to the cart, extracts some information, and completes the task.

Remember to handle potential errors and edge cases in your tasks, such as elements not being found or pages taking too long to load.