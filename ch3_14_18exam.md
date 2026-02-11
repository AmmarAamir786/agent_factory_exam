# Exam: Claude Code — Settings, Hooks, Plugins, Ralph Loop & Best Practices (Lessons 14–18)

This exam covers Claude Code's settings hierarchy, event-driven hooks, plugin ecosystem, the Ralph Wiggum Loop for autonomous iteration, and production best practices from the Claude Code team. Each question has exactly one correct answer. Read all options carefully before selecting.

**Total Questions: 40**

---

### Section A: Settings Hierarchy

**Q1.** Claude Code's settings hierarchy consists of three levels. What is the correct precedence order from highest to lowest priority?

- A) User > Project > Local, because user-level settings are the most deliberate choices
- B) Project > Local > User, because team standards should always take priority
- C) Project > User > Local, because shared configuration overrides personal preferences
- D) Local > Project > User, because the most specific context takes highest priority

> **Answer: D** — Claude Code follows a "most specific wins" precedence model. Local settings (`.claude/settings.local.json`) override project settings (`.claude/settings.json`), which override user settings (`~/.claude/settings.json`). This enables personal experimentation without disrupting team standards.

---

**Q2.** Why does Claude Code use a three-level settings hierarchy instead of a single global configuration file?

- A) It enables team standards, personal preferences, and temporary experiments to coexist without conflicts
- B) It mirrors the three-tier architecture of modern web applications for consistency
- C) It prevents unauthorized users from modifying project-critical settings remotely
- D) It reduces file size by distributing configuration across multiple smaller files

> **Answer: A** — The three-level hierarchy solves the fundamental tension between team consistency and individual flexibility. User settings carry personal defaults, project settings enforce team agreements, and local settings provide a safe space for experimentation—all without stepping on each other's configurations.

---

**Q3.** A developer has `outputStyle: "Concise"` in their user settings and `outputStyle: "Explanatory"` in the project settings. They create a `.claude/settings.local.json` with `outputStyle: "Verbose"` for a debugging session. After the session, they delete the local settings file. Which output style is now active and why?

- A) "Concise" — deleting the local file reverts all settings to user-level defaults
- B) "Verbose" — the last active setting persists even after the file is deleted
- C) "Explanatory" — the project-level setting is the most specific remaining override
- D) No output style is set — deleting a settings file clears the entire chain

> **Answer: C** — With the local file deleted, the precedence chain falls back to the next most specific level: project settings. Since project settings have `outputStyle: "Explanatory"`, that becomes active. The user-level "Concise" setting only applies when no project-level override exists.

---

**Q4.** What is the primary purpose of `.claude/settings.local.json` in the settings hierarchy?

- A) Storing security-sensitive credentials that should never be shared with the team
- B) Providing a private space for machine-specific settings and temporary experiments that stays off version control
- C) Caching frequently used settings for faster Claude Code startup times
- D) Backing up project settings in case the main configuration file is corrupted

> **Answer: B** — Local settings exist specifically for personal, temporary, or machine-specific configurations. They should be added to `.gitignore` so they never get committed to version control. This allows developers to experiment with settings without affecting team standards or exposing personal configurations.

---

**Q5.** A team lead wants to ensure no team member's Claude Code session can read `.env` files for security reasons. They also want each developer to choose their own preferred output style. Where should they configure each setting?

- A) Both settings at the user level, since each developer should manage their own security preferences
- B) The `.env` denial at the project level in `.claude/settings.json`, and output style at the user level in `~/.claude/settings.json`
- C) Both settings at the local level, since they vary per developer's machine configuration
- D) The `.env` denial at the local level for each machine, and output style at the project level for consistency

> **Answer: B** — Security policies like denying access to `.env` files belong at the project level where they're enforced for all team members through version control. Personal preferences like output style belong at the user level where each developer controls their own experience across all projects.

---

**Q6.** Where is the user-level settings file located in Claude Code's settings hierarchy?

- A) `.claude/settings.json` inside the current project directory
- B) `.claude/settings.local.json` inside the current project directory
- C) `/etc/claude/settings.json` in the system configuration directory
- D) `~/.claude/settings.json` in the user's home directory

> **Answer: D** — User settings live at `~/.claude/settings.json` in the home directory. This location ensures the settings apply across all projects on the machine. Project settings use `.claude/settings.json` within the project, and local settings use `.claude/settings.local.json`.

---

