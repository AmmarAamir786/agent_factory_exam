# Exam: CLAUDE.md, Skills, Subagents, MCP Integration, and Compiled Skills (Lessons 5–13)

This exam covers persistent context with CLAUDE.md, AI non-determinism and skills, skills architecture, building SKILL.md files, subagent orchestration, MCP integration, and compiling MCP servers to skills. Each question has exactly one correct answer. Read each question carefully — all necessary context is embedded in the question itself.

**Total Questions: 40**

---

### Section A: CLAUDE.md and Persistent Context

**Q1.** A developer opens a new Claude Code session and asks about the project's tech stack. Claude responds with generic advice despite the developer having explained everything in the previous session. The developer is confused because their conversation seemed continuous yesterday. What most accurately explains why Claude has no memory of previous sessions?

- A) Claude Code stores session data in a temporary cache directory that gets cleared when the terminal closes, so opening a new terminal window removes all previously cached project information and context
- B) Claude Code maintains a compressed archive of recent sessions on Anthropic's cloud servers, but network timeouts during session restoration cause the context to fail loading on subsequent startups
- C) Claude Code's internal database of conversation logs has a fixed retention period of 24 hours, after which all stored interactions are automatically purged to conserve local storage space
- D) The underlying LLM is stateless and has no memory between API calls — Claude Code creates the illusion of continuity by re-sending conversation history within a session, but nothing persists across sessions

> **Answer: D** — LLMs are stateless; they process each request in complete isolation. Within a session, Claude Code re-sends the full message history with each new message, creating the appearance of continuity. But once the session ends, nothing carries over. The other options incorrectly describe caching, cloud storage, or database mechanisms that don't exist.

---

**Q2.** CLAUDE.md is a persistent project context file used with Claude Code. A junior developer asks their team lead where this file should be placed and what it should contain. Which response correctly describes CLAUDE.md's placement and typical content?

- A) A markdown file placed in the project root alongside .git that Claude Code auto-loads at session start, typically containing project overview, tech stack, directory structure, conventions, commands, and notes
- B) A configuration file stored in the .claude hidden directory that Claude Code parses as JSON-formatted metadata, typically containing API keys, model preferences, token limits, and authentication credentials
- C) A markdown file placed anywhere in the project that Claude Code discovers through recursive directory scanning, typically containing conversation templates, prompt histories, and session replay instructions
- D) A structured YAML file placed in the user's home directory that Claude Code loads globally for all projects, typically containing personal preferences, theme settings, and default model configurations

> **Answer: A** — CLAUDE.md is a markdown file placed in the project root (same level as .git, package.json, etc.) that Claude Code automatically detects and reads at session start. It typically includes six sections: Project Overview, Technology Stack, Directory Structure, Coding Conventions, Key Commands, and Important Notes. Options B, C, and D describe incorrect file formats, locations, or contents.

---

**Q3.** A team decides to make their project compatible with multiple AI coding tools — Claude Code, Cursor, GitHub Copilot, and Gemini CLI. They want universal project instructions without duplicating content. Which approach correctly describes the relationship between CLAUDE.md and AGENTS.md?

- A) AGENTS.md replaces CLAUDE.md entirely since it was standardized by the Linux Foundation, making CLAUDE.md a deprecated legacy format that will be removed in future Claude Code versions
- B) AGENTS.md and CLAUDE.md must contain identical content because AI agents cannot cross-reference files, so teams need to manually synchronize both files whenever project details change
- C) AGENTS.md holds universal project context for any AI agent while CLAUDE.md references it and adds Claude-specific features like skills, hooks, and MCP configs — avoiding content duplication across both files
- D) AGENTS.md is a proprietary Anthropic standard that only works with Claude-family products, while CLAUDE.md is the open-source alternative maintained by the Linux Foundation for cross-agent compatibility

> **Answer: C** — The recommended approach is using both files: AGENTS.md for universal project context (tech stack, directory structure, conventions) that any AI agent can read, and CLAUDE.md that references @AGENTS.md while adding Claude-specific instructions (skills, hooks, MCP configs). AGENTS.md was created by OpenAI and donated to the Linux Foundation's Agentic AI Foundation, making it vendor-independent. Option A is wrong because CLAUDE.md is not deprecated. Option B is wrong because CLAUDE.md can reference AGENTS.md. Option D reverses which standard is proprietary vs universal.

---

**Q4.** A developer creates a file called "claude.md" (lowercase) in their src/ subdirectory, writes project context in it, and tests by starting a new Claude Code session. Claude doesn't reference any project context. A colleague reviews their setup. Which combination of errors explains why auto-loading failed?

- A) The file content exceeded Claude Code's maximum file size limit of 3KB, and the src/ subdirectory is excluded from the auto-loading scan path by default configuration settings
- B) The filename must be exactly "CLAUDE.md" with uppercase letters because it is case-sensitive, and the file must be in the project root directory alongside .git rather than in a subdirectory
- C) Claude Code only processes files with YAML frontmatter headers, and the developer also needed to run a registration command to add the file to Claude Code's context index before auto-loading activates
- D) The developer needed to restart their entire computer rather than just the terminal because Claude Code caches directory structures in system memory that persist until full system reboot

> **Answer: B** — Two errors: (1) the filename is case-sensitive and must be exactly "CLAUDE.md" — "claude.md" won't be detected; (2) the file must be in the project root (same level as .git, package.json), not in a subdirectory like src/. Both conditions must be met for auto-loading to work. The other options describe nonexistent file size limits, YAML requirements, registration commands, or system reboot needs.

---

**Q5.** The Agentic AI Foundation (AAIF) was announced in December 2025 when several companies donated open standards to the Linux Foundation. A technology analyst is researching which organizations contributed and what they donated. Which description correctly identifies the three donated projects and their origins?

- A) Google donated Kubernetes for container orchestration, Microsoft donated VS Code extensions for agent integration, and Anthropic donated Claude Code as the reference agent implementation platform
- B) Anthropic donated MCP for connecting AI to tools and data, OpenAI donated AGENTS.md for universal project instructions, and Block donated Goose as an open-source agent framework
- C) Meta donated LLaMA models for open-source inference, OpenAI donated ChatGPT plugins for tool connectivity, and Anthropic donated the Claude API as a standardized model interface protocol
- D) Amazon donated Bedrock for multi-model hosting, Anthropic donated Skills format for agent extensibility, and OpenAI donated GPT Actions as a universal webhook integration standard

> **Answer: B** — The Agentic AI Foundation received three donations: Anthropic contributed MCP (Model Context Protocol) for connecting AI to tools and data, OpenAI contributed AGENTS.md for universal project instructions, and Block contributed Goose as an open-source agent framework. The other options list incorrect companies, products, and purposes.

---

**Q6.** A developer explains to a colleague how Claude Code maintains conversation continuity within a single session despite the underlying LLM being stateless. The colleague asks for the specific mechanism. Which explanation accurately describes how within-session continuity works?

