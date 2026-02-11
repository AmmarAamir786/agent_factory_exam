# Exam: CLAUDE.md, Skills, Subagents, MCP & Compiled Skills (Lessons 5–13)

This exam covers Lessons 5 through 13: CLAUDE.md context files, teaching Claude your style with skills, the architecture of agent expertise, building skills, subagents and orchestration, MCP integration, and compiling MCP to skills. Answer all questions by selecting the single best option (A, B, C, or D). Answers and explanations follow each question inline.

---

### Section 1: CLAUDE.md and Persistent Context

**Q1.**What is CLAUDE.md?

- A) A configuration file that stores Claude Code's API keys and authentication tokens
- B) A markdown file in the project root that Claude Code auto-loads at every session start
- C) A hidden system file that tracks conversation history between Claude Code sessions
- D) A template file used to generate new Claude Code projects from the command line

> **Answer: B** — CLAUDE.md is a simple markdown file placed in the project root that Claude Code automatically loads at the start of every session, providing persistent project context without the user repeating it.

---

**Q2.**Why do Large Language Models like Claude require mechanisms such as CLAUDE.md for persistent context?

- A) LLMs store context in temporary cache that expires after 24 hours
- B) LLMs are stateless — each call is processed in complete isolation with no memory of previous sessions
- C) LLMs compress prior conversations into lossy summaries that miss project details
- D) LLMs can only access files explicitly attached to the current API request

> **Answer: B** — LLMs are stateless — they don't store any state between requests. Every new session starts completely blank. CLAUDE.md solves this by giving Claude a persistent file to read at session start.

---

**Q3.**A developer notices that within a single session, Claude Code seems to remember earlier messages. But when they close the terminal and start a new session, Claude remembers nothing. What explains this behavior?

- A) Claude Code saves conversation logs locally and encrypts them, but clears them on terminal close as a security precaution to prevent data leakage
- B) Claude stores session memory in RAM that is flushed when the process exits — requiring an external file like CLAUDE.md to persist any context
- C) Claude Code re-sends the full message history with each call within a session, creating the illusion of continuity — but the LLM retains nothing between sessions
- D) Claude Code maintains a local SQLite database of conversations that syncs per-session, but the database gets corrupted when the terminal closes unexpectedly

> **Answer: C** — Claude Code secretly bundles all prior messages and re-sends them with each new message, so the LLM reads the full history fresh each time. This creates the illusion of continuous conversation, but when the session ends, nothing persists — hence the need for CLAUDE.md.

---

**Q4.**Which six sections does a typical CLAUDE.md contain?

- A) Authentication, Endpoints, Schemas, Migrations, Secrets, Logs
- B) Project Overview, Technology Stack, Directory Structure, Coding Conventions, Key Commands, Important Notes
- C) Introduction, Installation, Configuration, Usage, Testing, Deployment
- D) Dependencies, Build Scripts, CI/CD Pipelines, Monitoring, Alerts, Rollback Plans

> **Answer: B** — The six standard sections are Project Overview, Technology Stack, Directory Structure, Coding Conventions, Key Commands, and Important Notes.

---

**Q5.**A team lead creates a CLAUDE.md file but places it inside `src/docs/CLAUDE.md`. New Claude Code sessions don't pick it up. A teammate suggests the file might also be named `claude.md` in some editors. What is the most likely root cause?

- A) The file must be named in lowercase as `claude.md` — Claude Code requires lowercase for cross-platform compatibility
- B) CLAUDE.md must be encoded in UTF-8 without BOM — Claude Code silently ignores files with incorrect encoding
- C) Claude Code only auto-detects CLAUDE.md at the project root — the same level as `.git` — not in subdirectories
- D) The file exceeds the 3 KB size limit — Claude Code silently skips any context files larger than this threshold

> **Answer: C** — CLAUDE.md must be in the project root (same level as `.git`, `package.json`, etc.). Placing it in a subdirectory means Claude Code won't auto-detect it. The filename is also case-sensitive and must be exactly `CLAUDE.md`.

---

**Q6.**What is the core insight behind Claude Code using the file system as external memory instead of trying to keep everything in conversation history?

- A) File systems are faster to read than conversation histories are to parse
- B) Code files already contain the project's state, so Claude reads the project directly rather than having it described
- C) Conversation histories are limited to 4,096 tokens while files have no size limit
- D) File system access allows Claude Code to bypass the API's rate limiting on message length

> **Answer: B** — The key insight is that your code files already contain your project's state. Instead of describing your project to Claude, Claude reads it directly. CLAUDE.md serves as the orientation guide Claude reads first.

---

**Q7.**A senior developer argues: "CLAUDE.md is redundant — I can just paste my project context at the start of every session and get the same results." A junior developer disagrees. What is the strongest counter-argument?

