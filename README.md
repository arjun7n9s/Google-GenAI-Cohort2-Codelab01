# Google GenAI Cohort 2 - ADK Codelab Labs

My personal repository for the Google GenAI Academy APAC Cohort 2 Codelab. 

## Log & Updates

### 🧞 Session 1: Your First Agent with Runner

I successfully completed the setup and ran the first agent! Here is a simple explanation of what I did:

1. **Set up the starter repo**: Pulled the initial lab code from `cuppibla/ADK_Basic` to my local workspace.
2. **Environment Setup**: Created a Python 3.12 virtual environment and installed the google-adk library and other required dependencies.
3. **API Key Integration**: Created a root `.env` file and configured my Google AI Studio API key.
4. **Fixing the Quota Bug**: Originally the codebase was using `gemini-2.0-flash`, which was hitting a daily free quota limit. I upgraded the model definition in `a_single_agent/day_trip.py` to `gemini-2.5-flash` which is newer and has fresh free tier quota limits.
5. **Testing the Agent**: Fired up the local ADK web interface with `adk web` and created a fresh chat session. I asked the agent for a weekend trip to chill out around Bangalore in this hot summer. The agent used its built-in Google Search capability to construct a perfect, budget-aware day-trip suggestion to Chikmagalur (with precise coordinates and a details log).

![Session 1 Test Output](screenshots/session1_dating_planner.png)

---

### 🛠️ Session 2: Custom Tools

I successfully built and integrated custom Python functions as tools inside the agent! Here is a simple explanation of what I did:

1. **Created Custom Python Tools**: Defined two custom Python functions (`save_user_preferences` and `recall_user_preferences`) that take `tool_context: ToolContext` to read/write state directly to the session memory.
2. **LLM Schema Generation**: Handled how the ADK parses custom function signatures and docstrings to automatically generate JSON schemas so the LLM knows how to call them.
3. **Equipped the Agent**: Bound these custom tools to the `MemoryCoordinatorAgent` under `f_agent_with_memory/agents.py`.
4. **Execution and State persistence**: Fired up the agent and told it my preferences: *"I love eating Hyderabadi biryani and watching movies"*. Behind the scenes, the agent successfully executed the `recall_user_preferences` tool, routed the request to a specialist planner to suggest **Bagara's - Hyderabadi Biryani** and **AMC Sunnyvale 12**, and then executed the `save_user_preferences` tool to write these new preferences directly to the SQLite session database.

![Session 2 Test Output](screenshots/session2_custom_tools.png)

#### Persistent Session State & Tool Execution Details:
| Stored Preferences in sqlite DB | Tool Call Graph & JSON Response |
| :---: | :---: |
| ![Session 2 State](screenshots/session2_custom_tools_state.png) | ![Session 2 Trace Graph](screenshots/session2_custom_tools_trace.png) |

---

### 🧱 Session 3: Agent-as-a-Tool (Multi-Agent Architectures)

I successfully built and executed a hierarchical multi-agent orchestration system where agents use other specialized agents as tools! Here is a simple explanation of what I did:

1. **Created Specialized Specialist Agents**: Defined two expert sub-agents:
   - `LocationScoutAgent`: An expert finder tasked with finding specific venues matching a user query using Google Search and returning only its name.
   - `LogisticsValidatorAgent`: A travel coordinator designed to find travel times between points or retrieve operating hours.
2. **Built the Trip Architect (Orchestrator)**: Configured the root `TripArchitectAgent` and wrapped the specialized sub-agents inside `AgentTool` wrappers. The architect follows a self-correcting cognitive loop:
   - Deconstructs the user request, finds a primary activity, finds a meal venue, and calculates travel time.
   - **Self-Correction Loop**: If travel time exceeds 20 minutes, it automatically discards the venue, queries the scouts again for a closer restaurant, and outputs the final plan without user intervention.
3. **Execution & Trace Validation**:
   - Manually launched the server in my terminal and successfully connected to the local ADK Web UI.
   - Provided the prompt: *"I want to visit a technology museum and eat at a moderately priced South Indian restaurant in Jabalpur."*
   - Behind the scenes, the root agent dynamically spawned tool-calls to `LocationScoutAgent` (which discovered **AOC Museum** and **SSS Veg South Indian Restaurant**) and `LogisticsValidatorAgent` (which calculated a travel time of **15 minutes**).
   - The architect verified the travel time was within the 20-minute threshold and returned a completed, structured itinerary.

| Successful Multi-Agent Execution | Terminal Command Logs & Web Server |
| :---: | :---: |
| ![Session 3 Web UI](screenshots/session3_agent_as_tool.png) | ![Session 3 Terminal Command Logs](screenshots/session3_terminal.png) |

---

### ⛓️ Session 4: Sequential Chaining (Linear Workflows)

I successfully configured and executed a linear multi-agent workflow where agents communicate sequentially by sharing a persistent session state! Here is a simple explanation of what I did:

1. **Created Sub-Agent Roles**:
   - `foodie_agent`: Specialized in finding high-quality dining/venue suggestions. I configured it with `output_key="destination"` so its final response is automatically saved in the session's shared state under that key.
   - `transportation_agent`: Designed to act as a navigation helper. By using the `{destination}` placeholder in its instructions, it dynamically retrieves the destination from the session state and plans routes from the user's start location.
2. **Linear Pipeline Orchestration**: Grouped both agents under a `SequentialAgent` named `find_and_navigate_agent`. This ensures the first agent completely finishes and writes to state before the second agent starts.
3. **Execution & Trace Verification**:
   - Submitted the prompt: *"I am at Secundrabad railway station and I want to watch IPL at Uppal Stadium (Rajiv Gandhi International Cricket Stadium)."*
   - First, `foodie_agent` resolved the stadium as **Rajiv Gandhi International Cricket Stadium (Uppal Stadium)**.
   - Second, `transportation_agent` injected the stadium name, parsed "Secundrabad railway station" as the start, searched public transit details, and generated detailed routes (Hyderabad Metro Blue Line, bus, and taxi directions).
   - The trace logs successfully verified the clean, sequential execution flow.

![Session 4 Sequential Trace](screenshots/session4_sequential.png)

---

## Starter Resources
All the original labs, agent folders, and workflow setup files are included in this workspace.

- `a_single_agent/` - Basic agent configuration
- `b1_sequential_agent/` - SequentialAgent setup
- `b2_parallel_agent/` - ParallelAgent setup
- `b3_loop_agent/` - LoopAgent setup
- `b4_manual_sequential_flow/` - Manual routing and sequentially executing agents
- `c_custom_agent/` - Creating custom agent setups
- `d_routing_agent/` - Router agent setup
- `e_agent_as_tool/` - Agent-as-a-Tool implementation
- `f_agent_with_memory/` - Context-aware agent setup with conversational memory