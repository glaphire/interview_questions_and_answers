# Vocabulary

*Note: explanations are given for the Claude
- **AGI**: Artificial General Intelligence, a hypothetical AI 
    that can perform any intellectual task that a human can do.Key features:
  - General learning: it solves problems in areas it was never taught before
  - Adaptability: it moves skills from one topic to another like a person.
  - Autonomy: it works on its own without needing new programming for every task.
- **Context**: information a model has available when generation 
    a response - the input it's actively "paying attention to". 
    This includes current prompt, prior messages in the conversation and other data provided to inform the output.
- **Context window**: maximum amount for text (in tokens) a model can process at once. It has fixed limit.
- **Evals**: short for "evaluations" is the process of measuring how well a model performs specific tasks.
   An eval measures how often or how well the system gets the right answer across a batch of test cases. 
   Read more [here](https://www.thoughtworks.com/insights/decoder/a/ai-evals)
- **GenAI**: Generative AI - refers to AI systems that create new content (texts, images, code etc.) rather than
   just analyzing or classifying existing data.
- **Hallucination**: a situation when model generates information that is false, fabricated or not grounded in its input
   or reality - while presenting it as true.
- **Harness**: its a software and the code that connects model to the tools, manages inputs and outputs and orchestrates
   multistep behavior.
   <br/>Harness typically handles:
   - Tool integration: access to functions, API, file systems, external services etc.;
   - Context management - handling memory across steps and managing what to include in each model call;
   - Control loop - the logic that lets the model take multiple turns (act, observe result, act again);
   - Parsing and formatting - interpreting the models outputs and format into structured actions;
   - Error handling and guardrails - retrying failures, enforcing limits, keeping system on track.
- **Human-in-the-loop**: model or control pattern where humans actively participate in the AI's training, operation,
   supervision, or decision-making process.
- **LLM**: Large Language Model - advanced AI system trained on massive amounts of data of text to understand,
  summarize, translate and generate human-like language.
- **Loop**: Agent Loop - it's a runtime cycle in which the agent evaluates its current state, chooses an action,
  executes it, observes the result, and updates its plan for the next step, then it repeats this process. The reasoning 
  usually runs through LLM, which proposes the next action, and the runtime executes it within whatever permissions the
  agent has been given. [Source](https://www.jetbrains.com/pages/ai-agents/architecture/ai-agent-loops/#what-is-an-ai-agent-loop-)
- **MCP**: Model Context Protocol - open standard to give AI models a universal way to connect with external tools,
  data sources, and workflows.
- **Model**: computer program trained on massive dataset to recognize patterns, make decisions or generate new content
  without human intervention. LLM is a specialized type of model focused exclusively on text and language.
- **Prompt**: input (text, code, image etc.) given to AI system to guide it to produce a specific response.
- **RAG**: is a technique that improves AI responses by retrieving relevant information from an external knowledge source
  and feeding it into the model's context before it generates an answer - rather than relying solely on what 
  the model learned during training.
- **Spec-driven development (SDD)**: is an approach where a detailed specification serves as the central driving artifact
  of the development process. The typical flow moves through stages: defining the *spec* (requirements and intent), 
  often breaking it into a *plan* (technical approach and architecture), then a *task* breakdown, 
  and finally implementation 0 with the spec remaining the anchor that everything traces back to. Examples: Github Spec Kit,
  OpenSpec, Kiro.

### Engineering:
- **Prompt Engineering**: approach that focuses on crafting the individual input to get a good output. 
  It's about wording, structure, examples, and instructions within a single prompt (or a few). The unit of work is the message.
- **Context Engineering**: Approach that focuses on designing what information model sees on each step - curating, structuring
  and managing the contents of the context window so the model has exactly what it needs to perform.
  Context engineering asks "what should be in the window at all, and in what form?". The unit of work is the context payload:
  everything assembled intho the models input for a given call. Context engineering is fully covered under 
  Claude Code's internals and core architecture.
- **Loop Engineering**: approach with the focus on *control loop* - the cycle of act -> observe -> act again. The unit 
  of work is the iteration cycle. The main concern is how model iterates: when loop continues or stops, how errors are
  caught and retried. Loop engineering is covered by internal agent harness in Claude Code, Codex etc.
- **Agentic Engineering**: approach of designing a full autonomous system that pursues goal and has minimum human intervention.
  It covers the loop, tool selection, planning and task decomposition, memory, multiagent coordination, guardrails,
  and the overall architecture (harness). The unit of work is the whole agent it it's behavior over and extended task.
  

### Claude:
- **Agent**: autonomous software system that uses AI to pursue goals, make multi-step plans and take actions using 
  external tools. 
  <br/>AI agent operates in a continuous loop consisting of three main parts:
  - *Observe* - the agent collects data from its environment of user instructions and retains context using memory.
  - *Plan* - it uses a LLM as its brain to break a major goal down into smaller, sequential steps.
  - *Act* - It uses external tools, databases or APIs to execute the plan, check its results and 
    adjust its steps until the task is complete.
- **Subagent**: a specialized, secondary AI instance spawned and directed by a primary orchestrator agent 
  to handle a specific, bounded subtask within a larger workflow.
  <br/>Core characteristics:
  - *Isolated Context Window* - to prevent cluttering main thread with intermediate data;
  - *Specialized instructions* - Configured with a focused role, custom system prompt and specific goals 
    (e.g. code review, QA testing) 
  - *Targeted tool access* - equipped only with the specific tool permissions required for its individual assignment.
  - *Delegation and synthesis* - executes tasks independently or in parallel, returning only distilled results back 
    to the parent orchestrator.
- **Skill**: a modular, reusable package of instructions, tools and workflows that teaches and AI agent how to perform
  a specific, repeatable task.
- **Hook**: programmable checkpoint or automated script that executes at specific moment in an agent's lifecycle to observe,
  log, modify or block actions.
  <br/><br/>How Hooks work:
  - *Triggers* - a defined event in the workflow, such as session startup, right before the prompt is submitted, 
    or after code is generated.
  - *Actions* - a shell command, HTTP endpoint, or fast auxiliary prompt that runs automatically without relying
  on the primary AI decision's making.
  - *Enforcement* - returns an exit code or status that can hard-block unauthorized or risky operations before they execute.
  <br/><br/>Common Use Cases:
  - *Security & Compliance*: Running automated vulnerability scans or blocking force-pushes and dangerous commands.
  - *Guardrails and Governance*: acting as programmatic middleware to enforce organizational policies without needing 
    constant human oversight.
  - *Workflow automation*: Triggering auto-formatting, logging audit trails, or enriching context dynamically.