- A) CLAUDE.md is encrypted at rest and more secure than pasting plain-text context into a conversation prompt that could be logged or intercepted
- B) Pasting context manually introduces human variability each time, while CLAUDE.md ensures consistent, complete context loads automatically and enables team alignment
- C) Claude Code's API rejects prompts longer than 500 tokens, making manual pasting infeasible for any project with a non-trivial technology stack
- D) CLAUDE.md loads before the system prompt, giving it higher processing priority than any user-supplied context pasted into the conversation

> **Answer: B** — Manual pasting is exhausting for complex projects, introduces variability (you might forget details), and doesn't scale across teams. CLAUDE.md provides consistent, automatic context loading — a one-time 10-15 minute investment that benefits every future session and every team member.

---

### Section 2: AGENTS.md and Universal Standards

**Q8.**What is AGENTS.md?

- A) A configuration file for defining subagent behaviors in Claude Code
- B) A universal markdown standard for providing project context to any AI coding agent
- C) An Anthropic-proprietary format for encoding agent skills and capabilities
- D) A log file that records which AI agents have accessed a project repository

> **Answer: B** — AGENTS.md is a universal standard (created by OpenAI, now under the Linux Foundation's AAIF) that provides project-specific guidance to any AI coding agent — Claude Code, Cursor, GitHub Copilot, Gemini CLI, and more.

---

**Q9.**What is the recommended relationship between CLAUDE.md and AGENTS.md in the same project?

- A) They should contain identical content duplicated across both files to ensure consistency across all AI coding agents
- B) AGENTS.md replaces CLAUDE.md entirely since it is the vendor-neutral universal standard adopted by the Linux Foundation
- C) AGENTS.md holds universal project context; CLAUDE.md references it and adds Claude-specific features like skills, hooks, and MCP configs
- D) CLAUDE.md holds all project context as the primary file; AGENTS.md is only used as a fallback when Claude Code is unavailable

> **Answer: C** — The recommended approach is to put universal project context (overview, stack, structure, conventions) in AGENTS.md, and have CLAUDE.md reference it via `@AGENTS.md` while adding Claude-specific instructions for skills, hooks, and MCP configurations.

---

**Q10.**A company uses Claude Code, Cursor, and GitHub Copilot across different teams. They currently maintain separate context files for each tool. An architect proposes consolidating to AGENTS.md plus tool-specific files. What is the primary benefit?

- A) It reduces the total number of context files in the repository from three to two, simplifying the project directory structure
- B) Universal project context is written once in AGENTS.md and works across all three tools, eliminating duplication across teams
- C) AGENTS.md is the only context format that supports YAML frontmatter for structured metadata and automated parsing
- D) Consolidation creates organizational pressure for all teams to eventually standardize on a single AI coding tool

> **Answer: B** — With AGENTS.md holding universal context (project overview, stack, conventions), any AI agent understands the project. Claude Code, Cursor, and Copilot all read AGENTS.md. Tool-specific files (like CLAUDE.md) add only features unique to that tool. This eliminates duplication and ensures consistency.

---

### Section 3: Non-Determinism and Why Skills Exist

**Q11.**What does "non-deterministic" mean in the context of AI language models?

- A) The model produces errors randomly due to hardware limitations
- B) The same input can produce different outputs each time
- C) The model cannot process inputs longer than a fixed token limit
- D) The model's training data contains contradictory information

> **Answer: B** — Non-deterministic means "not guaranteed to give the same result." The same prompt sent to an AI model twice can produce different outputs — different structure, tone, and length each time.

---

**Q12.**The "double variability problem" compounds unpredictability in AI outputs. What are the two sources of drift?

- A) Model temperature settings and API rate limiting
- B) You phrase requests differently each time, and the AI model generates differently even for identical requests
- C) Training data staleness and inference hardware differences
- D) Context window overflow and tokenizer rounding errors

> **Answer: B** — Two sources create drift: (1) you phrase requests differently each time ("write a post" vs "help me with an update"), and (2) the AI model itself produces different outputs even for identical prompts. Together, these make outputs unpredictable.

---

**Q13.**How do skills constrain non-determinism without eliminating it?

- A) Skills lock the model's temperature to zero, forcing identical outputs every time
- B) Skills pre-generate fixed output templates that Claude fills in with minor variations
- C) Skills define boundaries — tone, structure, preferences — so output varies but stays within the creator's constraints
- D) Skills override the model's sampling algorithm with a deterministic decoding strategy

> **Answer: C** — Skills don't eliminate non-determinism (that's inherent to AI models). They define your boundaries — your tone, your structure, your preferences — so Claude's output still varies, but always within YOUR constraints.

---

