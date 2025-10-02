# Multi-Agent Supervisor System

A sophisticated AI workflow orchestration system built with LangGraph that intelligently routes tasks through specialized agents to deliver accurate, comprehensive responses.

## Overview

This project implements a supervisor-based multi-agent architecture where a central supervisor coordinates three specialized agents to handle different types of user requests. The system automatically determines the best agent for each task and validates responses before returning results.

## Architecture

### Agents

The system consists of five key nodes working in harmony:

1. **Supervisor**: The orchestrator that analyzes requests and routes them to the most appropriate specialist
2. **Enhancer**: Transforms vague or ambiguous queries into clear, actionable requests
3. **Researcher**: Gathers information from the web using Tavily search
4. **Coder**: Handles technical implementations, calculations, and code execution
5. **Validator**: Ensures response quality and determines workflow completion

### Workflow

```
User Query → Supervisor → [Enhancer/Researcher/Coder] → Validator → [Supervisor/END]
```

The supervisor intelligently routes requests based on analysis, specialists process the task, and the validator ensures quality before either completing the workflow or routing back for refinement.

## Features

- **Intelligent Routing**: Supervisor analyzes context and selects optimal agent path
- **Query Enhancement**: Automatically clarifies ambiguous requests without user intervention
- **Web Research**: Real-time information gathering using Tavily search
- **Code Execution**: Python code execution for calculations and technical solutions
- **Quality Validation**: Built-in response validation to ensure accuracy
- **Iterative Refinement**: Automatic re-routing if responses need improvement

## Requirements

```
langgraph
langchain
langchain-openai
langchain-community
langchain-experimental
python-dotenv
pydantic
requests
youtube-transcript-api
faiss-cpu
pymupdf
arxiv
tavily-python
```

## Setup

1. Clone the repository

2. Install dependencies:
```bash
pip install langgraph langchain langchain-openai langchain-community langchain-experimental python-dotenv pydantic requests youtube-transcript-api faiss-cpu pymupdf arxiv tavily-python
```

3. Create a `.env` file with your API keys:
```
OPENAI_API_KEY=your_openai_key
TAVILY_API_KEY=your_tavily_key
```

4. Run the Jupyter notebook:
```bash
jupyter notebook Supervisor_multiagent.ipynb
```

## Usage

### Basic Example

```python
inputs = {
    "messages": [
        ("user", "Weather in Chennai"),
    ]
}

for event in app.stream(inputs):
    for key, value in event.items():
        if value and "messages" in value:
            last_message = value["messages"][-1]
            print(f"Output from node '{key}':")
            print(last_message)
```

### Example Queries

**Weather Information:**
```python
inputs = {"messages": [("user", "Weather in Chennai")]}
```
The supervisor routes to the researcher, who fetches real-time weather data.

**Mathematical Calculations:**
```python
inputs = {"messages": [("user", "Give me the 20th fibonacci number")]}
```
The supervisor routes to the coder, who calculates and returns the result (6765).

**Ambiguous Queries:**
```python
inputs = {"messages": [("user", "Tell me about it")]}
```
The supervisor routes to the enhancer, who clarifies the query before processing.

## How It Works

### 1. Supervisor Decision Making

The supervisor uses structured output to determine routing:

```python
class Supervisor(BaseModel):
    next: Literal["enhancer", "researcher", "coder"]
    reason: str
```

It analyzes the conversation history and selects the most appropriate specialist with a clear rationale.

### 2. Agent Processing

Each specialist agent has a specific role:

- **Enhancer**: Refines queries using GPT-4o without asking follow-up questions
- **Researcher**: Uses Tavily search tool to gather current information
- **Coder**: Executes Python code using PythonREPLTool for calculations

### 3. Validation

The validator ensures quality by:
- Comparing the original question with the final answer
- Accepting "good enough" responses to avoid over-iteration
- Only routing back if the answer is completely off-topic or incorrect

## Project Structure

```
.
├── Supervisor_multiagent.ipynb  # Main implementation notebook
├── .env                          # API keys (not in repo)
└── README.md                     # This file
```

## Advanced Features

### State Management

Uses LangGraph's `MessagesState` for tracking conversation history across agent transitions.

### Error Handling

The validator acts as a quality gate, preventing poor responses from reaching the user.

### Extensibility

Easy to add new specialist agents by:
1. Creating a new node function
2. Adding it to the supervisor's routing options
3. Registering it in the graph

## Performance

- **Average response time**: 5-15 seconds depending on complexity
- **Success rate**: High accuracy due to validation layer
- **Iteration efficiency**: Typically completes in 1-2 cycles

## Limitations

- Requires OpenAI and Tavily API keys
- Code execution limited to Python
- Research limited to Tavily search results
- Sequential processing (agents don't work in parallel)

## Future Enhancements

- Add parallel agent execution for independent subtasks
- Implement memory persistence across sessions
- Add more specialized agents (e.g., image analysis, data visualization)
- Implement streaming responses for real-time feedback
- Add human-in-the-loop approval for sensitive operations

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

## Acknowledgments

Built with:
- [LangGraph](https://github.com/langchain-ai/langgraph) - Agent orchestration framework
- [LangChain](https://github.com/langchain-ai/langchain) - LLM application framework
- [OpenAI](https://openai.com) - Language model provider
- [Tavily](https://tavily.com) - Search API provider
