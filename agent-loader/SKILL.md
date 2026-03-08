---
name: agent-loader
description: Code, analyse, refactor and deploys python agents using Google's Agent Development Kit (ADK), an agentic framework
---

# ADK Python

Agent Development Kit (ADK) is a flexible and modular framework for developing and deploying AI agents.
While optimized for Gemini and the Google ecosystem, ADK is model-agnostic, deployment-agnostic, and is built for compatibility with other frameworks. 
ADK was designed to make agent development feel more like software development, to make it easier for developers to create, deploy, and orchestrate agentic architectures that range from simple tasks to complex workflows.

## When to use this skill

- Use this when the user wants to create, analyse or refactor an agentic app using ADK

## How to use ADK

- Check if you have a recent downloaded copy of https://github.com/google/adk-docs from the last 24 hours
- If not, always download the repo https://github.com/google/adk-docs into a temporary directory and use it for reference

### Installation and set up
- The root directory of the workshpace should have a python virtual environment created using `python -m venv .venv`
- If you find a .venv, activate it and use it for all python code.
- If you do not find a .venv, create one
- Install or update the python package `google-adk` using `pip install --upgrade google-adk`
- In the root directory, every standalone agent will be in its own directory

#### Creating an agent
- Make sure you are in the root direcotry of the wokspace
- Run `gcloud config list` to get a the currently active project
- Do a google search for the latest Gemini Pro model in Vertex that is generally available, go to the page and find the model id, like `gemini-2.5-pro`
- Run this command to create the agent `adk create --model MODEL_ID --project PROJECT_ID --region us-central1 my_agent`

### Coding the agent
- In the location where you downloaded the adk-docs repository, recursively search through adk-docs/tree/main/docs/agents path to find all documentation on how to code an ADK agent in Python

### Running/testing the agent locally when you can launch a browser UI
- From the workspace root directory, run the `adk web --reload_agents --verbose` command
- Open the browser and navigate to http://127.0.0.1:8000
- In the top left dropdown, select the agent from the dropdown
- In the right handside panel, type the message to the agent in the text input in the bottom

### Running/testing the agent locally when you do not have a browser UI and are in a terminal
- From the workspace root directory, run `adk run AGENT_DIRECTORY`
- Type in the inputs to the `[user]` prompt and wait for the agent responses.
- Type `exit` to exit

### Deploying the agent to Agent Engine - the default if the user just says deploy
- Edit the .env file for the agent being deployed to add these two lines `OTEL_INSTRUMENTATION_GENAI_CAPTURE_MESSAGE_CONTENT=true` and `GOOGLE_CLOUD_AGENT_ENGINE_ENABLE_TELEMETRY=true`
- Make sure you are in the workspace root directory
- Run the command `adk deploy agent_engine --agent_engine_id AGENT_NAME --trace_to_cloud --otel_to_cloud --display_name AGENT_NAME --description AGENT_DESCRIPTION `

### Deploying the agent to Cloud Run
- Edit the .env file in the agent directory for the agent being deployed to add these two lines `OTEL_INSTRUMENTATION_GENAI_CAPTURE_MESSAGE_CONTENT=true` and `GOOGLE_CLOUD_AGENT_ENGINE_ENABLE_TELEMETRY=true`
- Make sure you are in the workspace root directory
- If the agent name has underscores, replace with hyphens as Cloud Run service_name does not allow underscores in name
- Run the command `adk deploy cloud_run --project PROJECT_NAME --region us-central1 --service_name AGENT-NAME-WITH-HYPHEN-NO-UNDERSCORE --trace_to_cloud --otel_to_cloud --with_ui --log_level debug --region us-central1 AGENT_FOLDER`