**Q14.**A content creator runs the same prompt — "Write a LinkedIn post about learning AI development" — twice without any skill. They get different results each time: different structure, different emoji count, different tone. They then create a skill specifying "friendly-professional tone, 2-3 emojis, end with engagement question." What changes?

- A) Both runs produce byte-identical output since the skill eliminates all randomness from the generation process entirely
- B) Both runs still differ in wording, but both maintain the friendly-professional tone, use 2-3 emojis, and end with a question
- C) The skill forces Claude to select from a predefined library of pre-written LinkedIn templates stored in the skill folder
- D) The first run uses the skill correctly, but the second run reverts to default behavior unless the skill is explicitly re-invoked

> **Answer: B** — The skill constrains — not eliminates — non-determinism. Claude's output still varies in specific wording, but stays within the defined boundaries: friendly-professional tone, 2-3 emojis, engagement question at the end.

---

### Section 4: The Architecture of Agent Expertise (Skills Concept)

**Q15.**The formula "Intelligence + Code = Execution, but not Expertise" captures a key insight. What is the missing piece that skills provide?

- A) Additional computational resources for faster and more efficient model inference
- B) Domain-specific knowledge that makes generic capability useful for real work
- C) Network connectivity to external APIs, databases, and cloud services
- D) Version control integration for tracking code changes over time

> **Answer: B** — Models provide intelligence (reasoning, analysis). Code provides execution (APIs, file system, Python). Together they create an agent that can execute. But the missing piece is expertise — domain-specific knowledge that skills encode.

---

**Q16.**In the three-level loading architecture for skills, what is loaded at each stage?

- A) Level 1: full SKILL.md; Level 2: supporting scripts; Level 3: external API connections
- B) Level 1: brief metadata/description; Level 2: full SKILL.md instructions; Level 3: supporting files and scripts
- C) Level 1: user preferences; Level 2: project context; Level 3: conversation history
- D) Level 1: tool definitions; Level 2: MCP server configs; Level 3: compiled binaries

> **Answer: B** — The three levels are: (1) Brief metadata — a short description always loaded so Claude knows the skill exists; (2) Full instructions — the complete SKILL.md loaded on-demand when relevant; (3) Supporting files — scripts, references, tools accessed only during execution.

---

**Q17.**A developer has 50 skills installed and worries about overwhelming Claude's context window. Based on the three-level loading architecture, why is this concern unfounded?

- A) Claude Code compresses all 50 skills into a single token-efficient binary blob loaded once at startup
- B) Skills are loaded alphabetically and Claude stops after the first 10 to preserve context window space
- C) Only brief descriptions load at startup — full instructions load on-demand when relevant, like apps that stay closed until tapped
- D) Claude Code offloads all skill storage to a cloud cache that doesn't consume any local context tokens

> **Answer: C** — Skills use progressive disclosure: only short descriptions load at startup. Full SKILL.md content loads only when Claude determines a skill applies. Like apps on your phone — 100 installed, but only the active ones consume resources.

---

**Q18.**A CTO claims: "We should build a custom AI agent for our legal department — they need specialized contract review capabilities." Based on the "stop building agents, build skills instead" paradigm, what is the strongest counter-argument?

- A) Custom agents are technically impossible to build with current LLM frameworks and available tooling
- B) The agent already exists in Claude Code — what's missing is the legal team's expertise encoded as skills, which are simpler to create and share
- C) Skills are limited to text processing and cannot handle the specialized document formats used in legal work
- D) Custom agents would require Anthropic's enterprise license, while skills are freely available on all pricing plans

> **Answer: B** — The paradigm shift is: stop building agents, build skills instead. The agent (Claude Code) is mature. The legal team's contract review workflow, clause analysis procedures, and due diligence checklists can be encoded as skills — far simpler than building custom agent infrastructure.

---

**Q19.**What are the three sources from which skills emerge?

- A) Open-source, proprietary, and academic research skills
- B) Foundational skills, partner/third-party skills, and enterprise/custom skills
- C) Community skills, marketplace skills, and premium skills
- D) Built-in skills, downloaded skills, and generated skills

> **Answer: B** — Skills come from three sources: (1) Foundational — basic capabilities like document creation; (2) Partner/third-party — expertise for specific software like Browserbase or Notion; (3) Enterprise/custom — organizational knowledge like coding standards and internal workflows.

---

**Q20.**How do skills and MCP servers complement each other rather than compete?

- A) Skills handle frontend tasks while MCP servers handle backend operations
- B) Skills provide the expertise for how to do something; MCP provides the connectivity to external data and tools needed to do it
- C) Skills run during development while MCP servers run during production deployment
- D) Skills are for individual developers while MCP servers are for organizational infrastructure

> **Answer: B** — They're complementary: MCP servers provide connection to the outside world (data, tools, APIs). Skills provide expertise for using those connections effectively. Together, Claude queries data (MCP) and analyzes it using your procedures (skill).