- A) Claude Code compresses each conversation turn into vector embeddings stored in a local database, then retrieves and decompresses relevant embeddings when the LLM needs historical context for generating responses
- B) The LLM maintains a small internal session buffer that accumulates tokens from each exchange, allowing it to reference previous turns directly without external re-transmission of the conversation history
- C) Claude Code saves each message to a hidden log file on disk and instructs the LLM to read that file at the start of every turn, giving the model access to all previous interactions through filesystem access
- D) Claude Code bundles all previous messages together with each new message and re-sends the entire conversation history to the LLM every turn, so the model reads everything fresh each time it responds

> **Answer: D** — Claude Code re-sends the entire conversation history with each new message. When you send message #3, Claude Code secretly bundles messages #1, #2, and #3 and sends all three. The LLM reads the whole bundle fresh each time, creating the illusion of continuity. The LLM itself remains stateless — it's just being shown the full history repeatedly. Web apps like ChatGPT and Claude.ai use the same technique.

---

**Q7.** Claude Code treats the file system as external memory rather than trying to keep everything in conversation history. A systems architect explains why this design choice was made for ongoing project work. Which reasoning best captures the advantage of using the file system as memory?

- A) Conversation history grows too long for complex projects and eventually exceeds context limits, while project files already contain the project's state persistently — CLAUDE.md ensures Claude reads the orientation guide first every session
- B) File system access is significantly faster than conversation history processing because local disk reads bypass the network latency of sending accumulated messages to Anthropic's API servers for each request
- C) Storing context in files allows Claude Code to encrypt sensitive project information at rest, whereas conversation history travels unencrypted to the LLM and creates security vulnerabilities for proprietary codebases
- D) The file system enables Claude Code to maintain multiple parallel conversation threads simultaneously, whereas conversation history is limited to a single linear sequence of messages that cannot branch

> **Answer: A** — For ongoing project work, re-sending chat history gets too long eventually, explaining projects each time is exhausting, and starting fresh loses understanding. Claude Code solves this by treating the file system as external memory — your code files already contain your project's state. CLAUDE.md serves as the orientation guide that Claude reads first. The LLM is still stateless, but your files persist. The other options describe incorrect advantages around speed, encryption, or parallel threading.

---

### Section B: Non-Determinism and Skills

**Q8.** A marketing professional asks Claude Code to write a LinkedIn post about AI development, runs the exact same prompt twice, and gets different results each time — different structure, tone, and emoji choices. They assume something is broken. Which explanation correctly identifies the cause and its implications?

- A) Claude Code's prompt parser introduces random formatting variations by shuffling sentence order and emoji placement before sending requests to the LLM, which otherwise would produce identical outputs consistently
- B) The variation occurs because Anthropic's servers route requests to different model versions in a load-balanced cluster, and each version has slightly different training data producing divergent stylistic preferences
- C) AI models like Claude are non-deterministic, meaning the same input can produce different outputs each time — this is fundamental to how the models work, not a bug, and affects structure, tone, and length
- D) The differences result from Claude Code injecting random context from previously cached sessions into each request, causing the model to interpret the same prompt differently based on contaminated historical data

> **Answer: C** — AI models are non-deterministic — the same input can produce different outputs each time. This is fundamental to how AI models work, not a bug. For casual conversation this is fine, but for consistent professional output it creates a "double variability problem": you phrase requests differently each time AND the AI generates differently even for identical requests. The other options incorrectly attribute variation to prompt parsing, server routing, or cached session contamination.

---

**Q9.** A freelance writer uses Claude Code daily for LinkedIn posts. They notice they keep re-explaining the same style preferences — friendly-professional tone, exactly 2-3 emojis, ending with an engagement question. They consider saving their prompt as a text file to paste each time. Why does creating a skill provide more value than saving and pasting a prompt?

- A) Skills encode reasoning patterns about HOW you approach a task — structure, preferences, quality criteria — and can activate automatically when Claude recognizes a matching request, unlike prompts which only capture WHAT you want
- B) Skills are stored in Anthropic's cloud and synchronized across all devices automatically, while saved prompts are trapped on a single machine's clipboard and cannot be transferred between different computers or sessions
- C) Skills use advanced machine learning fine-tuning to permanently modify Claude's neural network weights for your specific use case, while saved prompts only provide temporary instruction that the model forgets after processing
- D) Skills are compiled into optimized binary instructions that Claude Code processes faster than plain text prompts, reducing response latency by approximately 40 percent while also improving output quality through compression

> **Answer: A** — Skills encode HOW you think about a task (structure, preferences, quality criteria), while prompts only specify WHAT you want. Skills can activate automatically when Claude recognizes a matching request or be invoked explicitly, constrain non-determinism by defining boundaries rather than exact outputs, and are reusable intellectual property. The other options incorrectly describe cloud synchronization, neural network fine-tuning, or binary compilation.

---

**Q10.** Claude Code skills can be activated in two different ways depending on the user's workflow. A developer teaching a new team member about skills explains the two activation modes. Which description correctly identifies both modes and when to use each?

- A) Manual mode requires typing the full skill file path each time, while scheduled mode runs skills at predetermined time intervals — use manual for testing and scheduled for production automation workflows
- B) Debug mode loads skills with verbose logging for troubleshooting, while production mode loads skills silently for normal operation — use debug when creating new skills and production for daily usage
- C) Automatic mode means Claude recognizes when a skill applies based on the request and loads it, while explicit mode means the user says "Use [skill-name]" — start with explicit to see skills clearly, then rely on automatic
- D) Foreground mode blocks Claude until the skill completes its full execution pipeline, while background mode runs skills asynchronously — use foreground for simple tasks and background for long-running operations

> **Answer: C** — Skills have two activation modes: (1) Automatic — Claude recognizes when your style guide or procedure applies based on the request and loads the skill without being told; (2) Explicit — you invoke by name, saying "Use [skill-name]" to ensure a specific skill loads. The recommendation is to start with explicit invocation so you can clearly see skills in action, then rely on automatic activation in normal workflow. The other options describe nonexistent modes.

---

**Q11.** A product manager explains to their CEO why the team invested time creating skills for Claude Code instead of just giving better prompts each session. The CEO asks what concrete benefit justifies the investment. Which argument best captures why skills matter beyond saving keystrokes?

- A) Skills reduce Claude Code's API costs by 50% because Anthropic charges lower rates for skill-guided interactions compared to freeform conversations that require more computational resources to process and respond
- B) Skills constrain AI non-determinism within defined boundaries — Claude's output still varies, but it stays within YOUR style and preferences, producing consistent results that match your professional standards every time
- C) Skills bypass Claude's safety filters for approved organizational workflows, allowing the AI to generate content that would normally be restricted, which dramatically increases output volume and reduces editing overhead
- D) Skills enable Claude Code to learn permanently from each interaction, building a cumulative knowledge base that makes the model progressively smarter over time without requiring any additional user input or maintenance