**Q7.** A developer accidentally deletes the `.claude/` directory from their project. They notice Claude Code still works but behaves differently. A colleague asks them what happened. Which explanation is most accurate?

- A) All project-level and local-level settings were lost, so Claude Code reverted to user-level defaults for everything, and any team-shared configuration needs to be restored from version control
- B) Only the local settings were affected because project settings are stored in a cloud backup
- C) Claude Code automatically regenerated the directory with factory defaults for safety
- D) Nothing meaningful changed because Claude Code only reads settings at session start

> **Answer: A** — The `.claude/` directory contains both `settings.json` (project-level, shared with team) and `settings.local.json` (local overrides). Deleting it removes all project customization, causing Claude Code to fall back to user-level settings. Since `settings.json` should be in version control, it can be restored via git; local settings would be permanently lost.

---

### Section B: Hooks — Event-Driven Automation

**Q8.** Which exit code should a hook script return to block an action and display an error message?

- A) Exit code 0, which signals the hook processed successfully
- B) Exit code 1, which is the standard Unix error code
- C) Exit code 2, which specifically signals "block this action"
- D) Exit code 127, which indicates the command was not found

> **Answer: C** — In Claude Code's hook system, exit code 0 means success (stdout is processed normally), exit code 2 specifically means "block the action" (error message displayed), and any other exit code produces a non-blocking warning. This three-tier system gives hooks precise control over Claude's behavior.

---

**Q9.** Why are hooks considered more reliable than prompting instructions for enforcing coding standards like formatting?

- A) Hooks execute as automated scripts on every matching event, removing the dependency on Claude choosing to follow the instruction
- B) Hooks run at a lower system level with root privileges that override Claude's behavior
- C) Hooks are processed before Claude sees the conversation, filtering out non-compliant requests
- D) Hooks store formatting rules in a compiled binary that Claude cannot override

> **Answer: A** — The key insight about hooks is that they turn suggestions into guaranteed automation. While you can tell Claude to "always format code after editing," it might forget. A hook executes your script automatically on every matching event, making compliance deterministic rather than probabilistic.

---

**Q10.** A team wants to automatically run Prettier after every file that Claude writes or edits, but not after bash commands or file reads. Which hook configuration achieves this?

- A) A `PreToolUse` hook with matcher `"Write|Edit"` that runs the Prettier command before changes
- B) A `PostToolUse` hook with an empty matcher that runs Prettier after every tool execution
- C) A `PostToolUse` hook with matcher `"Write|Edit"` that runs the Prettier command after changes
- D) A `SessionEnd` hook that runs Prettier on all modified files when the session closes

> **Answer: C** — `PostToolUse` fires after a tool completes, which is the right timing for formatting (format after the file is written). The matcher `"Write|Edit"` restricts the hook to only fire for Write and Edit tools, excluding Bash, Read, and other tools. `PreToolUse` would fire before changes happen, which is too early for formatting.

---

**Q11.** A developer creates a `PreToolUse` hook with matcher `"Bash"` that checks if a command contains `rm -rf`. The hook outputs a warning to stderr but exits with code 0. What happens when Claude attempts to run `rm -rf ./temp`?

- A) The command is blocked and Claude sees the warning message about dangerous operations
- B) The command proceeds normally because exit code 0 signals success, and the stderr warning is logged but does not block
- C) The hook fails silently because stderr output is never processed by Claude Code
- D) Claude Code crashes because hooks cannot write to stderr during PreToolUse events

> **Answer: B** — Exit code 0 means success—the action is allowed to proceed. While the warning is written to stderr, only exit code 2 actually blocks actions. To block the `rm -rf` command, the hook would need to exit with code 2. The developer's intent was correct, but the implementation uses the wrong exit code for blocking.

---

**Q12.** A team argues that hooks and CLAUDE.md instructions serve the same purpose because both can tell Claude to format code after edits. A senior engineer disagrees. One team member says hooks are "just automated CLAUDE.md rules," while another claims CLAUDE.md is redundant if you have enough hooks. What is the most accurate assessment of the relationship between hooks and CLAUDE.md?

- A) Hooks and CLAUDE.md are interchangeable — any rule in CLAUDE.md can be fully replaced by a hook with identical reliability
- B) CLAUDE.md is strictly superior because it provides richer context, whereas hooks can only run simple shell commands
- C) Hooks should replace all CLAUDE.md rules over time, since deterministic execution always outperforms probabilistic instruction-following
- D) Hooks guarantee execution of automated tasks but cannot guide Claude's reasoning or creative decisions, while CLAUDE.md shapes how Claude thinks and makes choices — they serve complementary purposes

