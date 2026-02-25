# HelloAgents: Building a Lightweight Agent Framework from Scratch

[![Python](https://img.shields.io/badge/Python-3.10+-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Teaching-friendly · Minimal · Extensible** — Understand how Agent systems really work by building one from scratch

---

## 🎯 Philosophy

HelloAgents is not another production-heavy framework. It's a set of **LEGO bricks**:
- Core modules < 1000 lines of code, no magic
- Every component is independently understandable
- Progressive learning: from SimpleAgent to ReAct

### Why Build Your Own Framework?

| Pain Points of Existing Frameworks | HelloAgents Approach |
|-----------------------------------|---------------------|
| Black box, hard to debug | Completely open, readable code |
| Bloated dependencies, breaking changes | Only Pydantic + requests |
| Difficult to customize | Inherit base classes, add features in 50 lines |
| Over-abstraction | Design decisions documented, verbose comments |

### Five Core Design Principles

- **🎯 Minimalism**: Readability over feature completeness
- **🔌 Uniform Abstraction**: Standard interfaces for LLM, Agent, Tool
- **🏗️ Open/Closed**: Open for extension, closed for modification
- **📖 Teaching-First**: Each module testable in isolation
- **🔄 Progressive Compatibility**: Seamless switch between local/cloud models

---

## 🏗️ Architecture Overview

```
User Input
    ↓
Config (Global · Singleton)
    ↓
Agent Base (Template Method · run interface)
    ↓
LLM Layer (Multi-provider · Auto-detection)
    ↓
Tool System (Everything is a Tool + Registry + Chain)
    ↓
Message History (Message Class · Persistable)
    ↓
Final Output + Reflection/Iteration
```

### Core Abstractions (Study Guide)

#### 📦 Config Class
- **Purpose**: Centralized configuration management
- **Design**: Pydantic + Singleton pattern
- **Features**: `.env` override support, type validation

#### 💬 Message Class
- **Purpose**: Unified message format
- **Fields**: content, role, timestamp, tool_call_id, metadata
- **Method**: `to_dict()` → OpenAI compatible format

#### 🧬 Agent Base Class
- **Design Pattern**: Template Method
- **Core Method**: `run()` is template, subclasses implement `_execute()`
- **Common Features**: Message history management, streaming, history clearing

---

## 🤖 LLM Extension Layer

### Motivation
Support any LLM without locking the framework to OpenAI. Users only need to change `.env`, no code modification.

### Auto-detection Mechanism (Priority)
1. Explicit provider specification
2. base_url pattern matching (`localhost:11434` → Ollama)
3. api_key format guessing (`sk-` → OpenAI)

### Supported Model Types
- **OpenAI**: GPT series
- **Ollama**: Zero-config local models
- **vLLM**: High-throughput inference
- **Extensible**: Gemini, Claude, DeepSeek, etc.

---

## 🧠 Agent Paradigms (5 Classics)

| Paradigm | Core Idea | Use Case |
|----------|-----------|----------|
| **SimpleAgent** | Basic LLM call + optional tools | Simple conversations, single-turn tasks |
| **ReActAgent** | Thought → Action → Observation loop | Multi-step reasoning tasks |
| **ReflectionAgent** | Execute → Reflect → Improve | High-quality output scenarios |
| **PlanAndSolveAgent** | Plan steps first, then execute | Complex task decomposition |
| **FunctionCallAgent** | Native tool calling + parameter parsing | Precise tool parameters needed |

### Core Flows (Study Guide)

**ReActAgent Loop**:
```
1. Model outputs Thought
2. Parse Action
3. If tool call → Execute → Return Observation
4. Continue loop until Final Answer
```

**ReflectionAgent Flow**:
```
1. Generate initial answer
2. Model reflects on its answer (find issues)
3. Optimize based on feedback
4. Repeat 2-3 until satisfied
```

**PlanAndSolveAgent Flow**:
```
1. Decompose task into steps
2. Execute each step sequentially
3. Synthesize results
```

---

## 🔧 Tool System

### Core Design: Everything is a Tool

**BaseTool Abstract Class**:
- Attributes: `name`, `description`, `parameters` (JSON Schema)
- Methods: `execute(params)` → str
- Methods: `to_schema()` → OpenAI function calling format

### Three Core Components

#### 1. ToolRegistry
- **Purpose**: Register, retrieve, execute tools
- **Core Methods**: `register()`, `get()`, `execute()`
- **Helpers**: `get_descriptions()` (for models), `get_openai_schema()` (for OpenAI)

#### 2. ToolChain
- **Purpose**: Chain multiple tools sequentially
- **Feature**: Variable substitution (`{{prev_output}}`, `{{step_0}}`)
- **Use Case**: Fixed workflows (e.g., search → summarize → translate)

#### 3. AsyncToolExecutor
- **Purpose**: Parallel tool execution with thread pool
- **Use Case**: Independent tools called simultaneously

---

## 📊 Project Strengths & Limitations (Review)

### ✅ Strengths
- **High teaching value**: Small codebase, clear logic, learn in 1-2 days
- **Highly extensible**: Add new Agents or Tools by inheritance
- **Lightweight**: No heavy dependencies, pip install and go
- **Good compatibility**: OpenAI interface + multi-LLM support

### ❌ Limitations
- No advanced features (long-term memory, multi-modal, distributed)
- Tool returns str limits structured data expression
- Not performance-optimized (no caching, no GPU acceleration)

---

## 🗺️ Knowledge Map (Review)

```
┌─────────────────────────────────────┐
│        HelloAgents Knowledge         │
├─────────────────────────────────────┤
│                                     │
│   ┌──────────────┐                  │
│   │  Config Layer│  ← Singleton      │
│   │   (Config)   │     .env support  │
│   └──────────────┘                  │
│            ↓                         │
│   ┌──────────────┐                  │
│   │  Message Layer│ ← Pydantic Model │
│   │   (Message)  │    role/content   │
│   └──────────────┘    timestamp      │
│            ↓                         │
│   ┌──────────────┐                  │
│   │   Agent Layer│ ← Base (Template) │
│   │  (5 Paradigms)│    run/_execute   │
│   └──────────────┘   history mgmt    │
│       ↗  ↓  ↖                        │
│   ┌─────┐ ┌─────┐ ┌─────┐           │
│   │ LLM │ │Tool │ │ ... │           │
│   │Layer│ │System│ │     │           │
│   └─────┘ └─────┘ └─────┘           │
│                                     │
└─────────────────────────────────────┘
```

### Core Concepts Cheat Sheet

| Concept | Definition | Key Points |
|---------|------------|------------|
| **Agent** | Entity that thinks and uses tools | Inherit base, implement _execute |
| **LLM** | Large Language Model for reasoning | Unified interface, auto-detection |
| **Tool** | Functional module Agent can call | Everything is a tool, unified execute |
| **Message** | Basic unit of conversation history | Pydantic model, unified format |
| **Registry** | Central tool management | Register, get, execute |
| **Chain** | Tool sequence with variable substitution | Fixed workflows |

---

## 🎯 Interview Preparation (高频考点)

### 一、基础概念类

#### Q1: What is an Agent? How is it different from an LLM?
**A**: LLM is the brain, Agent is brain + hands/feet. LLM only generates text, Agent can:
- Autonomously plan tasks
- Call external tools
- Remember conversation history
- Self-correct based on feedback

#### Q2: What's the core idea of the ReAct paradigm?
**A**: Interleaving reasoning and action. Each loop:
1. **Thought**: Reason about current state and next action
2. **Action**: Execute a specific action
3. **Observation**: Observe result, start next loop

#### Q3: What is Function Calling?
**A**: Model outputs structured parameters, framework executes the function. Key points:
- Provide tool descriptions and parameter schemas
- Model learns to "request" tool calls
- Return results for final answer generation

### 二、架构设计类

#### Q4: How to design an extensible Agent framework?
**A**: Core is "programming to interfaces":
- **LLM Abstraction**: Unified chat/stream interface, multi-provider
- **Tool Abstraction**: All functions inherit BaseTool, unified execute
- **Agent Base**: Template Method pattern, fixed run flow
- **Registry**: Central tool management

#### Q5: How to implement "Everything is a Tool"?
**A**: Wrap all functional modules as Tools:
- Memory Tool: Read/write memory
- RAG Tool: Retrieve from knowledge base
- Calculator Tool: Math operations
- API Tool: Call external services
Unified interface means Agent doesn't need to know "what type" it's calling.

#### Q6: How to handle tool call errors?
**A**: Multi-layer fault tolerance:
1. **Tool-level try-catch**: Return friendly error messages
2. **Registry-level catch**: Standard error format for missing tools
3. **Agent-level retry**: Decide based on error message
4. **Final fallback**: Tell user "temporarily unavailable"

### 三、实现细节类

#### Q7: How to parse model tool calls?
**A**: Support multiple formats:
- **JSON**: `{"tool": "search", "params": {"q": "weather"}}`
- **XML**: `<tool name="search"><param name="q">weather</param></tool>`
- **Natural language**: Regex match "I want to search for..."
Production: Use JSON or OpenAI native function calling.

#### Q8: How to manage multi-turn conversation history?
**A**:
- Use Message class, store in list
- Support sliding window (last N turns)
- Persist to database if needed
- Key: Tool calls must be stored in history for context

#### Q9: How to implement streaming output?
**A**:
- LLM layer: `stream_chat()` returns generator
- Agent layer: `stream_run()` yields tokens
- Frontend: Show "thinking..." effect
- Note: Tool calls usually wait for complete output

### 四、优化与挑战类

#### Q10: What if Agent falls into infinite loop?
**A**:
- **Max steps**: `max_steps=10` force stop
- **Duplicate detection**: Stop if same thought repeats
- **Cost control**: Stop when token limit exceeded
- **Human intervention**: Provide "stop" button

#### Q11: How to improve tool call accuracy?
**A**:
1. Clear tool descriptions
2. Parameter examples in description
3. Few-shot examples in system prompt
4. Choose models with function calling support
5. Validation layer for parameter correction

#### Q12: How to avoid conflicts in multi-tool collaboration?
**A**:
- **ToolChain**: Define fixed execution order
- **Dependency declaration**: Tool A output → Tool B input
- **Variable substitution**: Reference previous results
- **Parallel execution**: AsyncExecutor for independent tools

### 五、对比分析类

#### Q13: HelloAgents vs LangGraph/AutoGen?

| Dimension | HelloAgents | LangGraph/AutoGen |
|-----------|------------|-------------------|
| **Purpose** | Teaching framework | Production framework |
| **Code size** | < 1000 lines | Tens of thousands |
| **Learning curve** | Gentle, 1-2 days | Steep, weeks |
| **Extension** | Inheritance | Graph/Flow config |
| **Use case** | Learning, experiments, prototypes | Complex production systems |
| **Dependencies** | Minimal | Many |

#### Q14: When to build your own Agent framework?
**A**:
- ✅ Want to deeply understand Agent principles
- ✅ Teaching or training needs
- ✅ Specific scenarios need extreme minimalism
- ✅ Existing frameworks can't meet custom needs
- ❌ Production complex apps → use mature frameworks
- ❌ Limited maintenance resources → use mature frameworks

### 六、场景应用题

#### Q15: Design a "Research Assistant" Agent. What components needed?
**A**:
1. **LLM**: GPT-4 or Claude (strong reasoning)
2. **Tools**:
   - WebSearchTool: Latest info
   - CalculatorTool: Data analysis
   - SummarizerTool: Long text summarization
   - SaveTool: Save results
3. **Paradigm**: PlanAndSolve (plan research steps first)
4. **Memory**: Store search history and intermediate results
5. **Reflection**: Self-evaluate after each step

#### Q16: How to test Agent reliability?
**A**:
1. **Unit tests**: Test each tool independently
2. **Integration tests**: Test full flow, mock external APIs
3. **Golden dataset**: 100 typical questions with human-annotated answers
4. **Automated evaluation**: GPT-4 scoring (1-5)
5. **Boundary tests**: Fuzzy input, empty input, long input
6. **Stress tests**: Run multiple Agents concurrently

---

## 🚀 Quick Start

```bash
# 1. Install
pip install hello-agents

# 2. Configure .env
echo "OPENAI_API_KEY=sk-..." > .env

# 3. Run example
python examples/basic_agent.py
```

### Three-Step Workflow
1. **Configure**: Config reads `.env`
2. **Register**: ToolRegistry registers tools
3. **Create**: Agent.create(paradigm, config, tools)

---

## 📈 Project Status

**Alpha · Teaching Ready**

✅ Completed:
- Core abstractions (Config, Message, Agent base)
- Multi-LLM support
- 5 Agent paradigms
- Tool system (Registry, Chain)

🚧 Planned:
- Long-term memory (vector DB)
- Plugin system
- Multi-modal support

---

## 📄 License

MIT © [jjyaoao](https://github.com/jjyaoao)

## ❤️ Learning Resources

- Repository: [https://github.com/jjyaoao/helloagents](https://github.com/jjyaoao/helloagents)
- Tutorial: Follow commits for progressive learning
- Interview Prep: 16 questions above cover 80% of common topics
- Discussion: Issues for ideas and questions