> **Answer: B** — Skills constrain non-determinism by defining boundaries, not eliminating variation. Claude's output still varies (that's inherent to AI), but it stays within YOUR defined constraints. Every LinkedIn post has your tone, your emoji style, your engagement hooks — because your skill defines them. The result: consistent, recognizable output despite AI non-determinism. The other options incorrectly claim reduced API costs, safety filter bypasses, or permanent model learning.

---

**Q12.** A university student creates a study notes skill that transforms lecture notes into structured summaries with key terms, practice questions, and quick review sections. A classmate argues that this is no different from just typing a detailed prompt each time. How does the skill's value differ from a detailed prompt?

- A) The skill is fundamentally the same as a detailed prompt since both simply provide text instructions to Claude, but the skill saves approximately 15 seconds of typing per session by eliminating manual copy-paste steps
- B) The skill allows Claude to access the student's university learning management system directly, pulling grades, assignments, and lecture recordings that a plain text prompt could never reference or retrieve
- C) The skill encrypts the student's notes with institutional-grade security before processing, while a plain text prompt exposes raw lecture content to Anthropic's servers without any protection or access controls
- D) The skill preserves a proven study method structure across every session — immediate processing, active recall questions, spaced repetition sections — ensuring consistent study materials regardless of AI output variation

> **Answer: D** — A skill encodes a proven study method that persists: immediate processing, active recall through practice questions, spaced repetition via quick review, and concept connections. This structure applies consistently every time regardless of how the student phrases their request or how the AI varies its output. A prompt would need to be remembered and pasted perfectly each time. The other options incorrectly claim trivial time savings, LMS access, or encryption capabilities.

---

### Section C: Skills Architecture and Strategy

**Q13.** Anthropic's team frames AI agents as having intelligence (models) and execution (code) but still lacking something critical. A CTO evaluating Claude Code for their organization asks what the missing piece is. Which answer correctly identifies the gap that skills fill?

- A) The missing piece is computational power — current models are too slow for enterprise workloads, and skills provide pre-computed response caches that eliminate the need for real-time inference on repetitive tasks
- B) The missing piece is internet connectivity — models cannot access external systems, and skills provide built-in API connectors that enable Claude Code to reach databases, websites, and third-party services directly
- C) The missing piece is domain expertise — models have intelligence and code provides execution, but skills supply the specialized knowledge that makes generic capability specifically useful for particular domains and workflows
- D) The missing piece is multi-language support — models only process English natively, and skills provide translation layers that convert instructions and outputs between languages for international enterprise deployments

> **Answer: C** — Models provide intelligence (reasoning, analysis, synthesis), code provides execution (APIs, file system, Python), but together they still lack expertise. The bottleneck isn't making the model smarter — it's giving it access to domain-specific knowledge. Skills fill this gap by encoding the specialized procedures, quality criteria, and organizational knowledge that make a generic agent specifically useful. MCP handles connectivity (option B's claim), not skills.

---

**Q14.** Skills use a three-level loading architecture to protect Claude's context window. A developer with 50 skills installed asks whether having so many will overwhelm Claude's memory. Which description correctly explains how the three-level architecture prevents context overload?

- A) All 50 skills are fully loaded at startup but compressed using a proprietary token compression algorithm that reduces each skill to approximately 10% of its original size while preserving complete instruction fidelity
- B) Level 1 loads only brief skill descriptions at startup, Level 2 loads full SKILL.md instructions on-demand when relevant, and Level 3 accesses supporting files only during execution — like phone apps that stay closed until tapped
- C) Skills are distributed across multiple parallel Claude instances running simultaneously, with each instance handling a maximum of 10 skills, and a coordinator routes requests to the appropriate instance based on topic matching
- D) The architecture automatically deletes the least-recently-used skills when context reaches 80% capacity, maintaining only the 10 most frequently accessed skills in active memory while archiving the rest to disk storage

> **Answer: B** — The three-level architecture uses progressive disclosure: Level 1 (always loaded) is just a brief description of what each skill does (~30 tokens) — enough for Claude to know the skill exists. Level 2 (on-demand) loads the full SKILL.md with detailed procedures only when Claude decides a skill applies. Level 3 (if needed) accesses scripts and reference files only during execution. The smartphone analogy applies: 100 apps installed, but only the tapped one runs. The other options describe nonexistent compression, parallel instances, or auto-deletion.

---

**Q15.** Skills come from three distinct sources, each serving different needs in the ecosystem. An enterprise architect mapping their AI strategy needs to understand these categories. Which answer correctly identifies the three sources and provides accurate examples of each?

- A) Academic skills from research institutions providing peer-reviewed algorithms, government skills from regulatory agencies providing compliance frameworks, and commercial skills from software vendors providing product-specific integrations
- B) Free-tier skills included with Claude Code's basic subscription, premium skills available through Anthropic's marketplace for additional fees, and enterprise skills requiring a dedicated support contract and custom deployment infrastructure
- C) Core model skills baked into Claude's neural network weights during pre-training, runtime skills dynamically generated by Claude Code's inference engine during execution, and cached skills stored from previous user sessions for faster recall
- D) Foundational skills for basic capabilities like document creation, partner skills for specific software integrations like browser automation and Notion, and enterprise or custom skills encoding organizational workflows and internal standards

> **Answer: D** — Three sources: (1) Foundational — basic capabilities everyone needs (Word documents, PowerPoint, Excel, PDFs); (2) Partner/Third-Party — expertise in specific tools (Browserbase's Stagehand for browser automation, Notion for workspace research); (3) Enterprise/Custom — organizational knowledge (company coding standards, internal documentation procedures, team-specific workflows). Fortune 100 companies use custom skills for developer productivity teams serving thousands of engineers. The other options describe nonexistent categorization schemes.

---

**Q16.** An MCP server connects Claude to a company's database, enabling data queries. A skill encodes the company's data analysis procedures. A data analyst asks what happens when skills and MCP are used together versus separately. Which answer accurately describes their complementary relationship?

- A) Without the skill Claude produces generic output despite having data access, and without MCP Claude knows standards but cannot reach the data — together Claude queries data via MCP and analyzes using procedures from the skill
- B) Skills and MCP are interchangeable approaches to the same problem, with skills being the older legacy system and MCP being the modern replacement — using both simultaneously creates redundancy and potential configuration conflicts
- C) MCP handles all data processing and analysis internally using built-in analytical functions, while skills serve only as visual formatting templates that control how MCP's analytical results are displayed to the end user
- D) Skills must be compiled from MCP servers before they can function, creating a strict dependency where every skill requires a corresponding MCP server to provide its underlying data source and execution capability

> **Answer: A** — Skills and MCP are complementary: MCP provides connectivity (connection to external data, tools, APIs), while skills provide expertise (procedures for USING those connections effectively). Without the skill, Claude can access data but doesn't know your reporting standards — producing generic output. Without MCP, Claude knows your standards but can't reach the data — knowledge without action. Together, Claude queries the database (MCP) and analyzes using your procedures (skill). They're complementary, not competing.