> **Answer: D** — Hooks automate predictable behaviors (format code, validate commands, log activity) with guaranteed execution. CLAUDE.md provides context that shapes Claude's reasoning, coding style, architectural decisions, and creative problem-solving — things a shell script cannot express. "Always use path aliases" is a CLAUDE.md rule; "run Prettier after edits" is a hook. They address fundamentally different aspects of Claude's behavior.

---

**Q13.** How do hooks receive input data from Claude Code when they are triggered?

- A) Through command-line arguments passed to the hook script at execution time
- B) Through environment variables set by Claude Code before running the hook
- C) Through JSON data piped via stdin, containing event-specific fields like tool name and inputs
- D) Through a temporary file written to the `.claude/hooks/` directory before execution

> **Answer: C** — All hooks receive JSON input via stdin. The JSON includes common fields like `session_id`, `cwd`, and `hook_event_name`, plus event-specific fields. For example, `PreToolUse` and `PostToolUse` receive `tool_name` and `tool_input`, while `UserPromptSubmit` receives the `prompt` field. Scripts parse this with tools like `jq`.

---

**Q14.** A developer's `SessionStart` hook isn't producing any output when Claude Code launches. They've verified the settings.json configuration is correct and the script works when tested manually with `echo '{}' | bash .claude/hooks/session-info.sh`. What is the most likely remaining issue?

- A) The hook script needs to be recompiled after any changes to take effect in Claude Code
- B) The hook script file doesn't have execute permissions, preventing Claude Code from running it
- C) SessionStart hooks only fire on the second launch after configuration, not the first
- D) The script path in settings.json uses a relative path that resolves differently from Claude Code's working directory

> **Answer: B** — The debugging checklist for hooks specifies checking that the script is executable with `chmod +x`. Manual testing with `bash .claude/hooks/script.sh` bypasses the execute permission check because `bash` explicitly interprets the file. Claude Code may require the file to be directly executable, making this the most likely remaining issue after confirming JSON syntax and manual functionality.

---

**Q15.** What is the key difference between what hooks provide and what skills or MCP servers provide in Claude Code's architecture?

- A) Hooks are newer and designed to eventually replace both skills and MCP servers entirely
- B) Hooks can only run during specific tool events, while skills run continuously in the background
- C) Hooks require external API keys to function, whereas skills and MCP use local resources
- D) Hooks add automation to existing workflows by triggering on events, while skills and MCP add new capabilities that Claude can use

> **Answer: D** — This is a critical architectural distinction. Hooks automate behavior — they trigger scripts when events occur (file edited, session starts, tool runs). Skills give Claude new domain expertise and commands. MCP servers connect Claude to external services and data. They are complementary: hooks automate the predictable, skills extend expertise, MCP extends reach.

---

### Section C: Plugins — Discover and Install

**Q16.** What components can a Claude Code plugin bundle together into a single installable package?

- A) Skills, commands, agents, hooks, and MCP server configurations
- B) Only skills and commands, since hooks and MCP require separate installation
- C) Settings files, environment variables, and user credentials for quick onboarding
- D) Docker containers, CI/CD pipelines, and cloud deployment configurations

> **Answer: A** — A plugin bundles multiple Claude Code components into one package: skills (autonomous capabilities), commands (slash commands), agents (specialized subagents), hooks (event automation), and MCP servers (external integrations). This is the "complete capability package" concept — one install gets everything working together.

---

**Q17.** Why does Claude Code use a plugin architecture instead of building all integrations directly into the core tool?

- A) Built-in integrations would make Claude Code too expensive for individual developers
- B) Core integrations would require Claude Code to be recompiled with every new service added
- C) Plugin architecture was chosen solely to enable a revenue-sharing marketplace model
- D) Plugins let users opt into specific capabilities deliberately, keeping the core lightweight and letting the community extend functionality without core changes

> **Answer: D** — The plugin architecture follows a deliberate design philosophy. Not every user needs GitHub integration, LSP support, or autonomous iteration. Plugins let users choose capabilities they actually need, keep the core tool focused, and enable community contributions without requiring changes to Claude Code's codebase. The Ralph Wiggum Loop being a plugin rather than built-in exemplifies this.

---

**Q18.** A solo developer works across three projects: a React frontend, a Python API, and a shared documentation site. They want TypeScript language server support available everywhere and a custom documentation skill only for the docs project. How should they install each?

