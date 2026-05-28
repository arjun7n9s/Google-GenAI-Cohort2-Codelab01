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