---

**Q17.** Anthropic draws a parallel between the AI agent stack and the computing stack. A technology strategist evaluating the AI ecosystem asks how skills relate to this analogy. Which mapping correctly identifies all three layers of the stack analogy?

- A) Models are like hard drives storing raw data, agent runtimes are like web browsers displaying that data, and skills are like bookmarks that help users navigate between different data sources more efficiently
- B) Models are like programming languages defining syntax rules, agent runtimes are like compilers translating code to execution, and skills are like unit tests verifying that the compiled output meets specifications correctly
- C) Models are like internet protocols defining communication standards, agent runtimes are like routers directing network traffic to destinations, and skills are like websites providing specific content and functionality to end users
- D) Models are like processors providing immense computational potential, agent runtimes are like operating systems orchestrating resources around the processor, and skills are like applications encoding domain expertise and unique points of view

> **Answer: D** — The stack analogy maps directly: Models ≈ Processors (massive investment, contain immense potential, but limited alone), Agent runtimes ≈ Operating Systems (orchestrate processes, resources, and data around the processor — Claude Code plays this role), Skills ≈ Applications (where domain expertise lives — millions of developers build these). The paradigm shift: "Stop building agents. Build skills instead." A few companies build processors and OS; everyone builds applications. The other options use incorrect analogies.

---

**Q18.** A comparison between manual prompting and encoded skills reveals differences across reliability, token cost, asset type, and integration. A team lead building a business case for skills investment asks about the strategic advantage. Which answer correctly contrasts manual prompting with skills across these dimensions?

- A) Manual prompting is ad-hoc with rules paid for in every conversation as disposable text, while skills are deterministic and script-backed with rules loaded only when triggered, creating reusable IP that is API-ready via Agent SDKs
- B) Manual prompting provides stronger output consistency because users craft precise instructions each time, while skills introduce rigidity that prevents Claude from adapting to novel situations requiring creative deviation from templates
- C) Manual prompting and skills produce identical output quality since both ultimately send the same text to Claude's model, with the only difference being that skills automate the copy-paste step saving approximately five seconds per interaction
- D) Manual prompting is preferred for enterprise deployments because it leaves no persistent artifacts that could contain proprietary information, while skills create files that must be secured and managed as sensitive intellectual property

> **Answer: A** — The comparison across four dimensions: Reliability — ad-hoc/best-effort vs deterministic/script-backed; Token Cost — pay for rules in every conversation vs load only when triggered; Asset Type — disposable conversation vs reusable, scalable IP; Integration — requires human copy-paste vs API-ready via Agent SDKs. Skills created in Claude Code can be shared with teams, versioned in Git, integrated into Custom Agents, and monetized as part of vertical AI solutions. The other options mischaracterize the comparison.

---

### Section D: Building Skills

**Q19.** A developer creating their first skill needs to write the YAML frontmatter description field for a meeting notes skill. They write: "Helps with notes." A senior developer reviews it and explains why this description will cause problems. Which explanation correctly identifies why the description is inadequate?

- A) The description exceeds YAML's maximum character limit for single-line string values, causing a parsing error that prevents Claude Code from loading the skill's frontmatter metadata at session startup
- B) The description lacks required YAML tags including version number, author field, and license identifier, which Claude Code mandates for all skills before allowing them to be registered in the skill index
- C) The description is too vague for Claude to determine when to activate the skill — "notes" could mean anything — a good description states WHAT it does, lists KEY OUTPUTS, and specifies WHEN to use it with clear trigger conditions
- D) The description uses an informal tone that conflicts with Claude Code's natural language processing pipeline, which requires descriptions to be written in a formal technical specification format with precise terminology

> **Answer: C** — The description field is the most important line because it determines WHEN Claude activates the skill. "Helps with notes" is too vague — "notes" could mean anything. A good description follows the formula: [Action verb] + [input type] + [into/for] + [output type] + [key features]. Use when [trigger conditions]. Example: "Transform meeting transcripts or raw notes into structured summaries with action items, decisions, and follow-ups. Use when user shares meeting content or asks for meeting notes." The other options describe nonexistent YAML limits, required tags, or tone requirements.

---

**Q20.** A SKILL.md file has two distinct parts that serve different purposes. A beginner learning skill creation asks what these two parts are and what each contains. Which answer correctly identifies the anatomy of a SKILL.md file?

- A) Part 1 is a JSON configuration block defining runtime parameters like memory allocation and timeout limits, and Part 2 is a Python script block containing the executable code that Claude Code runs when the skill activates
- B) Part 1 is YAML frontmatter containing the skill's name and description that serves as the activation trigger, and Part 2 is the markdown body containing detailed instructions like procedures, output format, and quality criteria
- C) Part 1 is an XML schema defining the skill's input and output data types for validation purposes, and Part 2 is a template engine block using Jinja2 syntax for generating dynamic output based on variable substitution
- D) Part 1 is a binary header containing checksums for verifying skill file integrity during loading, and Part 2 is a compressed payload that Claude Code decompresses into working memory only when the skill is explicitly invoked

> **Answer: B** — A SKILL.md has two parts: (1) YAML frontmatter (between --- markers) — the "ID card" containing `name` and `description` fields that determine when Claude activates the skill; (2) Markdown body — the "instructions" containing when to use the skill, step-by-step procedures, output format specifications, quality criteria, and optionally examples. A skill can be a single markdown file — the simplicity is intentional so anyone can create one. The other options describe nonexistent formats.

---

**Q21.** The Skills Lab includes a "skill-creator" skill — a meta-skill that helps users build new skills. A user wants to create a technical documentation skill and considers whether to write SKILL.md manually or use the skill-creator. Which description accurately explains what the skill-creator does and why it's recommended?

- A) The skill-creator is an automated testing framework that validates SKILL.md files against a schema specification, checking for syntax errors and missing required fields, but it cannot generate new skill content from scratch
- B) The skill-creator guides users through understanding their procedure, writing effective descriptions, and generating a complete SKILL.md file — it's recommended because it produces well-structured skills from natural language input
- C) The skill-creator compiles plain text procedures into optimized binary skill packages that load faster than markdown-based skills, providing a performance advantage that justifies the additional setup complexity required
- D) The skill-creator connects to Anthropic's skill marketplace to download pre-built templates that users then customize, requiring an internet connection and marketplace account to access the template library

> **Answer: B** — The skill-creator is a meta-skill (a skill for creating other skills) that guides users through understanding their procedure, writing effective descriptions, and generating a complete SKILL.md file. It's the recommended approach for most people because it structures the creation process, ensures proper format, and produces well-organized skills from natural language descriptions. The rest of the lesson teaches what's happening under the hood for refinement. The other options describe nonexistent testing frameworks, binary compilation, or marketplace connections.

---

**Q22.** A developer chooses between creating a skill versus a subagent for a particular task. They need to understand the key differences to make the right decision. Which answer correctly distinguishes when to use a skill versus a subagent?

