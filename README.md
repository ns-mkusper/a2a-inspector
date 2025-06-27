# A2A Protocol Inspector

The A2A Inspector is a web-based tool designed to help developers inspect, debug, and validate servers that implement the Google A2A (Agent-to-Agent) protocol. It provides a user-friendly interface to interact with an A2A agent, view communication, and ensure specification compliance.

The application is built with a FastAPI backend and a TypeScript frontend.

## Features

- **Connect to a local A2A Agent:** Specify the base URL of any agent server to connect (e.g., `http://localhost:5555`).
- **View Agent Card:** Automatically fetches and displays the agent's card.
- **Spec Compliance Checks:** Performs basic validation on the agent card to ensure it adheres to the A2A specification.
- **Spec Compliance Checks:** Performs basic validation on the agent card to ensure it adheres to the A2A specification.
- **Token-Based Authentication:** Optionally provide a JWT to authenticate requests to the agent server.
- **Live Chat:** A chat interface to send and receive messages with the connected agent.
- **Debug Console:** A slide-out console shows the raw JSON-RPC 2.0 messages sent and received between the inspector and the agent server.
- **File Upload:** Attach PDF documents to your messages; the inspector will base64‑encode and stream them alongside your chat as file parts.

## Prerequisites

- Python 3.10+
- [uv](https://github.com/astral-sh/uv)
- Node.js and npm

## Project Structure

This repository is organized into two main parts:

- `./backend/`: Contains the Python FastAPI server that handles WebSocket connections and communication with the A2A agent.
- `./frontend/`: Contains the TypeScript and CSS source files for the web interface.

## Setup and Running the Application

Follow these steps to get the A2A Inspector running on your local machine. The setup is a three-step process: install Python dependencies, install Node.js dependencies, and then run the two processes.

### 1. Clone the repository

```sh
git clone https://github.com/google-a2a/a2a-inspector.git
cd a2a-inspector
```

### 2. Install Dependencies

First, install the Python dependencies for the backend from the root directory. `uv sync` reads the `uv.lock` file and installs the exact versions of the packages into a virtual environment.

```sh
# Run from the root of the project
uv sync
```

Next, install the Node.js dependencies for the frontend.

```sh
# Navigate to the frontend directory
cd frontend

# Install npm packages
npm install

# Go back to the root directory
cd ..
```

### 3. Run the Application

The application requires two processes to run concurrently: the frontend build process (in watch mode) and the backend server.

**In your first terminal**, run the frontend development server. This will build the assets and automatically rebuild them when you make changes.

```sh
# Navigate to the frontend directory
cd frontend

# Build the frontend and watch for changes
npm run build -- --watch
```

**In a second terminal**, run the backend Python server.

```sh
# Navigate to the backend directory
cd backend

# Run the FastAPI server with live reload
uv run app.py
```

### 4. Access the Inspector

Once both processes are running, open your web browser and navigate to:
**[http://127.0.0.1:5001](http://127.0.0.1:5001)**

- Enter the URL of your A2A server agent that needs to be tested.

## JWT Authentication

If your A2A agent server requires JWT authentication, enter your token in the JWT field on the home page before clicking **Connect**. The provided JWT will be included as a Bearer token in the Authorization header for all agent requests and chat messages.

After clicking **Connect**, open the Debug Console (using the “Debug Console” toggle) to inspect the `auth` debug log entry, which shows the HTTP headers (including your Bearer token) that the inspector will use for subsequent requests.

You should also see a **REQUEST** debug log entry for `initialize_client`; check its `data.jwt` field to ensure your token was actually sent over the socket.

> Note: the Debug Console is cleared each time you click **Connect**, so it will show only this session’s request/auth logs.