- A) Install the TypeScript LSP plugin at user scope so it's available in all projects, and install the documentation skill at project scope in the docs repository only
- B) Install both at project scope in each respective repository so each project is self-contained
- C) Install both at local scope to avoid accidentally affecting other developers on any team
- D) Install the TypeScript LSP at project scope in the React project and create a symlink for other projects

> **Answer: A** — User scope (`~/.claude/`) makes a plugin available across all projects on the machine — perfect for universally useful tools like the TypeScript LSP. Project scope (`.claude/settings.json`) restricts a plugin to a specific repository — ideal for the documentation skill that only applies to the docs site. This matches the general recommendation: personal tools at user scope, project-specific tools at project scope.

---

**Q19.** A development team is adding GitHub integration to their Claude Code setup. One engineer proposes writing a custom MCP server for GitHub. Another suggests checking the plugin marketplace first. A third wants to copy GitHub-related scripts from a blog post. Which approach best follows Claude Code's recommended workflow?

- A) Write the custom MCP server first, since it provides the most control over the integration
- B) Copy the blog post scripts as a starting point, then customize them for the team's needs
- C) Check the plugin marketplace for an existing GitHub plugin, install it if available, and only build custom if the plugin doesn't meet requirements
- D) Use all three approaches simultaneously to compare which performs best

> **Answer: C** — The core plugin principle is "check what exists before building from scratch." The official marketplace includes a GitHub integration plugin that handles MCP configuration, bundled skills, and automation hooks. Installing it is a single command. Custom development should only happen when existing plugins don't meet specific requirements.

---

**Q20.** What is the relationship between a plugin and a marketplace in Claude Code's ecosystem?

- A) A marketplace is a specific type of plugin that manages other plugins automatically
- B) A plugin is a folder containing Claude Code components, while a marketplace is a catalog that lists and organizes multiple plugins for discovery
- C) Marketplaces are required for plugin installation — plugins cannot be used without one
- D) A plugin runs locally while a marketplace runs in the cloud as a hosted service

> **Answer: B** — The analogy is app (plugin) to app store (marketplace). A plugin is a folder with skills, agents, hooks, MCP configs, and a manifest. A marketplace is a catalog (`marketplace.json`) listing multiple plugins. You can use plugins without a marketplace (via `--plugin-dir`), but marketplaces provide discovery, organization, and update tracking. Multiple marketplaces can coexist.

---

**Q21.** What is the minimum required file for creating a valid Claude Code plugin?

- A) A `plugin.json` manifest inside a `.claude-plugin/` directory, containing name, description, version, and author fields
- B) A `SKILL.md` file at the root of the plugin directory with the skill's trigger description
- C) A `package.json` file with Claude Code listed as a dependency in the plugin root
- D) A `settings.json` file inside the `.claude/` directory with the plugin's hook configurations

> **Answer: A** — Every plugin requires a `plugin.json` manifest inside `.claude-plugin/` at minimum. The manifest needs just four fields: `name`, `description`, `version`, and `author`. Components like skills, agents, hooks, and MCP configs go at the root level of the plugin directory, not inside `.claude-plugin/`.

---

**Q22.** A team lead argues that all Claude Code capabilities should be built as custom plugins because "we'll have full control." A senior architect argues this is wasteful. The lead responds that marketplace plugins might not match their exact requirements. What is the strongest counter-argument to building everything custom?

- A) Custom plugins are inherently less secure than marketplace plugins because they bypass Anthropic's review process
- B) The official marketplace only updates plugins annually, so custom plugins provide fresher functionality
- C) Marketplace plugins represent community-tested solutions for common workflows, and the time saved on standard integrations can be redirected to building custom plugins only where genuine differentiation is needed
- D) Custom plugins cannot be shared across team members, limiting their usefulness to individual developers

> **Answer: C** — The "Composition Over Creation" mental model applies here. Marketplace plugins for standard tasks (git, GitHub, Slack, LSP) are community-tested and maintained. Building these from scratch wastes effort that could be spent on team-specific capabilities where no existing solution fits. The recommended approach is marketplace-first, custom-only-when-necessary.

---

### Section D: Ralph Wiggum Loop — Autonomous Iteration

**Q23.** What Claude Code mechanism does the Ralph Wiggum Loop use to enable autonomous iteration?

