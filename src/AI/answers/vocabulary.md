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
- **Agentic Engineering**: 
- **Loop Engineering**:
- **Prompt Engineering**:

### Claude:
- **Agent**:
- **Subagent**:
- **Skill**:
- **Hook**:

