---
name: agent-loader
description: Code, analyse, refactor and deploys python agents using Google's Agent Development Kit (ADK), an agentic framework and A2A protocol and manage those agents in Agent Engine, Cloud Run and register them in Gemini Enterprise (formerly Agentspace, informally GemEnt or Gem Ent).
---

# ADK, Agent Engine, A2A protocol and Gemini Enterprise
Agent Development Kit (ADK) is a flexible and modular framework for developing and deploying AI agents.
While optimized for Gemini and the Google ecosystem, ADK is model-agnostic, deployment-agnostic, and is built for compatibility with other frameworks. 

Agent Engine is a serverless runtime for deploying ADK agents on.
Read the page at https://docs.cloud.google.com/agent-builder/agent-engine/overview to know more about Agent Engine.

ADK agents can also be deployed to Cloud Run.

Multiple deployments of ADK agents (or non-ADK agents) can communicate with each other using the A2A protocol.

Gemini Enterprise is Google's is a portal where employees can access agents running on Agent Engine or Cloud Run or remotely over A2A provided those agents are registered in Gemini Enterprise.

## When to use this skill
- To create, analyse or refactor an agent using ADK
- To enable A2A client/server features in an agent
- To test an ADK agent
- To deploy to and manage an ADK agent on Agent Engine or Cloud Run
- To list and manage agents on Agent Engine
- To manage the registration of Agent Engine or Cloud Run or A2A agents on Gemini Enterprise

## Saving and reading placeholder variables as configurations 
- Whenever you discover the value of a placeholder variable, like PROJECT_ID for e.g., save that to agent-loader.cfg file in the current directory
- Never store OAUTH_CLIENT_ID and OAUTH_CLIENT_SECRET into the config file. Always ask user to provide those values or provide a file containing the values.
- Whenever you need to find the value of a placeholder variable, check in the agent-loader.cfg first to see if there is an existing known value.

### Frequently used placeholder variables and how to find the values
- Run `gcloud config list` to get the id of the currently active project, PROJECT_ID
- Run `gcloud projects list` to get the corresponding project number, PROJECT_NUMBER
- For MODEL_ID, use the default value `gemini-2.5-flash`

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

### Running/testing the agent remotely when you can launch a browser UI
- From the workspace root directory, run the `adk web --reload_agents --verbose ` command
- Open the browser and navigate to http://127.0.0.1:8000
- In the top left dropdown, select the agent from the dropdown
- In the right handside panel, type the message to the agent in the text input in the bottom

### Running/testing the agent remotely when you do not have a browser UI and are in a terminal
- From the workspace root directory, run `adk run AGENT_DIRECTORY --log_level debug `
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

# A2A Protocol Python
A2A provides the definitive common language for agent interoperability in a world where agents are built using diverse frameworks and by different vendors.

## When to use this skill
- Use this when the user wants to create, analyse or refactor agents that communicate with each other using A2A protocol

## How to use A2A
- Check if you have a recent downloaded copy of https://github.com/a2aproject/A2A from the last 24 hours
- If not, always download the repo https://github.com/a2aproject/A2A into a temporary directory and use it for reference

### Installation and set up
- The root directory of the workshpace should have a python virtual environment created using `python -m venv .venv`
- If you find a .venv, activate it and use it for all python code.
- If you do not find a .venv, create one
- Install or update the python package `a2a-sdk` using `pip install --upgrade a2a-sdk`

# Agent Engine

## When to use this skill
- Use this when the user wants to list, update or delete Agent Engine instances

## Typical operations
- Agent Engine, at this time, does not have a CLI, so you need to use REST APIs below
- Before each and every invocation of the REST API, always generate an ACCESS_TOKEN first by running `gcloud auth print-access-token`
- Below are a list of typical actions you can perform with Gemini enterprise

### Listing available Agent Engine instances
- Run the following curl command:
    curl -X GET \
        -H "Authorization: Bearer ACCESS_TOKEN" \
        -H "Content-Type: application/json" \
        -H "X-Goog-User-Project: PROJECT_ID" \
        -H "https://REGION-aiplatform.googleapis.com/v1/projects/PROJECT_ID/locations/REGION/reasoningEngines"
- Then print their name, displayName and createTime and updateTime

# Gemini Enterprise

## When to use this skill
- Use this when the user wants to see, manage or update agent registrations in Gemini Enterprise (previously known as Agentspace).

## Typical operations
- Gemini Enterprise, at this time, does not have a CLI or client library, so you need to use REST APIs to get things done with Gemini Enterprise
- Before each and every invocation of the REST API, always generate an ACCESS_TOKEN first by running `gcloud auth print-access-token`
- Below are a list of typical actions you can perform with Gemini enterprise

### Listing available Gemini Enterprise instances available
- Run the following curl command:
    curl -X GET \
        -H "Authorization: Bearer ACCESS_TOKEN" \
        -H "Content-Type: application/json" \
        -H "X-Goog-User-Project: PROJECT_ID" \
        "https://global-discoveryengine.googleapis.com/v1alpha/projects/PROJECT_ID/locations/global/collections/default_collection/engines/"
- Then filter to only those results that have "appType": "APP_TYPE_INTRANET" and print their name, displayName and createTime
- If there are none, inform the user that no Gemini Enterprise instance was found and ask them to create one
- If there are multiple Gemini Enterprise instances, ask the user to select one.
- Then save the name as GEM_ENT_NAME in the config file

### Listing all agents currently registered with a Gemini Enterprise instnance
- Check if there is a value for GEM_ENT_NAME. If not, follow "Listing available Gemini Enterprise instances available" above.
- After getting a value for GEM_ENT_NAME, run the following curl command:
    curl -X GET \
        -H "Authorization: Bearer ACCESS_TOKEN" \
        -H "Content-Type: application/json" \
        -H "X-Goog-User-Project: PROJECT_ID" \
        "https://global-discoveryengine.googleapis.com/v1alpha/GEM_ENT_NAME/assistants/default_assistant/agents"