- A) A `PostToolUse` hook that monitors every tool execution for completion signals
- B) A Stop hook that intercepts Claude's normal exit behavior and reinjects continuation prompts when completion criteria aren't met
- C) A `SessionEnd` hook that restarts the session automatically with the same initial prompt
- D) A custom MCP server that manages iteration state in an external database

> **Answer: B** — Ralph Loop uses the Stop hook, which fires when Claude is about to stop working. The hook checks if the completion promise text appears in Claude's last output. If not found and iterations remain, it reinjects a prompt telling Claude to continue. If found, Claude is allowed to stop. This creates a persistence layer that won't let Claude quit until success criteria are met.

---

**Q24.** Why is `--max-iterations` described as the "primary safety net" for Ralph Wiggum Loop rather than the completion promise?

- A) The completion promise uses AI inference to detect success, which can produce false positives
- B) Max iterations is cheaper to evaluate because it doesn't require parsing Claude's output
- C) The completion promise can be modified during runtime, making it unreliable as a safety mechanism
- D) The completion promise relies on fragile exact string matching that cannot be changed during runtime, so if the expected text never appears in Claude's output, only the iteration limit prevents an infinite loop

> **Answer: D** — The `--completion-promise` parameter is static — set once at loop start and checked via exact string matching on every iteration. It cannot be modified, added, or made conditional during runtime. If the exact string never appears in Claude's output (due to formatting changes, typos, or unexpected tool output), the loop would run forever without `--max-iterations` as a hard stop.

---

**Q25.** A developer wants to use Ralph Loop to fix all TypeScript errors in their project. They're debating between two approaches: (1) `--completion-promise "Found 0 errors"` relying on the TypeScript compiler's natural output, or (2) using the embedded promise pattern with `<promise>TYPES_FIXED</promise>`. Under what condition would approach 1 fail while approach 2 succeeds?

- A) When the project has more than 100 TypeScript errors, since the compiler truncates its output
- B) When Claude uses a different TypeScript compiler version that formats output differently
- C) When the TypeScript compiler outputs "Found 0 errors." with a period or "0 errors found" with different wording, causing the exact string match to fail
- D) When the TypeScript errors span multiple files, since the compiler reports per-file rather than global counts

> **Answer: C** — The completion promise uses exact string matching. If `tsc` outputs "Found 0 errors." (with period) instead of "Found 0 errors" (without period), or uses different phrasing across versions, the match fails. The embedded promise pattern avoids this fragility because Claude is explicitly instructed to output the exact marker text, giving full control over the completion signal regardless of tool output format variations.

---

**Q26.** A data scientist wants to use Ralph Loop for the task: "Analyze our customer churn data, identify the top 3 factors, and create a presentation recommending retention strategies." Why is this a poor fit for autonomous iteration?

- A) Ralph Loop cannot process data files or generate presentations due to tool limitations
- B) The task requires subjective judgment for identifying "top" factors and crafting strategy recommendations, which lacks the objective, verifiable completion criteria Ralph Loop needs
- C) Data analysis tasks always complete in fewer than 10 iterations, below Ralph Loop's minimum
- D) Ralph Loop can only work with code files, not data files or presentation formats

> **Answer: B** — Ralph Loop's golden rule states it excels when success is "objective, verifiable, and deterministic — measurable by tools, not human judgment." Identifying "top factors" involves analytical judgment, and "recommending strategies" requires business context and taste. There's no deterministic completion signal — no tool can verify that the strategies are good. This task needs interactive human collaboration, not autonomous iteration.

---

**Q27.** A developer runs `/ralph-loop "Upgrade React from v16 to v19 and fix all breaking changes" --max-iterations 30 --completion-promise "Build completed successfully"`. After 12 iterations, they notice Claude keeps encountering the same peer dependency conflict. What should they do?

- A) Increase `--max-iterations` to 60, since complex upgrades naturally require more attempts
- B) Change the completion promise to a less strict condition so the loop can exit sooner
- C) Add a second completion promise condition to handle the dependency error specifically
- D) Cancel the loop with `/cancel-ralph`, manually resolve the peer dependency conflict, then restart the loop to continue with the remaining issues

> **Answer: D** — When the same error repeats 3+ times, Claude is stuck in a local optimum. The recommended intervention is to cancel (`/cancel-ralph`), resolve the blocking issue manually (peer dependency conflicts often require human judgment about which version to pin), then restart. Increasing iterations wastes budget on a problem Claude can't solve alone. Completion promises cannot be modified or added during runtime.

---