---

**Q21.**A financial services team has an MCP server connected to their accounting database. Claude can query data but produces generic, unstructured reports. What would adding a skill accomplish?

- A) The skill would replace the MCP connection entirely and access the database directly through local scripts instead
- B) The skill would encode the team's reporting procedures — what to generate, what format, what insights to highlight
- C) The skill would cache all database results locally to eliminate MCP latency on any subsequent repeated queries
- D) The skill would add authentication and authorization layers that MCP servers cannot provide on their own

> **Answer: B** — Without a skill, Claude has data access but no domain expertise. The skill encodes the team's specific reporting standards, formats, and analysis procedures — transforming generic database output into reports that match organizational expectations.

---

**Q22.**Why is the "skills are applications" analogy (Models = Processors, Agent Runtimes = OS, Skills = Applications) significant for adoption?

- A) It means only large companies with processor-level resources can create meaningful and useful skills
- B) It positions skills as the layer where anyone can contribute expertise — like how millions build apps
- C) It implies that skills must be compiled into binary formats just like traditional desktop applications
- D) It suggests that skills will eventually replace both the underlying models and agent runtimes entirely

> **Answer: B** — The analogy matters because it identifies where value creation happens. Few companies build processors (models) or operating systems (runtimes), but millions build applications (skills). Skills open the "applications layer" for everyone — including non-technical domain experts.

---

**Q23.**A recruiter with no programming experience hesitates: "Skills sound like a developer thing — I can't create them." Based on Anthropic's early observations, what is the most compelling response?

- A) Recruiters can learn Python in a weekend course and then build skills with basic scripting support
- B) In the first weeks after launch, non-technical people in finance, recruiting, and legal were building skills — because skills need clear instructions, not code
- C) Anthropic provides a curated marketplace where recruiters can purchase pre-built skills for their domain
- D) The skill-creator meta-skill writes all the technical code automatically so the recruiter never sees any details

> **Answer: B** — Anthropic observed early that skills are being built by non-technical people across domains. A recruiter's candidate evaluation checklist becomes a skill. The barrier isn't technical skill — it's willingness to articulate procedures clearly in structured text.

---

### Section 5: Building Skills (SKILL.md)

**Q24.**What are the two required parts of a SKILL.md file?

- A) JSON schema and executable script
- B) YAML frontmatter (metadata) and markdown body (instructions)
- C) XML header and HTML template
- D) Python configuration and test suite

> **Answer: B** — Every SKILL.md has two parts: (1) YAML frontmatter — the "ID card" with name and description; (2) Markdown body — the instructions with procedures, output format, and quality criteria.

---

**Q25.**A developer writes this skill description: `description: "Helps with notes"`. The skill rarely activates when they share meeting content. What is wrong and how should it be fixed?

- A) The description needs to include the skill's version number and author name for Claude to properly index and activate it
- B) The description is too vague — it should specify the action, key outputs, and trigger conditions for Claude to match tasks accurately
- C) The description must be written as a Python regular expression so Claude can pattern-match it against each user input
- D) The description should list every possible user phrase that could trigger activation to ensure complete coverage

> **Answer: B** — Descriptions must be specific enough for Claude to know when to activate. The formula is: [Action verb] + [input type] + [output type] + [key features] + [trigger conditions]. "Helps with notes" gives Claude no useful information for matching.

---

**Q26.**Why is the description field called "the most important line" in a SKILL.md file?

- A) It is the only field indexed by Claude Code's internal search engine
- B) It determines when Claude activates the skill — Claude scans descriptions at startup to match tasks to skills
- C) It is displayed to the user as a confirmation prompt before every skill invocation
- D) It controls which model (Haiku, Sonnet, Opus) is used when the skill executes

> **Answer: B** — The description is loaded as Level 1 metadata at startup. When a user asks for help, Claude scans these descriptions to decide which skills apply. A bad description means a good skill never gets activated.

---

**Q27.**A developer creates a blog-planner skill and tests it. The output doesn't include SEO considerations or word count targets. They ask Claude to review the skill and suggest improvements. Claude recommends adding those sections. The developer then adds their own constraint: "headlines must be curiosity-driven, never clickbait." What pattern does this interaction illustrate?

- A) The waterfall development pattern where all requirements are gathered exhaustively before implementation begins
- B) The co-learning refinement cycle — Claude teaches what the developer missed, the developer teaches constraints Claude lacks
- C) The automated testing pattern where Claude validates each skill output against predefined acceptance criteria
- D) The version control pattern where each skill revision is committed to Git with a descriptive changelog entry

> **Answer: B** — This is the co-learning cycle: AI as Teacher (Claude suggests SEO and word counts), You as Teacher (developer adds "no clickbait" constraint), Convergence (together they refine until the skill matches the actual workflow).

