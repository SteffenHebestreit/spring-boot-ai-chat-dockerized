# AI Research Project

This project consists of a backend service built with Spring Boot and a frontend service. It is designed to be run using Docker Compose.

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
    This project uses a dedicated application.properties file for backend configuration.
    *   Create an `application.properties` file in the root of the project by copying the example file:
        ```bash
        copy application.properties.example application.properties
        ```
    *   Edit the `application.properties` file to add your actual API keys and customize the configuration.
    
    *Note: Any changes to application.properties will be applied without rebuilding the container.*

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
    You can update the `application.properties` file directly and the changes will be applied to the backend without rebuilding the container (restart may be required for some changes):
    ```bash
    # Edit application.properties with your changes
    docker-compose restart backend
    ```

## Services

*   **Backend (`backend`):**
    *   Built using `Dockerfile_backend`.
    *   Clones from `https://github.com/SteffenHebestreit/spring_boot_ai_agent.git`.
    *   Accessible on `http://localhost:8080`.
    *   Configurable through `application.properties` which is mounted as a volume for easy updates.
    *   Supports Agent-to-Agent (A2A) protocol and Model Context Protocol (MCP) for tool integration.

*   **Frontend (`frontend`):**
    *   Built using `Dockerfile_frontend`.
    *   Clones from `https://github.com/SteffenHebestreit/ai-chat-frontend.git`.
    *   Accessible on `http://localhost:3000` (maps to container port 4173).
    *   Connects to the backend using the `VITE_BACKEND_API_URL` environment variable.
    *   Features a modern UI with animated visualization and markdown support.

## Configuration Properties

The backend configuration is managed through `application.properties`. Here's an explanation of the key configuration sections:

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
agent.integrations.mcp-servers[0].name=ExternalMCP1
agent.integrations.mcp-servers[0].url=http://external-mcp-server1.com/api

agent.integrations.mcp-servers[1].name=AnotherMCP
agent.integrations.mcp-servers[1].url=http://another-mcp.com/mcp
```

### External A2A Agents (Peers)

```properties
agent.integrations.a2a-peers[0].name=PeerAlpha
agent.integrations.a2a-peers[0].url=http://localhost:8081

agent.integrations.a2a-peers[1].name=ServiceAgentBeta
agent.integrations.a2a-peers[1].url=http://localhost:8082
```

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