**Q28.** The Ralph Wiggum Loop plugin was created by Geoffrey Huntley and formalized by Boris Cherny. A product manager argues that autonomous iteration should be a built-in Claude Code feature rather than a plugin, since it's so useful. A staff engineer disagrees. What is the strongest argument for keeping Ralph Loop as an opt-in plugin?

- A) Autonomous iteration carries significant cost and control risks — API spending can reach $50-150 per session — and making it a plugin ensures users deliberately opt in rather than accidentally triggering expensive long-running loops
- B) Plugin architecture allows Ralph Loop to be updated independently of Claude Code's release cycle
- C) Built-in features cannot use Stop hooks, so the technical implementation requires plugin architecture
- D) Anthropic's legal team requires all autonomous features to be distributed as separate packages

> **Answer: A** — The source explicitly states: "Autonomous iteration carries cost and control risks. Making it a plugin ensures users opt in deliberately, not accidentally." A 14-hour autonomous session can cost $50-100+ in API credits. If this were a default behavior, users could inadvertently trigger expensive loops. The plugin model creates a deliberate, conscious choice to enable autonomous iteration.

---

**Q29.** A DevOps engineer wants to set up a Ralph Loop to deploy their application to staging and resolve all errors until the health check passes. They write: `/ralph-loop "Deploy to staging and fix issues" --max-iterations 25 --completion-promise "Health check: 200 OK"`. Before running, a colleague reviews and suggests improvements. Which improvement would have the most impact on reliability?

- A) Reducing max-iterations to 10 to keep costs low during the initial test run
- B) Adding a git commit checkpoint command at the start of the prompt description
- C) Changing the matcher pattern to only trigger on deployment-related tools
- D) Using the embedded promise pattern — instructing Claude to output `<promise>DEPLOYED</promise>` when the health check passes, rather than relying on the health check endpoint's exact output format

> **Answer: D** — The embedded promise pattern is recommended for reliability because the developer controls the exact completion signal. Health check output can vary across environments ("200 OK" vs "HTTP/1.1 200 OK" vs "Status: 200"). Instructing Claude to output a specific marker when it confirms the health check passes removes dependency on unpredictable tool output formatting.

---

**Q30.** Why does the Ralph Wiggum Loop's self-correcting behavior improve over iterations, even though Claude doesn't have traditional machine learning during a session?

- A) Claude Code is stateful — it retains the full conversation history, so each reinjected prompt adds context about what failed previously, allowing Claude to adjust its approach based on accumulated error information
- B) The Stop hook applies reinforcement learning signals after each failed iteration
- C) Claude downloads updated model weights from Anthropic's servers after each failed attempt
- D) The plugin maintains a separate error database that Claude queries before each new iteration

> **Answer: A** — Claude Code conversations are stateful — the entire conversation history persists. When the Stop hook reinjects a continuation prompt after a failed iteration, Claude can see all previous attempts, their errors, and what didn't work. This accumulated context allows Claude to try different approaches, creating a self-correcting loop without any actual learning or model updates.

---

**Q31.** A startup CEO reads about Ralph Loop and proposes using it for three tasks simultaneously: (1) fixing all linting errors, (2) adding unit tests for uncovered functions, and (3) updating the README documentation. They suggest a single Ralph Loop with completion promise "ALL_TASKS_DONE". A technical lead pushes back. The CEO argues this is more efficient since it's one command. What is the most compelling technical reason the CEO's approach will likely fail?

- A) Ralph Loop can only run one type of tool per session, so mixed tasks cause tool conflicts
- B) The loop will consume too much context window by tracking three independent goals simultaneously
- C) Multi-goal tasks lack a single clear completion signal — Claude may finish linting but get stuck on tests, and the static completion promise cannot distinguish partial completion from total completion, leading to wasted iterations on already-completed subtasks
- D) Claude Code's API rate limits prevent more than one type of code modification per session

> **Answer: C** — Multi-goal tasks are explicitly listed as a poor fit for Ralph Loop because there's "no single completion signal." With three independent goals, Claude might fix all linting errors but get stuck writing tests. The static `--completion-promise` can't detect that linting is done while tests aren't. The recommended approach is breaking this into separate focused loops, each with its own clear completion criteria.

---

### Section E: The Creator's Workflow — Best Practices

**Q32.** According to Boris Cherny, what is the "single biggest productivity unlock" when using Claude Code?