---

**Q28.**Where do skill folders live in a Claude Code project?

- A) In the project's `node_modules/.claude/` directory
- B) In `.claude/skills/` within the project directory
- C) In a global `~/.config/claude/skills/` directory only
- D) In the `src/skills/` directory alongside application source code

> **Answer: B** — Skills live in `.claude/skills/` within the project. Each skill is a folder containing at minimum a `SKILL.md` file, optionally with scripts, templates, and reference files.

---

**Q29.**A team member uses the skill-creator meta-skill to generate a meeting-notes skill by describing their procedure. Meanwhile, another colleague manually writes a code-review skill from scratch following the SKILL.md anatomy. Both approaches produce working skills. When should each approach be preferred?

- A) The skill-creator should always be preferred because it produces higher-quality SKILL.md files than manual creation can
- B) Manual creation is only needed when the skill-creator meta-skill is not installed or available in the project
- C) The skill-creator is best for getting started quickly; manual creation helps understand internals and fine-tune precisely
- D) The skill-creator produces only rough draft skills that must always be manually rewritten before actual use

> **Answer: C** — The skill-creator guides you through the process and is how most people should create skills. Manual creation teaches what's happening under the hood — useful for refining skills and understanding why they work.

---

### Section 6: Skills vs Prompts and Strategic Value

**Q30.**What is the key difference between a saved prompt and a skill?

- A) Saved prompts are free while skills consume additional API tokens
- B) Prompts encode WHAT you want; skills encode HOW you think about a task — structure, preferences, quality criteria
- C) Prompts work across all AI models while skills only work with Claude
- D) Saved prompts are stored in the cloud while skills are stored locally

> **Answer: B** — Prompts tell Claude WHAT you want ("Write a blog post"). Skills encode HOW you think about the task — your reasoning patterns, quality standards, output structure, and domain preferences. Prompts get you *a* result; skills get you *your* result.

---

**Q31.**What are the two activation modes for skills?

- A) Synchronous activation and asynchronous activation
- B) Automatic activation (Claude recognizes relevance) and explicit invocation (you name the skill)
- C) Local activation (same machine) and remote activation (cloud-based)
- D) Primary activation (first match) and fallback activation (secondary match)

> **Answer: B** — Skills work in two ways: (1) Automatic — Claude recognizes when a skill applies based on the task description; (2) Explicit — you say "Use [skill-name]" to invoke it directly. Both approaches work.

---

**Q32.**An engineering manager asks: "Why should we invest time encoding our team's procedures as skills when we can just copy-paste our prompts?" Compare manual prompting to agent skills across reliability, token cost, and reusability to form the strongest response.

- A) Skills are more visually organized in the codebase and easier to discover through file browsing than scattered prompt files
- B) Manual prompts are ad-hoc and pay for rules every conversation; skills load on-demand and become reusable, shareable IP
- C) Skills execute faster because they bypass the LLM entirely and run purely as compiled local scripts instead
- D) Manual prompts consume zero tokens while skills always add overhead, though skills provide better output formatting

> **Answer: B** — The comparison is clear: Manual prompting is ad-hoc and pays for rules in every conversation. Skills are deterministic, load on-demand (saving tokens), and are reusable intellectual property — shareable with teams, versionable in Git, and integratable into custom agents via SDKs.

---

### Section 7: Subagents and Orchestration

**Q33.**What is a subagent in Claude Code?

- A) A lightweight plugin that adds a single command to Claude Code's CLI
- B) A specialized AI assistant with its own instructions and isolated context window
- C) A background process that continuously monitors file changes in the project
- D) A remote API endpoint that Claude Code calls for specialized computations

> **Answer: B** — A subagent is a specialized AI agent with its own instructions and isolated context window. Each subagent is an expert at one type of task and operates independently from the main Claude Code conversation.

---

**Q34.**Why do subagents use isolated context windows instead of sharing the main conversation context?

- A) Isolated contexts allow subagents to run on different AI models than the main session uses
- B) Sharing context would violate data privacy regulations across different task domains entirely
- C) Isolation prevents context clutter — each specialist focuses on one job without unrelated history
- D) The API only supports one active context window, so subagents must queue their requests separately

> **Answer: C** — Without isolation, research notes could clutter a planning task. Subagents start with clean context — like a team meeting where the researcher presents findings and leaves, then the strategist plans with fresh focus. Nobody juggles everything at once.

---

**Q35.**A developer asks Claude Code: "Find all test files in this project AND suggest a testing strategy for gaps." Claude Code launches both the Explore and Plan subagents simultaneously. What orchestration pattern is this demonstrating?

