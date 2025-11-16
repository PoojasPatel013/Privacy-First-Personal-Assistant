# Privacy-First-Personal-Assistant

## Track: Concierge Agents

1. The Pitch (Problem, Solution, Value)
Problem: Standard personal assistants have a major flaw: they save everything you say by default. This creates a privacy nightmare, as users have no control over whether sensitive information (like health data, family birthdays, or private thoughts) is stored permanently.

Solution: This agent inverts the model. It's a "Privacy-First" assistant that assumes all information is temporary. It is built to detect sensitive information and must ask for explicit user permission before saving anything to its long-term memory.

Value: This gives the user true control and builds trust. The agent can still be helpful and remember important facts (like an allergy), but only after the user gives explicit consent via a Human-in-the-Loop (HITL) workflow.

2. The Implementation (Architecture & Key Concepts)
This agent demonstrates three advanced concepts from the course:

Long-Running Operations (LRO): The core of the privacy feature. The process_user_fact tool detects sensitive keywords. If found, it calls tool_context.request_confirmation() to pause the agent's execution and wait for the user's "yes/no" response.

Long-Term Memory: If (and only if) the user approves the LRO, the tool resumes and saves the session to the InMemoryMemoryService using add_session_to_memory. The preload_memory tool is used in all future sessions to retrieve these approved facts.

Context Engineering (Compaction): If the user rejects the save, the fact is not saved to long-term memory. We also enable EventsCompactionConfig to ensure the rejected fact is quickly removed from the short-term session history, making the agent truly "forget" it.