- A) Using Opus 4.5 with thinking enabled for all tasks without exception
- B) Running multiple parallel sessions, each with its own isolated context window
- C) Maintaining a comprehensive CLAUDE.md file that covers every possible scenario
- D) Enabling auto-accept mode so Claude can execute without permission prompts

> **Answer: B** — Boris Cherny explicitly calls parallel sessions "the single biggest productivity unlock, and the top tip from the team." He maintains 15-20 concurrent sessions. The key insight is that parallel directories (via git worktrees or separate checkouts) give each session its own isolated context window, enabling true parallelism where multiple Claude instances work different problems simultaneously.

---

**Q33.** Why does the Claude Code team recommend using Opus 4.5 with thinking even though it's slower per response than Sonnet?

- A) Opus 4.5 has a larger context window that prevents the need for frequent `/clear` commands
- B) Opus 4.5 requires less steering and correction, making total task completion faster despite slower individual responses — a "wrong fast answer" costs more time than a "right slow answer"
- C) Opus 4.5 is the only model that supports Plan Mode and parallel session features
- D) Sonnet produces output that fails hook validation more frequently, creating additional iterations

> **Answer: B** — Boris's counterintuitive insight is that optimizing for per-response speed is a false economy. Opus 4.5 "requires less correction and iteration, making total task completion faster despite slower per-response times." A fast but incorrect response triggers debugging, correction, and re-execution — ultimately taking longer than a single slower but correct response.

---

**Q34.** A developer has been using Claude Code for three weeks and notices Claude keeps making the same import path mistake — using relative paths instead of the project's `@/` path aliases. They've corrected Claude five times across different sessions. What workflow change would permanently fix this issue?

- A) Creating a `PreToolUse` hook that scans all file writes for incorrect import paths
- B) Switching to a different model that handles import paths more accurately
- C) Adding a detailed import path specification to the project's `package.json` file
- D) After the next correction, telling Claude: "Update your CLAUDE.md so you don't make that mistake again" — letting Claude write a self-correcting rule that persists across all future sessions

> **Answer: D** — This is the "CLAUDE.md self-writing" pattern from Boris's workflow. The magic phrase "Update your CLAUDE.md so you don't make that mistake again" leverages Claude's understanding of what went wrong to write a precise, lasting rule. Claude is "eerily good at writing rules for itself." Each correction becomes institutional memory that compounds over time, preventing entire categories of mistakes across all future sessions.

---

**Q35.** A team member asks Claude to investigate how the authentication system handles token refresh and whether any OAuth utilities already exist in their large codebase. The investigation requires reading dozens of files. Following the Claude Code team's best practices, how should this investigation be structured?

- A) Delegate the investigation to a subagent so it explores in its own context window and reports back findings, keeping the main conversation context clean for subsequent implementation work
- B) Ask Claude to investigate directly in the current session since it needs the full conversation history for context
- C) Create a new CLAUDE.md section documenting the authentication system before investigating
- D) Split the investigation across three parallel sessions, one for each authentication component

> **Answer: A** — The "investigation pattern" from the best practices recommends using subagents for codebase research because they explore in their own context window. When Claude reads many files during investigation, all that content consumes the main session's context window. A subagent reads the files, synthesizes findings, and reports a summary — keeping the main context clean for the actual implementation work that follows.

---

**Q36.** A developer is on their fifth correction of Claude in a single session. Claude keeps producing code that doesn't match the project's patterns, despite earlier corrections. Following the Claude Code team's best practices, what is the recommended course of action?

- A) Continue correcting until Claude learns the pattern, since persistence will eventually work
- B) Switch to a smaller, faster model that might handle the specific pattern more accurately
- C) Run `/clear` to reset the context and start fresh with a more specific prompt that incorporates what was learned from the failed attempts
- D) Create a hook that automatically rejects any code that doesn't match the project's patterns

> **Answer: C** — The "correction spiral" is a documented failure pattern. After two failed corrections, the context is cluttered with failed approaches and contradictory information. The official best practice is to `/clear` the context and craft a fresh, specific prompt incorporating all the insights from the failed attempts. Starting clean often succeeds immediately where continued correction fails.

---

**Q37.** Boris Cherny's team uses a "Claude-reviews-Claude" pattern where Session A creates a plan and Session B reviews it critically. A project manager suggests this is wasteful — "why not just have one Claude review its own plan?" They argue that a single session can be prompted to "review this plan critically" just as effectively. What is the strongest counter-argument to the single-session approach?