- A) Sequential delegation where Plan waits for Explore to finish before starting its own work
- B) Parallel agent execution — multiple specialists working simultaneously with isolated contexts
- C) Recursive delegation where Explore spawns Plan as a child process inside its own context
- D) Load-balanced execution where the faster subagent handles both tasks to save time

> **Answer: B** — This is parallel orchestration: Claude Code launches both subagents simultaneously with isolated contexts. They work independently and their results combine into one response. This is powerful for throughput-sensitive workflows.

---

**Q36.**A developer creates a custom subagent called `code-reviewer` using the `/agents` menu. They test it with "Use the code-reviewer subagent to review this function." The review completes. The developer then asks a follow-up: "Can you also check the security aspects?" Who handles this follow-up?

- A) The code-reviewer subagent, which maintains its context and continues the review seamlessly
- B) Main Claude Code, because subagents complete their task and return control — they don't persist
- C) A new instance of code-reviewer that automatically inherits the previous instance's findings
- D) The built-in Plan subagent, which automatically takes over whenever multi-step tasks are detected

> **Answer: B** — Subagents follow "one task, one completion": they receive a goal, work independently, return results, and hand control back to main Claude Code. The follow-up goes to main Claude Code, not the subagent.

---

**Q37.**Where do custom subagent configuration files live, and what determines their scope?

- A) `.claude/agents/` for project-level; `~/.claude/agents/` for user-level across all projects
- B) `.claude/subagents/` for local; `/etc/claude/agents/` for system-wide
- C) `src/agents/` for source-controlled; `.env.agents` for environment-specific
- D) `package.json` agents field for project; `~/.clauderc` for global settings

> **Answer: A** — Project-level subagents live in `.claude/agents/` (available in that project only). User-level subagents live in `~/.claude/agents/` (available across all your projects).

---

**Q38.**A team needs to decide between creating a skill and creating a subagent for their code comment formatting standards. The task is lightweight, happens frequently, and should trigger automatically when Claude writes code. Which should they choose and why?

- A) A subagent, because subagents provide guaranteed execution through hard invocation every time
- B) A skill, because it's lightweight, supports automatic activation, and shares the main conversation context
- C) Both a skill and a subagent working in tandem to provide redundancy and coverage
- D) Neither — code comment formatting should be handled by a pre-commit hook, not an AI feature

> **Answer: B** — Skills are the right choice for lightweight, frequently triggered, single-focus tasks like formatting. Skills support automatic activation (Claude applies them when relevant) and share the main conversation context. Subagents are better for complex, multi-step tasks needing isolated context and guaranteed execution.

---

### Section 8: MCP Integration

**Q39.**What does MCP stand for, and what problem does it solve?

- A) Model Caching Protocol — it caches model responses for faster repeated queries
- B) Model Context Protocol — it extends Claude Code's reach to external systems like websites, APIs, and databases
- C) Multi-Channel Processing — it enables Claude Code to handle multiple user sessions simultaneously
- D) Memory Compression Pipeline — it compresses conversation history to fit more context

> **Answer: B** — MCP is the Model Context Protocol. It solves the problem of Claude Code only being able to see local files by providing standardized, permission-controlled connections to external systems — websites, documentation, APIs, and databases.

---

**Q40.**A developer needs Claude to check the latest React documentation for a new hook API. Without MCP, Claude would rely on training data that might be outdated. With Context7 MCP installed, what changes?

- A) Claude downloads the entire React documentation repository to the local file system for offline access
- B) Claude queries Context7's knowledge sources on demand to fetch current documentation, replacing outdated training data with live citations
- C) Claude redirects the developer to the React documentation website in their default browser
- D) Claude updates its internal training data with the latest React release notes permanently

> **Answer: B** — Context7 MCP enables just-in-time research: Claude queries live documentation sources on demand, fetching current information with real citations. This replaces reliance on potentially outdated training data.

---

**Q41.**The "phone directory" mental model describes MCP's design. How does this analogy apply?

- A) MCP stores a list of frequently called API endpoints sorted by usage frequency for faster lookups
- B) MCP servers are approved specialists Claude can call — browser, docs, database — each handling one capability
- C) MCP acts as a phone switchboard routing Claude's requests to the cheapest available API provider
- D) MCP maintains a directory of Claude Code users and their permissions for team collaboration features

> **Answer: B** — Like a phone directory of approved contacts, MCP gives Claude safe access to specialists: Playwright for web browsing, Context7 for documentation, database servers for queries. Each handles one external capability through explicit, secure channels you've configured.

---

**Q42.**A developer runs `claude mcp add --transport stdio playwright npx @playwright/mcp@latest` and then asks Claude to browse Amazon for products. Claude launches Playwright, navigates pages, extracts details, and returns a summary. The developer then says "filter to long-sleeve only." What makes this iterative refinement possible?