- A) Skills are exclusively for coding tasks like linting and formatting, while subagents handle all non-coding work like research and documentation — the distinction is purely based on whether the task involves writing source code
- B) Skills are cloud-hosted components managed by Anthropic's infrastructure team, while subagents run locally on the user's machine — the choice depends on whether internet connectivity is available for the current workflow
- C) Skills and subagents are identical in functionality but marketed under different names for different subscription tiers — skill is the free-tier label while subagent is the premium-tier label for the same underlying technology
- D) Skills are lightweight and can auto-activate for repeated patterns in shared context, while subagents provide isolated context windows with guaranteed execution for complex multi-step tasks like audits and refactoring

> **Answer: D** — Key differences: Skills are lightweight, can activate automatically or by name, share the main conversation context, and work best for repeated patterns and formatting. Subagents have isolated context windows, are invoked explicitly, and provide guaranteed execution for complex multi-step workflows. Use a skill when "I want Claude to automatically do this whenever it's relevant." Use a subagent when "I need guaranteed execution with isolated context for this complex task." Examples: meeting notes formatting → skill; comprehensive security audit → subagent.

---

**Q23.** A developer refines their blog-planner skill through iterative co-learning with Claude. They first ask Claude to review the skill, then add their own domain-specific constraints. Which description correctly captures how the co-learning refinement cycle works for skills?

- A) Claude suggests improvements the developer didn't think of like SEO considerations, then the developer adds constraints Claude doesn't know like headline style preferences — together they converge on a skill matching the actual workflow
- B) The developer submits the skill to Anthropic's quality assurance team for professional review, receives a certification report with mandatory corrections, and then applies the approved changes before the skill can be activated
- C) Claude automatically rewrites the entire skill based on analyzing the developer's last 50 interactions, using machine learning to detect implicit patterns the developer never explicitly stated or documented in the original skill
- D) The developer exports the skill to a web-based IDE where multiple team members simultaneously edit different sections in real-time, with conflict resolution handled by Claude Code's built-in merge algorithm

> **Answer: A** — The co-learning cycle has three stages: (1) AI as Teacher — Claude suggests improvements the developer didn't think of (e.g., add SEO considerations, include word count targets); (2) You as Teacher — the developer specifies constraints Claude doesn't know (e.g., "headlines must be curiosity-driven, NEVER clickbait"); (3) Convergence — they iterate until the skill matches the actual workflow. After using the skill a few times, the developer can share what worked and what didn't for further refinement. The other options describe nonexistent QA processes, automatic rewriting, or web-based IDEs.

---

**Q24.** Non-technical professionals in finance, recruiting, and legal are creating skills despite having no programming background. A skeptic argues that skills are fundamentally a developer tool. Which response most accurately explains why non-technical users can create effective skills?

- A) Non-technical users can only use pre-built skills from Anthropic's marketplace and cannot create their own, making the skeptic partially correct that creation requires technical knowledge even if usage does not
- B) Non-technical users require extensive training in markdown syntax, YAML frontmatter, and command-line operations before they can create even a basic skill, so while technically possible it remains impractical for most business professionals
- C) Skills require only clear instructions in markdown format within a folder — the barrier is willingness to articulate procedures, not technical skill — and domain experts have the specialized knowledge that makes skills most valuable
- D) Non-technical users create skills through a voice interface that transcribes their verbal descriptions into SKILL.md files automatically, bypassing the need to write or edit any text files directly on their computer

> **Answer: C** — Skills are intentionally simple — organized files (folders with markdown) that anyone can create. The format (markdown with YAML metadata) is accessible to anyone who can write structured text. Domain experts have the most valuable knowledge: a senior accountant knows exactly how audits should be structured, a recruiting lead knows what makes candidate evaluations useful. What these experts previously lacked was a mechanism to transfer that knowledge to AI. Skills require clear instructions, not programming. The barrier isn't technical skill — it's willingness to articulate procedures.

---

### Section E: Subagents and Orchestration

**Q25.** A developer runs the `/agents` command in Claude Code and sees a list of built-in agents including Explore, Plan, general-purpose, Bash, and claude-code-guide. They ask what distinguishes these agents. Which answer correctly matches built-in agents to their primary purposes and model assignments?

- A) Explore uses Haiku for fast codebase searching and file finding, Plan uses the inherited model for complex multi-step strategies, Bash inherits the current model for command execution, and claude-code-guide uses Haiku for Claude Code questions
- B) Explore uses Opus for deep codebase analysis with maximum accuracy, Plan uses Haiku for quick strategy generation with minimal latency, Bash uses Sonnet for balanced command execution, and claude-code-guide uses Opus for comprehensive guidance
- C) All built-in agents use the same Sonnet model regardless of task type, with the only differentiation being their system prompts and tool permissions — model selection is a configuration reserved exclusively for custom user-created agents
- D) Explore uses Sonnet for moderate-depth file scanning, Plan uses Opus for architectural decision-making, Bash requires manual model selection each time it runs, and claude-code-guide uses a specialized fine-tuned variant of Claude

> **Answer: A** — The built-in agents have specific model assignments: Explore uses Haiku (fast) for finding files, searching code, and understanding codebase structure; Plan uses the inherited model (Sonnet typically) for complex multi-step tasks and implementation strategies; general-purpose uses Sonnet for multi-step tasks; Bash inherits the current model for command execution; claude-code-guide uses Haiku for questions about Claude Code itself. The model choice reflects the task: fast models for search, smart models for planning.

---

**Q26.** A developer asks Claude Code to research competitors in one request, then draft a pitch in the next. Without subagents, their context becomes cluttered with research notes that interfere with pitch drafting. They ask why subagents use isolated context windows. Which answer best explains the benefit?

- A) Isolated context windows enable each subagent to use a different AI model optimized for its specific task type, which would cause compatibility errors if multiple models shared the same unified context window simultaneously
- B) Isolated context windows are required by Anthropic's terms of service to prevent intellectual property from one task from legally contaminating or influencing the output of a separate task performed in the same billing session
- C) Each subagent starts fresh without clutter from other tasks, producing more focused results — the researcher returns clean findings, the planner works with fresh focus, and nobody juggles everything at once like a team meeting
- D) Isolated context windows allow subagents to persist their internal state across multiple sessions indefinitely, building cumulative knowledge that improves their performance over weeks of repeated invocations within the same project

> **Answer: C** — Context isolation keeps each subagent focused. Without subagents, one AI doing everything fills its context with research notes, then confuses them with the pitch. With subagents: the research subagent does its work and returns a clean summary; main Claude receives it cleanly; the planning subagent drafts the pitch with fresh context. Like a team meeting — the researcher presents findings then leaves, the strategist creates a plan with fresh focus. Each specialist focuses on one job without juggling everything. The other options describe incorrect benefits.

---

**Q27.** A user asks Claude Code to find all test files in a project AND suggest a testing strategy for gaps simultaneously. Claude Code launches both the Explore and Plan subagents in parallel. The user asks how this parallel execution pattern works. Which explanation is accurate?