- A) A reviewing session has fresh context that hasn't been influenced by the exploration and reasoning that led to the plan, enabling it to catch blind spots that the writing session's accumulated context makes invisible — analogous to how code authors miss their own bugs
- B) Two sessions cost less in total API credits than one long session with review steps
- C) Claude Code technically cannot review its own output within the same session due to context limitations
- D) The single-session approach works equally well for simple plans but fails on complex multi-file changes

> **Answer: A** — The fresh context is the key advantage. Session A's context is filled with the exploration, dead ends, and reasoning that led to the plan. This accumulated context creates blind spots — the same way a code author who debugged for hours can't easily spot their own mistakes. Session B starts clean, seeing only the plan itself, enabling it to surface concerns that Session A's sunk-cost context makes invisible.

---

**Q38.** What is the "meta-pattern" that connects most failure modes in Claude Code usage, according to the official best practices?

- A) Insufficient model capability — most failures occur because users choose models too small for their tasks
- B) Permission configuration errors — most failures stem from overly restrictive or overly permissive settings
- C) Context pollution — either too much irrelevant information or failed approaches cluttering the conversation, degrading Claude's performance
- D) Network instability — most failures are caused by interrupted connections during long-running operations

> **Answer: C** — The meta-pattern explicitly stated in the best practices is that "most failures stem from context pollution — either too much irrelevant information, or failed approaches cluttering the conversation." This connects to the fundamental constraint: Claude's context window fills up fast, and performance degrades as it fills. Kitchen sink sessions, correction spirals, and infinite exploration all share this root cause.

---

**Q39.** A knowledge worker wants to adopt Claude Code best practices but doesn't write code. They ask which of Boris's techniques would translate best to their document-focused workflow. Which combination of practices provides the most direct value for a non-developer?

- A) Git worktrees and PostToolUse hooks for formatting, since these are the highest-impact practices
- B) Parallel sessions for different projects, session-end review skills to capture decisions, and the "give Claude the problem not the solution" approach for autonomous research across connected tools
- C) Ralph Wiggum Loop for autonomous document iteration and LSP plugins for document intelligence
- D) CLAUDE.md self-writing rules and `/permissions` configuration, since these are the only practices applicable outside development

> **Answer: B** — Parallel sessions (the "biggest productivity unlock") apply to any domain — open multiple windows for different projects. Session-end review skills capture decisions and follow-ups before closing any session. The autonomous problem-solving pattern ("paste a confusing email thread and say 'draft a response'") leverages MCP integrations with tools like Google Drive, Notion, and Slack. These are explicitly listed as knowledge worker equivalents in the best practices.

---

**Q40.** A CTO reads about Boris Cherny's workflow and mandates that every developer on the team must immediately adopt all practices: 20 parallel sessions, Ralph Loop for everything, comprehensive CLAUDE.md files, and custom subagents for every workflow. An engineering manager pushes back. The CTO argues that if these practices work for the creator of Claude Code, they should work for everyone. What is the most nuanced reason the CTO's mandate is likely counterproductive?

- A) These practices only work with enterprise-tier API access that most teams cannot afford
- B) The practices require specific IDE integrations that aren't available on all operating systems
- C) Boris explicitly warns against mandating specific tool configurations, recommending that each team discover their own optimal workflow naturally
- D) Expert workflows evolved through iterative mastery of fundamentals — Boris's 20 sessions reflect his specific role managing a massive product, while most developers should start with 2-3 sessions and build complexity as they internalize each technique, since the cognitive overhead of managing unfamiliar practices simultaneously reduces rather than increases productivity

> **Answer: D** — The source explicitly states: "Boris runs many sessions because he manages a massive software product. You do not need this many. Start with 2-3." The practices evolved through iterative experience — Plan Mode discipline, CLAUDE.md self-writing, and subagent ecosystems each require mastery before scaling. Mandating the full expert workflow for beginners creates cognitive overload, not productivity gains. The recommended approach is incremental adoption of each practice.

---

*End of Exam — 40 Questions*

**Answer Distribution Verification:**
- A: Q2, Q7, Q9, Q16, Q18, Q21, Q28, Q30, Q35, Q37 = 10 (25%)
- B: Q4, Q5, Q11, Q14, Q20, Q23, Q26, Q32, Q33, Q39 = 10 (25%)
- C: Q3, Q8, Q10, Q13, Q19, Q22, Q25, Q31, Q36, Q38 = 10 (25%)
- D: Q1, Q6, Q12, Q15, Q17, Q24, Q27, Q29, Q34, Q40 = 10 (25%)
