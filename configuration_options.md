# Configuration Options

This page details all the configuration options available in Browser-use, including `AgentSettings`, `BrowserConfig`, and other relevant configuration classes. We'll explain how to customize the behavior of the agent and browser sessions with examples.

## AgentSettings

`AgentSettings` is a class that defines options for the agent. Here are the main configuration options:

```python
class AgentSettings(BaseModel):
    use_vision: bool = True
    use_vision_for_planner: bool = False
    save_conversation_path: Optional[str] = None
    save_conversation_path_encoding: Optional[str] = 'utf-8'
    max_failures: int = 3
    retry_delay: int = 10
    max_input_tokens: int = 128000
    validate_output: bool = False
    message_context: Optional[str] = None
    generate_gif: bool | str = False
    available_file_paths: Optional[list[str]] = None
    override_system_message: Optional[str] = None
    extend_system_message: Optional[str] = None
    include_attributes: list[str] = [
        'title', 'type', 'name', 'role', 'tabindex', 'aria-label',
        'placeholder', 'value', 'alt', 'aria-expanded',
    ]
    max_actions_per_step: int = 10
    tool_calling_method: Optional[ToolCallingMethod] = 'auto'
    page_extraction_llm: Optional[BaseChatModel] = None
    planner_llm: Optional[BaseChatModel] = None
    planner_interval: int = 1
    is_planner_reasoning: bool = False
    enable_memory: bool = True
    memory_interval: int = 10
    memory_config: Optional[dict] = None
```

### Example Usage

```python
from browser_use.agent.views import AgentSettings

agent_settings = AgentSettings(
    use_vision=True,
    max_failures=5,
    max_input_tokens=64000,
    override_system_message="Custom system message for the agent",
    tool_calling_method='function_calling'
)
```

## BrowserConfig

`BrowserConfig` is used to configure the browser instance. Here are the main configuration options:

```python
class BrowserConfig(BaseModel):
    wss_url: str | None = None
    cdp_url: str | None = None
    browser_class: Literal['chromium', 'firefox', 'webkit'] = 'chromium'
    browser_binary_path: str | None = None
    extra_browser_args: list[str] = Field(default_factory=list)
    headless: bool = False
    disable_security: bool = True
    deterministic_rendering: bool = False
    keep_alive: bool = False
    proxy: ProxySettings | None = None
    new_context_config: BrowserContextConfig = Field(default_factory=BrowserContextConfig)
```

### Example Usage

```python
from browser_use.browser.browser import BrowserConfig, ProxySettings

browser_config = BrowserConfig(
    browser_class='chromium',
    headless=True,
    disable_security=False,
    extra_browser_args=['--no-sandbox', '--disable-setuid-sandbox'],
    proxy=ProxySettings(server="http://proxy.example.com:8080")
)
```

## BrowserContextConfig

`BrowserContextConfig` is used to configure individual browser contexts. Here are the main configuration options:

```python
class BrowserContextConfig(BaseModel):
    viewport: ViewportSize = Field(default_factory=lambda: ViewportSize(width=1280, height=720))
    screen: ViewportSize = Field(default_factory=lambda: ViewportSize(width=1920, height=1080))
    user_agent: str | None = None
    locale: str | None = None
    timezone_id: str | None = None
    geolocation: Geolocation | None = None
    permissions: list[str] = Field(default_factory=list)
    extra_http_headers: dict[str, str] = Field(default_factory=dict)
    offline: bool = False
    http_credentials: HttpCredentials | None = None
    device_scale_factor: float | None = None
    is_mobile: bool | None = None
    has_touch: bool | None = None
    color_scheme: Literal['dark', 'light', 'no-preference'] | None = None
    reduced_motion: Literal['reduce', 'no-preference'] | None = None
    forced_colors: Literal['active', 'none'] | None = None
    accept_downloads: bool | None = None
    default_browser_type: str | None = None
    proxy: ProxySettings | None = None
    record_har_path: str | None = None
    record_har_omit_content: bool | None = None
    record_video_dir: str | None = None
    record_video_size: ViewportSize | None = None
    storage_state: str | dict | None = None
    base_url: str | None = None
    strict_selectors: bool | None = None
    service_workers: Literal['allow', 'block'] | None = None
    record_har_url_filter: str | None = None
    record_har_mode: Literal['minimal', 'full'] | None = None
    record_har_content: Literal['attach', 'embed', 'omit'] | None = None
```

### Example Usage

```python
from browser_use.browser.context import BrowserContextConfig, ViewportSize, Geolocation

context_config = BrowserContextConfig(
    viewport=ViewportSize(width=1920, height=1080),
    user_agent="Custom User Agent String",
    locale="en-US",
    timezone_id="America/New_York",
    geolocation=Geolocation(latitude=40.7128, longitude=-74.0060),
    permissions=["geolocation"],
    color_scheme="dark"
)
```

## Customizing Agent and Browser Behavior

To customize the behavior of the agent and browser sessions, you can combine these configuration options when initializing your Browser-use session. Here's an example that puts it all together:

```python
from browser_use.agent.views import AgentSettings
from browser_use.browser.browser import Browser, BrowserConfig
from browser_use.browser.context import BrowserContextConfig, ViewportSize

# Configure agent settings
agent_settings = AgentSettings(
    use_vision=True,
    max_failures=5,
    max_input_tokens=64000,
    tool_calling_method='function_calling'
)

# Configure browser settings
browser_config = BrowserConfig(
    browser_class='chromium',
    headless=False,
    disable_security=True,
    extra_browser_args=['--disable-extensions']
)

# Configure browser context settings
context_config = BrowserContextConfig(
    viewport=ViewportSize(width=1920, height=1080),
    user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36",
    locale="en-US",
    timezone_id="Europe/London"
)

# Initialize the browser with custom configurations
browser = Browser(config=browser_config)

# Create a new context with custom configurations
context = await browser.new_context(config=context_config)

# Use the configured browser and context in your agent
# ... (agent initialization and usage code here)
```

By using these configuration options, you can fine-tune the behavior of your Browser-use sessions to meet your specific requirements, such as adjusting viewport sizes, setting custom user agents, configuring proxy settings, and more.