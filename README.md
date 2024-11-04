# Multi-Agent Workflow with LangChain and LangSmith Tracing

## Overview
This project demonstrates the creation of a **multi-agent workflow** using LangChain, LangGraph, and LangSmith for tracing and monitoring. It consists of a supervisor agent that manages two worker agents (`Researcher` and `Coder`), allowing dynamic task assignment and routing. Each agent can perform specific tasks (e.g., web research or code execution) and report back to the supervisor for further instructions.

The project uses LangChain's ecosystem to integrate language models, manage workflows, and trace execution steps for debugging and performance optimization.

## Project Components
- **Supervisor Agent**: Manages the workflow, assigning tasks to worker agents and deciding when the workflow is complete.
- **Researcher Agent**: A worker agent that performs search queries using the `TavilySearchResults` tool.
- **Coder Agent**: A worker agent capable of running Python code using `PythonREPLTool`.
- **LangSmith Integration**: Tracks and monitors workflow execution, providing observability for debugging and performance improvements.

## Setup Instructions

### Prerequisites
- **Python 3.8+**
- **API Keys**: OpenAI, Tavily, and LangChain API keys are required.
- **LangSmith Account**: Sign up for a LangSmith account to enable tracing.

### Installation
1. Clone the repository or download the project files.
2. Install the required Python packages:
   ```bash
   pip install -U langgraph langchain langchain_openai langchain_experimental langsmith pandas
   ```

### Environment Configuration
Define the required API keys and environment variables. You can set them in your script or use environment variables.

1. **Set API Keys**:
   ```python
   import os
   import getpass

   def set_api_key(var_name):
       os.environ[var_name] = getpass.getpass(f"Enter your {var_name}: ")

   set_api_key("OPENAI_API_KEY")
   set_api_key("TAVILY_API_KEY")
   set_api_key("LANGCHAIN_API_KEY")
   ```

2. **LangSmith Tracing Variables**:
   ```python
   os.environ["LANGCHAIN_TRACING_V2"] = "true"
   os.environ["LANGCHAIN_ENDPOINT"] = "https://api.smith.langchain.com"
   os.environ["LANGCHAIN_API_KEY"] = "your-langchain-api-key"
   os.environ["LANGCHAIN_PROJECT"] = "Multi Agent"
   ```

### Running the Workflow
1. **Define Agents and Workflow**:
   - Set up the agents (supervisor, researcher, and coder) using `LangChain` tools.
   - Create a workflow with conditional routing based on agent responses.

2. **Start the Workflow Execution**:
   ```python
   # Invoke the workflow
   for step in graph.stream({
       "messages": [
           HumanMessage(content="code hello world and print it to the terminal")
       ]
   }):
       if "__end__" not in step:
           print(step)
           print("----")
   ```

### Tracing and Observability with LangSmith
1. Log in to your LangSmith dashboard.
2. Verify that the `LANGCHAIN_PROJECT` matches the name of your project in LangSmith.
3. Start the workflow as described above, then refresh your LangSmith dashboard to view traces and logs for debugging and performance analysis.

## Code Structure

- **Agent Definitions**: Sets up the `Researcher` and `Coder` agents, each with their specific tools.
- **Workflow Graph**: Defines a `StateGraph` that manages the task flow between agents and the supervisor.
- **Tracing and Debugging**: Integrates LangSmith for real-time tracking and debugging of workflow execution.

## Challenges Faced

1. **Rate Limit Issues**: Encountered API rate limits during development, which required careful management of API calls and verification of usage quotas.
2. **Environment Variable Persistence**: Setting environment variables in Python required using `os.environ`, which is different from setting them in a shell or terminal session.
3. **LangSmith Integration**: Initially, the LangSmith dashboard did not display tracing data due to incorrect environment configuration. Adjusting the API key and project name resolved this issue.
4. **Workflow Management**: Managing complex routing logic in the `StateGraph` was challenging, particularly with ensuring conditional edges worked as expected.

## Future Improvements

- **Advanced Error Handling**: Implement more robust error handling for different failure scenarios.
- **Enhanced Observability**: Add custom metrics or additional logging to track agent performance in more detail.
- **Scalability**: Explore adding more agents or tools to handle more complex workflows.