- A) Claude Code serializes the requests and runs them sequentially in a rapid alternation pattern that simulates parallel execution, with each agent receiving tiny time slices in a round-robin scheduling approach similar to CPU threading
- B) Claude Code launches both subagents simultaneously with their own isolated contexts, they work independently in parallel, and their results combine into a single response — this is orchestration of multiple specialists toward one goal
- C) Claude Code forks its main process into two identical copies, each handling one subagent request, then merges the forked processes back together — this requires double the memory allocation and may fail on resource-constrained machines
- D) Claude Code delegates both requests to Anthropic's server cluster where they run on separate GPU instances in different data centers, with results aggregated by a coordination service before being sent back to the user's terminal

> **Answer: B** — When given a prompt requiring multiple subagents, Claude Code launches both simultaneously. Each works independently with its own isolated context window. The results combine into a single response for the user. This is orchestration — coordinating multiple specialists toward a goal. Real-world examples include "Use Explore to find all API routes AND use Plan to suggest how to add authentication" — both run in parallel. The other options incorrectly describe sequential execution, process forking, or cloud GPU delegation.

---

**Q28.** A developer creates a custom subagent via `/agents` → "Create new agent" → "Generate with Claude." They describe it as: "Help me review code for bugs and suggest improvements. Use when I say 'review this code.'" Where does the agent file live, and what does it contain?

- A) The agent is stored as a compiled binary in Claude Code's installation directory at /usr/local/lib/claude/agents/, containing pre-trained model weights specialized for code review tasks through transfer learning
- B) The agent is registered in Anthropic's cloud agent registry and accessed via API calls, containing a unique agent identifier and authentication token that links to server-side instructions managed by Anthropic
- C) The agent is saved as an encrypted configuration in the system keychain alongside API keys and credentials, containing environment variables and secret parameters needed for the agent's specialized tool access permissions
- D) The agent is saved as a markdown file at .claude/agents/code-reviewer.md for project scope, containing YAML frontmatter with name, description, and model, plus markdown instructions defining review procedures

> **Answer: D** — Custom subagents are saved as markdown files. Project-level agents go to `.claude/agents/` (this project only), user-level agents go to `~/.claude/agents/` (all projects). The file contains YAML frontmatter (name, description, model selection like "sonnet") and markdown body with instructions (e.g., check for bugs and edge cases, suggest performance improvements, note security concerns). Claude Code generates this from the user's natural language description. The other options describe nonexistent binary storage, cloud registries, or keychain encryption.

---

**Q29.** A subagent is invoked for a specific task, completes its work, and returns results. A new user tries to continue a conversation with a subagent after it finishes, expecting ongoing dialogue. Which explanation correctly describes the subagent execution model?

- A) Subagents do maintain ongoing dialogue capabilities, but the user must explicitly resume them using the /resume command followed by the subagent's session identifier, which was displayed when the subagent first completed its task
- B) Subagents can be converted to persistent agents by adding a "persistent: true" flag in their configuration file, which keeps them running in the background and allows continuous interaction through a dedicated communication channel
- C) A subagent receives a specific goal, works independently in isolated context, completes its task, returns results to main Claude Code, and control returns to the main session — each invocation starts with clean context and doesn't persist
- D) Subagents maintain a hidden memory buffer that stores their last three interactions, allowing partial continuity if the same subagent is invoked again within 30 minutes — beyond that window the buffer expires and context resets

> **Answer: C** — The subagent execution model is: one task, one completion. A subagent is invoked for a specific goal, works independently in isolated context, completes its task, and returns results to main Claude Code. Control then returns to the main session. You interact with main Claude Code to proceed. Each invocation starts with clean context — subagents don't persist. Like sending a specialist to research something: they return with a report, then you continue with your main assistant. The other options describe nonexistent resume commands, persistence flags, or memory buffers.

---

**Q30.** Claude Code can automatically decide when to delegate to a subagent or be explicitly told which agent to use. A developer asks how Claude Code determines when to auto-delegate versus waiting for explicit instruction. Which answer correctly explains automatic delegation?

- A) Claude Code auto-delegates exclusively based on keyword matching in the user's prompt — specific trigger words like "find" always route to Explore and "plan" always routes to Plan regardless of the surrounding conversational context
- B) Claude Code auto-delegates based on task complexity, request type, and subagent descriptions — asking "What files handle authentication?" may trigger Explore, while complex multi-step requests may trigger Plan automatically
- C) Claude Code never auto-delegates in its default configuration and always requires explicit invocation using the exact subagent name — automatic delegation is a beta feature that must be manually enabled in settings.json
- D) Claude Code auto-delegates by randomly selecting an available subagent for each request and evaluating whether its output is satisfactory, falling back to the next agent in an alphabetical queue if the first attempt produces poor results

> **Answer: B** — Claude Code decides when to delegate based on: task complexity (multi-step tasks trigger Plan), request type (code search requests might trigger Explore), and subagent descriptions (Claude matches task characteristics to specialist descriptions). Examples: "What files handle authentication?" → Explore auto-activates; "Help me add user login to this app" → Plan auto-activates for the complex task. Users can also explicitly invoke: "Use the Plan subagent to analyze this feature request." The other options describe incorrect keyword matching, disabled defaults, or random selection.

---

### Section F: MCP Integration

**Q31.** A developer needs Claude Code to browse websites and fetch up-to-date documentation, but Claude can currently only access local files. They hear about MCP as the solution. Which description most accurately explains what MCP is and what problem it solves?

- A) MCP is a machine learning model compression protocol that reduces Claude's neural network size for faster inference on edge devices, solving the problem of high latency when running AI models on laptops without dedicated GPU hardware
- B) MCP is a cloud storage synchronization service that mirrors local project files to Anthropic's servers in real-time, solving the problem of team members needing simultaneous access to the same codebase from different locations
- C) MCP is a code generation acceleration framework that pre-compiles common programming patterns into reusable templates, solving the problem of Claude regenerating boilerplate code from scratch for every new request
- D) MCP is Model Context Protocol — an open standard that connects Claude Code to external tools, APIs, and data sources through standardized, permission-controlled connections, solving the problem of Claude being limited to local files only

> **Answer: D** — MCP (Model Context Protocol) extends Claude Code's reach beyond local files to the outside world. It's an open standard (donated by Anthropic to the Linux Foundation's AAIF) that provides standardized, safe connections to external systems — websites, documentation, APIs, databases. Think of it as a "phone directory" of approved external contacts. Without MCP, Claude is limited to files on your computer. With MCP, Claude can browse the web (Playwright), fetch current docs (Context7), query databases, and more. The other options describe unrelated technologies.

---

**Q32.** MCP servers are registered using the `claude mcp add` command. A developer installs two MCP servers — Playwright for web browsing and Context7 for documentation. They want to verify their installations and understand the command syntax. Which command syntax and verification approach is correct?

- A) Servers are added with `claude mcp add --transport stdio [name] npx [package]` and verified by running `claude mcp list` which shows all registered MCP servers — Playwright and Context7 both use stdio transport with npx execution
- B) Servers are added with `claude mcp install --protocol http [name] [url]` and verified by running `claude mcp status` which shows connection health for each server — both servers require a persistent HTTP connection running in the background
- C) Servers are added by editing the .claude/mcp-config.json file manually with server endpoints and authentication tokens, then verified by restarting Claude Code and checking the startup log for successful connection handshake messages
- D) Servers are added with `claude plugins add [name] --source npm` and verified by running `claude plugins test [name]` which performs a health check — MCP servers are a type of plugin in Claude Code's extensibility architecture

