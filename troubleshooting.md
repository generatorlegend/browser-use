# Troubleshooting Guide for Browser-use

This guide covers common issues you may encounter when using Browser-use, along with step-by-step solutions and debugging techniques.

## Table of Contents

1. [Browser Connection Problems](#browser-connection-problems)
2. [LLM API Errors](#llm-api-errors)
3. [Action-Specific Issues](#action-specific-issues)
4. [Using Logging for Debugging](#using-logging-for-debugging)

## Browser Connection Problems

### Issue: Unable to Connect to Browser Instance

If you're having trouble connecting to a browser instance, try the following steps:

1. Check if the browser is running:
   ```python
   import requests

   try:
       response = requests.get('http://localhost:9222/json/version', timeout=2)
       if response.status_code == 200:
           print("Browser is running")
       else:
           print("Browser is not running")
   except requests.ConnectionError:
       print("Browser is not running")
   ```

2. Ensure the correct browser binary path is set:
   ```python
   from browser_use.browser.browser import Browser, BrowserConfig

   config = BrowserConfig(browser_binary_path='/path/to/your/browser')
   browser = Browser(config)
   ```

3. Verify that the port 9222 is not already in use:
   ```python
   import socket

   with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
       if s.connect_ex(('localhost', 9222)) == 0:
           print("Port 9222 is already in use")
       else:
           print("Port 9222 is available")
   ```

### Issue: Browser Closes Unexpectedly

If the browser closes unexpectedly, check the following:

1. Ensure you're not using headless mode (not recommended):
   ```python
   config = BrowserConfig(headless=False)
   ```

2. Check if the `keep_alive` option is set to True:
   ```python
   config = BrowserConfig(keep_alive=True)
   ```

## LLM API Errors

### Issue: LLM API Connection Failed

If you encounter LLM API connection errors:

1. Verify that the required environment variables are set:
   ```python
   import os
   from browser_use.agent.views import REQUIRED_LLM_API_ENV_VARS

   llm_class = "YourLLMClass"
   required_vars = REQUIRED_LLM_API_ENV_VARS.get(llm_class, [])
   for var in required_vars:
       if var not in os.environ:
           print(f"Missing environment variable: {var}")
   ```

2. Check if the API key is valid by running a simple test:
   ```python
   from browser_use.agent.service import Agent

   async def test_llm_connection(llm):
       test_prompt = "What is the capital of France? Respond with a single word."
       try:
           response = await llm.ainvoke([HumanMessage(content=test_prompt)])
           if "paris" in str(response.content).lower():
               print("LLM API connection successful")
           else:
               print("LLM API connection failed: Unexpected response")
       except Exception as e:
           print(f"LLM API connection failed: {str(e)}")

   # Run this test in an async context
   ```

## Action-Specific Issues

### Issue: Click Action Fails

If a click action fails:

1. Check if the element is visible and clickable:
   ```python
   async def check_element_clickable(browser, index):
       selector_map = await browser.get_selector_map()
       if index not in selector_map:
           print(f"Element with index {index} does not exist")
           return

       element_node = await browser.get_dom_element_by_index(index)
       try:
           await element_node.scroll_into_view_if_needed()
           await element_node.click(timeout=1500, force=True)
           print("Element is clickable")
       except Exception as e:
           print(f"Element is not clickable: {str(e)}")
   ```

2. Try using JavaScript to click the element:
   ```python
   async def js_click(browser, index):
       element_node = await browser.get_dom_element_by_index(index)
       try:
           await element_node.evaluate('el => el.click()')
           print("Clicked element using JavaScript")
       except Exception as e:
           print(f"Failed to click element using JavaScript: {str(e)}")
   ```

### Issue: Input Text Action Fails

If an input text action fails:

1. Verify that the element is an input field:
   ```python
   async def check_input_field(browser, index):
       element_node = await browser.get_dom_element_by_index(index)
       tag_name = await element_node.evaluate('el => el.tagName')
       if tag_name.lower() in ['input', 'textarea']:
           print("Element is an input field")
       else:
           print(f"Element is not an input field (tag: {tag_name})")
   ```

2. Ensure the element is not disabled or readonly:
   ```python
   async def check_input_editable(browser, index):
       element_node = await browser.get_dom_element_by_index(index)
       is_disabled = await element_node.evaluate('el => el.disabled')
       is_readonly = await element_node.evaluate('el => el.readOnly')
       if is_disabled:
           print("Input is disabled")
       elif is_readonly:
           print("Input is readonly")
       else:
           print("Input is editable")
   ```

## Using Logging for Debugging

Browser-use uses Python's logging module for debugging. To enable more detailed logging:

1. Set up logging in your script:
   ```python
   import logging

   logging.basicConfig(level=logging.DEBUG)
   logger = logging.getLogger(__name__)
   ```

2. You can also set the log level for specific modules:
   ```python
   logging.getLogger('browser_use.agent.service').setLevel(logging.DEBUG)
   logging.getLogger('browser_use.browser.browser').setLevel(logging.DEBUG)
   ```

3. To log to a file in addition to console output:
   ```python
   import logging

   logging.basicConfig(
       level=logging.DEBUG,
       format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
       filename='browser_use_debug.log'
   )
   ```

4. Look for log messages with prefixes like 🌎, 🔌, 🖱️, ⌨️, etc., which indicate different types of actions and events in Browser-use.

By using these troubleshooting techniques and leveraging the logging system, you should be able to identify and resolve most common issues in Browser-use. If you encounter persistent problems, please refer to the official documentation or reach out to the support community for further assistance.