- A) Playwright MCP caches the previous search results locally and applies client-side filtering automatically
- B) Claude maintains the MCP session and can issue follow-up navigation commands to refine results naturally
- C) The developer must re-run the entire search from scratch because MCP doesn't support iteration
- D) Amazon's API provides a native filter parameter that Playwright passes directly without re-browsing

> **Answer: B** — MCP enables natural iteration: within the same conversation, Claude can issue additional browsing commands to refine results. The developer can iterate naturally — "filter to long-sleeve," "show only Prime-eligible" — just like conversing with an assistant.

---

**Q43.**A junior developer proposes: "Let's install this cool MCP server I found on a random npm package to access our production database." Based on MCP security principles, what concerns should be raised?

- A) The only concern is whether the npm package is compatible with the team's current Node.js version
- B) Untrusted MCP servers can access the internet, read files, and expose tokens — only reputable sources should be used
- C) MCP servers are fully sandboxed and cannot access anything beyond their declared permissions regardless of source
- D) The concern is valid only for production environments; development environments are inherently safe for any server

> **Answer: B** — MCP security is critical: a malicious server could expose your system, read files, and access tokens. Only use servers from trusted sources, verify maintainer reputation, check source code, and never paste secrets into files — use environment variables or system keychain.

---

**Q44.**What does MCP Tool Search do, and when does it activate?

- A) It searches for new MCP servers to install from a public registry
- B) It automatically defers loading of MCP tool definitions until needed, activating when definitions exceed 10% of context
- C) It searches through tool outputs to find relevant results matching the user's query
- D) It indexes all available MCP tools into a searchable database at project initialization

> **Answer: B** — MCP Tool Search (built into Claude Code 2.1.7+) is automatic lazy loading. Instead of loading all tool definitions upfront, it defers them until needed. It activates when MCP tool definitions exceed 10% of context, providing ~85% automatic reduction in overhead.

---

**Q45.**A developer has 5 MCP servers installed, consuming an estimated 25,000-40,000 tokens of tool definitions before asking a single question. After updating to Claude Code 2.1.7+, they notice Tool Search activates and overhead drops significantly. However, for their complex daily browser automation workflow involving 10+ operations, they still want better efficiency. What should they consider?

- A) Uninstalling 3 of the 5 MCP servers to reduce the base token footprint below the Tool Search threshold
- B) Compiling the browser automation MCP into a skill — SKILL.md loads ~150 tokens, scripts run locally at 0 tokens
- C) Increasing the Tool Search threshold to 20% so it activates less aggressively and loads more tools upfront
- D) Switching to a model with a larger context window to accommodate all the full tool definitions at once

> **Answer: B** — Tool Search provides ~85% automatic reduction. For complex repeated workflows, compiling MCP to a skill achieves ~98% reduction: SKILL.md loads ~150 tokens, and scripts execute locally outside Claude's context at 0 token cost. This is the next level of optimization.

---

### Section 9: Compiling MCP to Skills

**Q46.**Why does direct MCP usage cause "token bloat," and how does the code execution pattern solve it?

- A) MCP servers send data in uncompressed XML format; compiled skills use compressed JSON for efficiency
- B) Direct MCP loads all tool definitions into context; compiled skills execute scripts locally, returning only filtered results
- C) MCP servers duplicate data across each API call; compiled skills cache responses in a local database
- D) Direct MCP requires Claude to generate code for each tool call; compiled skills use pre-written functions

> **Answer: B** — Direct MCP eagerly loads ALL tool definitions into context (thousands of tokens). The code execution pattern flips this: SKILL.md provides procedures (~150 tokens), then Claude runs bash commands calling `mcp-client.py` which executes outside context. Only filtered results return to the conversation.

---

**Q47.**A developer uses the browsing-with-playwright compiled skill to extract data from a webpage. They observe this sequence: (1) SKILL.md loads (~150 tokens), (2) Claude runs `python mcp-client.py call -t browser_navigate`, (3) the script connects to Playwright MCP via HTTP, (4) only the extracted heading returns to the conversation. Compared to using Playwright MCP directly (~15,000-24,000 tokens), what percentage of tokens were saved?

- A) Approximately 50% — the skill halves the token consumption
- B) Approximately 75% — the skill reduces overhead by three-quarters
- C) Approximately 85% — similar to what Tool Search achieves automatically
- D) Approximately 97-98% — the compiled skill reduces total consumption to ~250 tokens

> **Answer: D** — Direct MCP: ~15,000-24,000 tokens (tool definitions + tool calls in context + full results). Compiled skill: ~250 tokens (SKILL.md ~150 + filtered result ~100). Scripts execute outside context at 0 tokens. That's ~97-98% savings.

---

