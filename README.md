# AI Research Project

This project consists of a backend service built with Spring Boot and a frontend service. It is designed to be run using Docker Compose.

## Recent Updates (June 15, 2025)

### Latest Commit: Update Dockerfile and RoleImport
- **Enhanced WebCrawl MCP Docker Configuration**: Added XDG environment variables (`XDG_CONFIG_HOME` and `XDG_DATA_HOME`) for improved Chromium browser support in containerized environments
- **AI Agent Role Configuration**: Added `role.yml` file with comprehensive AI websearch agent prompt template
- **Volume Mount Integration**: The `role.yml` file is now mounted as a volume for easy configuration updates without rebuilding containers
- **Tool Integration**: Enhanced configuration for webSearch, smartCrawl, crawlWithMarkdown, generateSitemap, extractLinks, and searchInPage tools

## Prerequisites

*   Docker
*   Docker Compose

## Getting Started

1.  **Clone the repository (if you haven't already):**
    ```bash
    # This project uses pre-defined repositories in the Dockerfiles
    # Ensure Dockerfiles point to the correct repositories if you are not the original author
    ```

2.  **Set up Configuration:**
    This project uses dedicated configuration files for backend and AI agent configuration.
    *   Create an `application.properties` file in the root of the project by copying the example file:
        ```bash
        copy application.properties.example application.properties
        ```
    *   Edit the `application.properties` file to add your actual API keys and customize the configuration.
    *   The `role.yml` file contains the AI websearch agent prompt template and tool configuration.
    
    *Note: Any changes to application.properties or role.yml will be applied without rebuilding the container.*

## Building and Running with Docker Compose

1.  **Build and run the services:**
    From the root directory of the project (where `docker-compose.yml` is located), run:
    ```bash
    docker-compose up --build
    ```
    To run in detached mode (in the background):
    ```bash
    docker-compose up --build -d
    ```

2.  **Stopping the services:**
    To stop the services (if running in detached mode or in another terminal):
    ```bash
    docker-compose down
    ```

3.  **Updating the configuration without rebuilding:**
    You can update the `application.properties` or `role.yml` files directly and the changes will be applied to the backend without rebuilding the container (restart may be required for some changes):
    ```bash
    # Edit application.properties or role.yml with your changes
    docker-compose restart backend
    ```

## Services

*   **Backend (`backend`):**
    *   Built using `Dockerfile_backend`.
    *   Clones from `https://github.com/SteffenHebestreit/spring_boot_ai_agent.git`.
    *   Accessible on `http://localhost:8080`.
    *   Configurable through `application.properties` and `role.yml` which are mounted as volumes for easy updates.
    *   Supports Agent-to-Agent (A2A) protocol and Model Context Protocol (MCP) for tool integration.
    *   Depends on the WebCrawl MCP service for web crawling capabilities.
    *   The `role.yml` file contains AI websearch agent prompt templates and tool configurations.

*   **Frontend (`frontend`):**
    *   Built using `Dockerfile_frontend`.
    *   Clones from `https://github.com/SteffenHebestreit/ai-chat-frontend.git`.
    *   Accessible on `http://localhost:3000` (maps to container port 4173).
    *   Connects to the backend using the `VITE_BACKEND_API_URL` environment variable.
    *   Features a modern UI with animated visualization and markdown support.

*   **WebCrawl MCP (`webcrawl-mcp`):**
    *   Built using `Dockerfile_mcp_webcrawl`.
    *   Clones from `https://github.com/SteffenHebestreit/webcrawl-mcp.git`.
    *   Accessible on `http://localhost:3001` (internal port 3000).
    *   Provides web crawling capabilities via Model Context Protocol (MCP).
    *   Includes Puppeteer for browser automation and web scraping.
    *   Configured with resource limits (1GB memory, 1 CPU) for stability.
    *   Enhanced with improved Chromium configuration using XDG environment variables for better containerized browser support.

## Technical Improvements (Latest Updates)

### WebCrawl MCP Docker Enhancements

The latest update includes significant improvements to the WebCrawl MCP Docker configuration:

#### Chromium Browser Support
- **XDG Environment Variables**: Added `XDG_CONFIG_HOME=/tmp/.chromium` and `XDG_DATA_HOME=/tmp/.chromium` for better containerized browser support
- **Puppeteer Configuration**: Enhanced Puppeteer setup with proper Chrome installation and caching
- **Resource Management**: Configured with 1GB memory and 1 CPU limits for optimal performance

#### Environment Variables
```dockerfile
ENV NODE_ENV=production \
    PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=false \
    PUPPETEER_CACHE_DIR=/app/.cache/puppeteer \
    XDG_CONFIG_HOME=/tmp/.chromium \
    XDG_DATA_HOME=/tmp/.chromium
```

### Configuration File Management

Both `application.properties` and `role.yml` are now mounted as Docker volumes:

```yaml
volumes:
  - ./application.properties:/app/config/application.properties
  - ./role.yml:/app/config/role.yaml
```

This allows for:
- **Hot Configuration Updates**: Changes take effect without rebuilding containers
- **Easy Customization**: Modify AI agent behavior and backend settings instantly
- **Version Control**: Keep configuration changes tracked in git

### AI Agent Tool Integration

The `role.yml` file provides comprehensive configuration for:
- **Primary Tools**: webSearch, smartCrawl, generateSitemap, extractLinks, searchInPage
- **Fallback Strategies**: Automatic fallback from smartCrawl to crawlWithMarkdown
- **Research Workflows**: Step-by-step instructions for complex research tasks
- **Example Use Cases**: Quantum computing research, academic paper analysis, and more

## Configuration Properties

The backend configuration is managed through `application.properties` and `role.yml`. Here's an explanation of the key configuration sections:

### AI Agent Role Configuration (role.yml)

The `role.yml` file contains the AI websearch agent prompt template that defines:
- **Role Definition**: Specialized search agent capabilities
- **Tool Configuration**: Integration with webSearch, smartCrawl, crawlWithMarkdown, generateSitemap, extractLinks, and searchInPage tools
- **Step-by-Step Instructions**: Detailed workflow for comprehensive research tasks
- **Fallback Strategies**: Primary and secondary tool usage patterns (smartCrawl → crawlWithMarkdown)
- **Example Workflows**: Practical examples for quantum computing research and other complex topics

This configuration is mounted as a volume and can be updated without rebuilding the container.

### OpenAI API Configuration

```properties
openai.api.baseurl=https://api.openai.com/v1
openai.api.key=your_api_key_here
openai.api.model=gpt-4
```

### Agent Card Configuration

```properties
agent.card.id=ai-research-agent-2025
agent.card.name=Research Assistant Agent
agent.card.description=An advanced AI research assistant...
agent.card.url=http://localhost:8080/research-agent/api
agent.card.provider.organization=Your Organization
agent.card.provider.url=https://example.com
agent.card.contact_email=contact@example.com
```

### H2 Database Configuration

```properties
spring.datasource.url=jdbc:h2:./data/airesearch
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

### External MCP Servers

```properties
agent.integrations.mcp-servers[0].name=WebCrawlMCP
agent.integrations.mcp-servers[0].url=http://webcrawl-mcp:3000
```

*Note: The WebCrawl MCP service is now integrated as part of the Docker Compose stack and provides web crawling capabilities to the AI agent.*

### File Upload Configuration

```properties
spring.servlet.multipart.max-file-size=5MB
spring.servlet.multipart.max-request-size=8MB
```

### LLM Configurations

```properties
# Qwen 3 (text-only model with tool use support)
llm.configurations[0].id=qwen_qwen3-14b
llm.configurations[0].name=Qwen 3 14B
llm.configurations[0].supportsText=true
llm.configurations[0].supportsImage=false
llm.configurations[0].supportsPdf=false

# Google Gemma 3 (vision-enabled model with image support)
llm.configurations[1].id=google_gemma-3-12b-it@q4_k_s
llm.configurations[1].name=Google Gemma 3 12B
llm.configurations[1].supportsText=true
llm.configurations[1].supportsImage=true
llm.configurations[1].supportsPdf=false

# DeepSeek R1 0528 Qwen3 8B (text and reasoning model)
llm.configurations[2].id=deepseek-r1-0528-qwen3-8b
llm.configurations[2].name=DeepSeek R1 0528 Qwen3 8B
llm.configurations[2].supportsText=true
llm.configurations[2].supportsImage=false
llm.configurations[2].supportsPdf=false
```

## Frontend Configuration

The frontend configuration is managed through Docker Compose environment variables:

```yaml
frontend:
  build:
    args:
      - VITE_BACKEND_API_URL=http://localhost:8080/research-agent/api
  environment:
    - VITE_BACKEND_API_URL=http://localhost:8080/research-agent/api
```

To change the frontend configuration, update the values in `docker-compose.yml` and rebuild the frontend container.