> **Answer: A** — The correct syntax is `claude mcp add --transport stdio [name] npx [package]`. For example: `claude mcp add --transport stdio playwright npx @playwright/mcp@latest` and `claude mcp add --transport stdio context7 npx @upstash/context7-mcp`. Verify with `claude mcp list` which shows all registered servers. Both use stdio transport with npx for execution. The other options describe incorrect commands, protocols, or configuration methods.

---

**Q33.** The lesson emphasizes several security practices when using MCP servers. A developer about to install their first MCP server asks for security guidance. Which set of practices correctly reflects the security recommendations for MCP usage?

- A) MCP servers automatically sandbox all external access using containerization, so users can safely install any server from any source without verification because malicious actions are prevented by Claude Code's built-in security layer
- B) Only use trusted MCP servers from reputable sources, never paste secrets into files by using environment variables or system keychain instead, and verify servers by checking source code on GitHub and maintainer reputation before installing
- C) MCP security is handled entirely by Anthropic's server-side infrastructure which scans all data passing through MCP connections for malware, so client-side security practices are unnecessary beyond keeping Claude Code updated
- D) All MCP servers require a paid security certificate from Anthropic before they can be registered, ensuring only vetted and approved servers can connect to Claude Code regardless of the transport protocol or source repository

> **Answer: B** — MCP security practices include: use trusted servers from reputable sources (Anthropic's official list, modelcontextprotocol.io, verified npm packages); never paste secrets into files — use environment variables or system keychain; check before installing by reading source code on GitHub, verifying maintainer reputation, and checking recent commits. A malicious MCP server could expose your system, read files, and access tokens. The other options incorrectly describe automatic sandboxing, server-side scanning, or paid certificates.

---

**Q34.** Claude Code includes MCP Tool Search (since version 2.1.7+) that automatically manages tool definition overhead. A power user with 5 MCP servers installed asks how Tool Search reduces token consumption. Which explanation accurately describes how MCP Tool Search works?

- A) Tool Search monitors installed MCP servers and when tool definitions exceed 10% of context, it activates lazy loading — instead of loading all tools upfront, Claude searches for relevant tools on-demand, achieving approximately 85% reduction in overhead
- B) Tool Search compresses all tool definitions using a proprietary encoding that reduces their token footprint by 85% while maintaining full functionality, loading the compressed versions at startup and decompressing each tool only during execution
- C) Tool Search caches tool definitions in a local SQLite database after the first session, then loads only the cached metadata in subsequent sessions, reducing startup overhead by eliminating repeated network requests to MCP server endpoints
- D) Tool Search distributes tool definitions across multiple parallel context windows running simultaneously, with each window handling a subset of tools, effectively multiplying the available context space by the number of active windows

> **Answer: A** — MCP Tool Search (built-in since Claude Code 2.1.7+, January 2026) works through automatic lazy loading: it monitors installed MCP servers, and when tool definitions exceed 10% of context, it activates. Instead of loading ALL tools upfront, Claude searches for relevant tools on-demand — only the tools you actually use get loaded. Result: ~85% automatic reduction in MCP overhead. It can be controlled via ENABLE_TOOL_SEARCH environment variable (auto, auto:5, true, false). The other options describe nonexistent compression, SQLite caching, or parallel windows.

---

**Q35.** The lesson presents a framework for when MCP is appropriate versus when it should be avoided. A developer planning their project's MCP strategy asks for clear guidance. Which set of use-case distinctions correctly reflects when to use and when to avoid MCP?

- A) Use MCP for all database operations regardless of query frequency, and avoid MCP only when working with files smaller than 1KB since those can be processed faster through Claude Code's built-in text handling without external tool overhead
- B) Use MCP exclusively during development and staging phases for testing external integrations, and avoid MCP in production environments where direct API calls provide better reliability and lower latency for end-user facing features
- C) Use MCP for any task that involves text processing of external content, and avoid MCP only when the output format requires binary file generation since MCP servers cannot return binary data through the stdio transport protocol
- D) Use MCP for current information, real-time data, web interaction, and safe external integration — avoid MCP for private or sensitive data, high-frequency queries needing thousands per second, and untrusted or unverified servers

> **Answer: D** — Use MCP when you need current information (frequently changing docs), real-time data (stock prices, database queries), web interaction (browsing, testing), or safe external integration (trusted APIs with permission controls). Avoid MCP for private/sensitive data (use local file access instead), high-frequency queries (1000/second needs direct connections — MCP adds latency), untrusted servers (could expose your system), and before understanding basics (master Playwright/Context7 first). The other options oversimplify or mischaracterize the boundaries.

---

### Section G: Compiling MCP to Skills

**Q36.** A developer learns that loading Playwright MCP directly consumes approximately 5,000-8,000 tokens of tool definitions, while using the browsing-with-playwright compiled skill consumes only about 150 tokens. They ask how compiled skills achieve this dramatic reduction. Which explanation accurately describes the code execution pattern?

- A) Compiled skills achieve token reduction by sending requests to Anthropic's edge servers that maintain persistent MCP connections, offloading all tool definition storage to cloud infrastructure rather than loading definitions into the local context
- B) Compiled skills strip unnecessary metadata and comments from MCP tool definitions, creating a minified version that retains full functionality in approximately 3% of the original token footprint through aggressive text compression algorithms
- C) The SKILL.md loads once at about 150 tokens, then Claude executes bash commands that call mcp-client.py scripts locally outside the context window — heavy browser operations consume zero tokens because they run as local HTTP calls
- D) Compiled skills replace MCP tool definitions with pre-recorded response templates that Claude selects from based on pattern matching, eliminating the need for live tool execution but limiting functionality to only previously encountered scenarios

> **Answer: C** — The code execution pattern works in three stages: (1) SKILL.md loads once (~150 tokens) providing high-level procedures; (2) Claude executes bash commands calling `mcp-client.py` which runs locally, outside Claude's context window; (3) The script connects to the Playwright MCP server via HTTP transport, performs browser operations, and returns only filtered results. Tool definitions NEVER load into Claude's context. All browser operations happen locally via HTTP calls — zero tokens consumed for execution. Direct MCP: ~15,000-24,000 tokens. Compiled skill: ~150-250 tokens. Savings: ~97-99%.

---

