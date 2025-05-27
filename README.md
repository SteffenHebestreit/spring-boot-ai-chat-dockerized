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

2.  **Set up Environment Variables:**
    This project requires API keys for OpenAI services.
    *   Create a `.env` file in the root of the project by copying the `.env.example` file:
        ```bash
        cp .env.example .env
        ```
    *   Edit the `.env` file and add your actual API keys and configuration:
        ```
        OPENAI_API_BASEURL=your_openai_api_baseurl_here
        OPENAI_API_KEY=your_openai_api_key_here
        OPENAI_API_MODEL=your_openai_model_here
        ```
        Refer to the `backend/src/main/resources/application.properties` for details on what these variables configure.

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

## Services

*   **Backend (`backend`):**
    *   Built using `Dockerfile_backend`.
    *   Clones from `https://github.com/SteffenHebestreit/spring_boot_ai_agent.git`.
    *   Accessible on `http://localhost:8080`.
    *   Requires OpenAI API environment variables (see `.env` file).

*   **Frontend (`frontend`):**
    *   Built using `Dockerfile_frontend`.
    *   Clones from `https://github.com/SteffenHebestreit/ai-chat-frontend.git`.
    *   Accessible on `http://localhost:3000` (maps to container port 4173).

## Environment Variables

The backend service requires the following environment variables, which are loaded from the `.env` file:

*   `OPENAI_API_BASEURL`: The base URL for the OpenAI-compatible API.
*   `OPENAI_API_KEY`: Your API key.
*   `OPENAI_API_MODEL`: The specific model to be used (e.g., `google_gemma-3-12b-it`).

Make sure your `.env` file is correctly populated before running `docker-compose up`. The `.env.example` file shows the required variables.

**Note:** The `.env` file should be added to your `.gitignore` to prevent committing secrets to your repository.