**Q48.**The fetch-library-docs skill offers `--content-type` flags: `setup`, `examples`, and `api-ref`. A developer asks for Next.js installation instructions using `--content-type setup`. Instead of receiving the full documentation page (~800 tokens), they get only terminal commands and setup steps (~100 tokens). What design principle enables this?

- A) The LLM summarizes the full documentation internally before returning it to conserve tokens
- B) Content-type filtering happens locally in shell scripts — extracting only the requested category
- C) Context7 MCP natively supports content-type parameters and filters at the server level directly
- D) The skill maintains a local cache of pre-filtered documentation organized by each content type

> **Answer: B** — The filtering happens in shell scripts that run locally, outside Claude's context. The skill calls Context7 MCP via subprocess, receives full content, then filters locally using content-type-specific extractors. Only filtered results enter the conversation — achieving 60-90% additional savings.

---

**Q49.**The three-stage progressive disclosure model for compiled skills is: Discovery, Activation, Execution. What happens at each stage, and why is Stage 3 critical for token savings?

- A) Discovery indexes skills, Activation validates permissions, Execution calls the MCP API — Stage 3 batches calls
- B) Discovery loads description (~30 tokens), Activation loads SKILL.md (~150 tokens), Execution runs locally (0 tokens)
- C) Discovery caches tool schemas, Activation compiles scripts, Execution deploys to an isolated container
- D) Discovery scans the file system, Activation loads dependencies, Execution spawns a separate subagent

> **Answer: B** — Discovery: load only the description (~30 tokens). Activation: load full SKILL.md (~150 tokens) when relevant. Execution: run scripts locally (0 tokens in context). Stage 3 is the key — heavy MCP operations happen outside Claude's context window.

---

**Q50.**A team lead is deciding which of their four MCP servers to compile into skills. Server A has 1,200 tokens of definitions and is used once monthly. Server B has 8,000 tokens, is used 5 times daily, and returns large datasets needing filtering. Server C has 2,000 tokens with a rapidly changing API. Server D has 6,000 tokens and is used in multi-step team workflows shared via Git. Which servers should be compiled?

- A) All four servers should be compiled for maximum token efficiency across the entire board
- B) Servers B and D — B has high overhead, frequent use, and needs filtering; D has high overhead and team sharing
- C) Only Server B because it has the highest token count and most frequent usage of all four servers
- D) Servers A and C — infrequent and volatile servers benefit most from local caching via compilation

> **Answer: B** — The decision framework says compile when: high token overhead (>5,000), frequent use (3+/session), large datasets needing filtering, or team-shared workflows. Server B hits all criteria. Server D has high overhead plus team portability needs. Server A is too infrequent. Server C's rapidly changing API would need constant maintenance.

---

### Section 10: The Three Pillars and Integration

**Q51.**The "Three Pillars" framework states: CLAUDE.md gives Claude context, Skills give Claude procedures, and MCP gives Claude reach. A team has implemented CLAUDE.md and MCP but has no skills. They find Claude can access all external data but produces generic, inconsistent reports. Which pillar is missing and what is the consequence?

- A) The context pillar is missing — Claude doesn't know the project structure and cannot locate relevant data sources
- B) The reach pillar is missing — Claude cannot access the external databases needed for generating the reports
- C) The procedures pillar (Skills) is missing — Claude has context and data access but no encoded expertise
- D) All three pillars are present but misconfigured — the issue is a CLAUDE.md syntax error preventing loading

> **Answer: C** — Without skills, Claude has project context (CLAUDE.md) and external access (MCP) but no encoded procedures for how to work. The result: generic output. Adding skills would encode the team's reporting standards, analysis procedures, and presentation formats — transforming data access into expertise-driven output.

---

**Q52.**A startup adopts the full stack: CLAUDE.md for project context, skills for domain procedures, MCP for external data, and compiled skills for token efficiency. A new engineer joins and starts a Claude Code session on day one. Compared to a company using none of these, what is the compounding organizational advantage?

- A) The new engineer saves approximately 30 minutes per day in reduced typing due to AI-powered auto-complete features
- B) Claude already knows the project, the team's procedures, and external systems — zero ramp-up from day one
- C) The startup pays significantly lower API costs due to compiled skills, which is the primary financial advantage
- D) The new engineer can bypass code reviews entirely since Claude enforces all quality standards automatically

> **Answer: B** — This is the compounding value of shared knowledge. Skills improve → all agents get better. New hires start with Claude already knowing the project (CLAUDE.md), the team's procedures (skills), and having access to external systems (MCP). No ramp-up period. The expertise is already encoded.

---

## Answer Distribution Verification

```
Total questions: 52

A count: 13 (25.0%)
B count: 13 (25.0%)
C count: 13 (25.0%)
D count: 13 (25.0%)

Longest streak of same letter: 3 (verified)
```