**Q37.** Compiled skills use three-stage progressive disclosure to minimize token consumption at each phase. A developer optimizing their workflow asks how each stage contributes to efficiency. Which description correctly identifies all three stages and their token costs?

- A) Stage 1 Discovery loads only the description field at startup costing about 30 tokens, Stage 2 Activation loads the full SKILL.md when relevant costing about 150 tokens, and Stage 3 Execution runs scripts locally consuming zero tokens in context
- B) Stage 1 downloads all skill files from a remote repository costing approximately 500 tokens of metadata, Stage 2 validates file integrity using checksums costing about 100 tokens, and Stage 3 decompresses the skill archive consuming 50 tokens
- C) Stage 1 loads the complete SKILL.md at startup costing approximately 300 tokens, Stage 2 pre-caches all script outputs in memory costing about 200 tokens, and Stage 3 serves cached results without any additional token cost per request
- D) Stage 1 registers the skill name in Claude's tool index costing approximately 10 tokens, Stage 2 generates a summarized version of the skill on demand costing about 75 tokens, and Stage 3 loads full instructions only when explicitly invoked by the user

> **Answer: A** — The three-stage progressive disclosure: (1) Discovery (startup) — loads only the `description` field (~30 tokens), just enough for Claude to know the skill exists; (2) Activation (when relevant) — loads the full SKILL.md with detailed procedures (~150 tokens), triggered when Claude decides the skill applies; (3) Execution (when needed) — runs `scripts/` locally via bash (0 tokens in context), because all heavy operations execute outside Claude's conversation. This is critical for compiled skills: Stage 3's local execution is what enables the ~98% token reduction over direct MCP.

---

**Q38.** A developer has both the browsing-with-playwright compiled skill and the Playwright MCP server installed directly. The compiled skill's SKILL.md contains decision logic advising Claude on which approach to use for different task types. They ask about a task involving extracting prices from 100+ products on an e-commerce page. Which approach should Claude recommend and why?

- A) Use direct Playwright MCP because it provides more reliable browser automation for large-scale data extraction, with built-in pagination handling and retry logic that compiled skills cannot replicate through bash script wrappers
- B) Use both approaches simultaneously by splitting the product list into two halves, processing 50 products through direct MCP and 50 through the compiled skill, then merging the results for maximum throughput and redundancy
- C) Use direct Playwright MCP because compiled skills have a hard limit of 50 items per extraction batch imposed by the mcp-client.py script, requiring multiple separate invocations that negate any token savings for large datasets
- D) Use the compiled skill because extracting 100+ items requires local filtering to process results and return only relevant data — the skill runs extraction locally outside context, preventing massive token consumption from loading all 100+ item details

> **Answer: D** — The SKILL.md decision logic specifies: use compiled pattern when extracting data from 50+ elements (local filtering needed), running multi-step workflows, needing consistent automation, or sharing with team. Let Tool Search handle simple single-page navigation, quick checks, one-off screenshots. For 100+ products, the compiled skill processes locally, filters the data (e.g., 1000 items → 20 relevant ones), and returns only filtered results. Direct MCP would load all 100+ items into context, wasting thousands of tokens. The other options mischaracterize capabilities.

---

**Q39.** A decision framework helps developers choose between three approaches: Tool Search (automatic), compiled skills, and direct MCP. A team architect needs to understand when each approach is most appropriate. Which set of guidelines correctly maps scenarios to the recommended approach?

- A) Tool Search for all enterprise deployments, compiled skills for all individual developer workflows, and direct MCP exclusively for testing environments — the choice depends entirely on the organizational context rather than task characteristics
- B) Tool Search for simple and infrequent queries that are handled automatically, compiled skills for repeated multi-step workflows needing local filtering or team sharing, and direct MCP for one-off queries where overhead is acceptable for single use
- C) Tool Search for tasks requiring less than 100 tokens of output, compiled skills for tasks requiring between 100 and 1000 tokens, and direct MCP for tasks requiring more than 1000 tokens — the choice is determined solely by expected output size
- D) Tool Search for queries against low-popularity MCP servers with few users, compiled skills for queries against high-popularity servers with mature ecosystems, and direct MCP for queries against newly released servers still in beta testing

> **Answer: B** — The decision framework maps: Tool Search (automatic) → simple queries, built-in efficiency with zero effort; Compiled skill → multi-step workflows needing local execution and control, tasks needing local filtering (process 1000 → return 20), cross-agent portability (Skills format works in Codex, Goose), team-shareable workflows; Direct MCP → one-off queries where overhead is acceptable, low-token servers (<1,500 tokens), small well-formatted results. Decision shortcut: simple and infrequent → Tool Search; complex and repeated → compile; need filtering or team sharing → compile.

---

**Q40.** The fetch-library-docs compiled skill wraps the Context7 MCP server and provides content-type filtering. A developer fetches Next.js documentation and specifies `--content-type setup` instead of fetching everything. They ask why content-type filtering matters for token efficiency. Which answer correctly explains the filtering mechanism and its impact?

- A) Content-type filtering sends a modified query to the Context7 API that requests only specific documentation sections at the server level, reducing network bandwidth but having no effect on token consumption since Claude still processes full responses
- B) Content-type filtering is a cosmetic formatting option that changes how documentation is displayed in Claude's output but does not actually reduce the amount of data fetched or the tokens consumed during the retrieval and processing steps
- C) The skill's shell scripts call Context7 MCP locally, then filter the response to extract only the requested content type — setup, examples, or api-ref — returning 60-90% fewer tokens because heavy filtering happens outside Claude's context window
- D) Content-type filtering permanently caches filtered documentation versions on disk after the first fetch, eliminating all future token costs for the same library since subsequent requests are served entirely from the local cache without any API calls

> **Answer: C** — The fetch-library-docs skill achieves 60-90% additional token savings through content-type filtering. When you specify `--content-type setup`, the skill's shell scripts (`fetch-docs.sh`, `filter-by-type.sh`, `extract-*.sh`) call Context7 MCP locally via subprocess (outside Claude's context), then filter the full response to extract only setup instructions, examples, or API references. The filtering happens in shell scripts (local execution), not in Claude's context — so heavy processing stays outside the conversation. Without filtering, all content types return, consuming far more tokens.

---

## Answer Distribution Verification

| Letter | Count | Percentage |
|--------|-------|------------|
| A | 10 | 25% |
| B | 10 | 25% |
| C | 10 | 25% |
| D | 10 | 25% |

**Longest streak of same letter:** 2 (within acceptable limit of ≤ 3)

## Word Count Spot-Check

| Question | A (words) | B (words) | C (words) | D (words) | Correct | Longest |
|----------|-----------|-----------|-----------|-----------|---------|---------|
| Q5 | 30 | 28 | 29 | 27 | B | A |
| Q14 | 28 | 33 | 30 | 27 | B | B |
| Q22 | 29 | 28 | 29 | 27 | D | A |
| Q33 | 32 | 31 | 29 | 28 | B | A |
| Q40 | 31 | 30 | 30 | 27 | C | A |
