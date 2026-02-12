# Exam: Claude Code Settings, Hooks, Plugins, Ralph Wiggum Loop & Best Practices (Lessons 14–18)

This exam covers Claude Code's settings hierarchy and precedence rules, event-driven hook automation, plugin discovery and distribution, the Ralph Wiggum Loop for autonomous iteration, and production best practices from the Claude Code creator's workflow. Select the single best answer for each question. All questions are self-contained.

---

### Section A: Settings Hierarchy & Precedence

**Q1.** A developer on a shared repository notices that Claude Code uses "Explanatory" output style even though she configured "Concise" in her user-level settings file at `~/.claude/settings.json`. Her teammate confirms that `.claude/settings.json` in the project root sets `outputStyle` to "Explanatory." No local settings file exists. Why does "Explanatory" override her preference?

- A) User-level settings only apply when no project directory is detected, reverting to defaults once Claude Code identifies a repository with its own configuration files present
- B) The `outputStyle` key is a special setting that can only be changed at project level, making user-level declarations of this particular key silently ignored by Claude Code
- C) Claude Code merges all settings alphabetically and "Explanatory" comes after "Concise," so the later alphabetical value takes precedence over earlier ones in conflicts
- D) Project-level settings override user-level settings in Claude Code's precedence hierarchy, so the team-shared `.claude/settings.json` takes priority over her personal `~/.claude/settings.json`

> **Answer: D** — Claude Code follows the precedence order Local > Project > User. Since no local settings exist, the project-level "Explanatory" overrides the user-level "Concise." This hierarchy allows team standards to take priority over individual preferences.

---

**Q2.** Claude Code's settings system uses three configuration levels. A team lead wants to enforce a security rule that blocks Claude from reading `.env` files across all team members working on a specific repository. Which file path and setting would accomplish this while still allowing individual developers to customize their personal preferences?

- A) Set `permissions.deny: ["Read(./.env)"]` in `.claude/settings.json` at the project root, which all team members share through version control while their personal `~/.claude/settings.json` remains untouched
- B) Set `permissions.deny: ["Read(./.env)"]` in each developer's `~/.claude/settings.json` file, ensuring every team member manually adds the same security rule to their personal global configuration
- C) Set `permissions.deny: ["Read(./.env)"]` in `.claude/settings.local.json` at the project root, which provides project-specific security enforcement that stays private to each machine
- D) Set `permissions.deny: ["Read(./.env)"]` in a special `.claude/security.json` file that Claude Code checks separately from the main settings hierarchy for permission enforcement

> **Answer: A** — Project-level settings (`.claude/settings.json`) are shared via version control and apply to everyone on the project. This is the correct level for team standards. User-level settings are personal; local settings are private and not shared; there is no separate security.json file.

---

**Q3.** A developer creates `.claude/settings.local.json` with `outputStyle: "Verbose"` to test a new workflow. The project's `.claude/settings.json` has `outputStyle: "Explanatory"` and her `~/.claude/settings.json` has `outputStyle: "Concise"`. After testing, she wants to revert to team standards. What is the simplest way to restore the project's "Explanatory" output style?

- A) Edit `.claude/settings.local.json` to explicitly set `outputStyle: "Explanatory"`, ensuring the local file matches the project-level value so both layers produce the same result
- B) Run a Claude Code reset command that clears all local overrides and rebuilds the settings hierarchy from the user and project levels only
- C) Delete `.claude/settings.local.json` entirely, which removes the local override and lets the project-level setting become the active value through normal precedence
- D) Edit `.claude/settings.json` at the project level to set a higher priority flag that forces it to override any local settings files present in the same directory

> **Answer: C** — Local settings exist for temporary experimentation. Deleting the local settings file removes the override, and precedence falls through to the project level ("Explanatory"). This is by design—local settings are meant to be disposable.

---

**Q4.** A team is setting up Claude Code for a new repository. They want team-wide coding standards shared through git, personal preferences that follow each developer across all their projects, and a space for safe experimentation that doesn't affect teammates. Which combination correctly maps these needs to settings levels?

- A) Team standards in `~/.claude/settings.json`, personal preferences in `.claude/settings.json`, experiments in `.claude/settings.local.json` — since global settings reach all collaborators
- B) Team standards in `.claude/settings.json` committed to git, personal preferences in `~/.claude/settings.json` on each developer's machine, experiments in `.claude/settings.local.json` which is gitignored
- C) Team standards in `.claude/settings.local.json` for enforcement, personal preferences in `.claude/settings.json`, experiments in `~/.claude/settings.json` since global scope allows broader testing
- D) Team standards in a dedicated `.claude/team-config.json`, personal preferences in `~/.claude/settings.json`, experiments in `.claude/settings.local.json` using Claude Code's extended config format

> **Answer: B** — The three-level hierarchy maps directly: project settings (`.claude/settings.json`) for team standards shared via git, user settings (`~/.claude/settings.json`) for personal preferences across all projects, and local settings (`.claude/settings.local.json`) for private experimentation that is gitignored.

---

**Q5.** A developer accidentally commits `.claude/settings.local.json` to the repository. A colleague pulls the change and notices unexpected behavior in their Claude Code sessions. Why is committing this file problematic, and what is the recommended practice?

- A) The local settings file contains encrypted tokens that become invalid on other machines, causing Claude Code authentication failures when colleagues attempt to use the decrypted credentials
- B) Local settings are meant for private per-machine experimentation, so committing them imposes one developer's temporary overrides on the entire team, potentially overriding team standards unexpectedly
- C) Claude Code rejects repositories containing both `settings.json` and `settings.local.json` in version control, causing a configuration conflict error that blocks all team members from starting sessions
- D) The local settings file references absolute filesystem paths specific to one machine, causing path resolution errors for every other developer whose directory structure differs from the original author

> **Answer: B** — `.claude/settings.local.json` is designed for private, temporary, per-machine overrides. Committing it to git exposes personal experimental settings to the whole team and can override team standards. The recommended practice is to add it to `.gitignore`.

---

**Q6.** A developer is confused about the `.claude/` directory in their project root. They consider deleting it to clean up what they perceive as unnecessary configuration files. What would happen if they delete this directory?

- A) Claude Code would automatically regenerate the directory with factory defaults on the next session start, causing only a brief interruption while configuration files are rebuilt from templates
- B) Claude Code would fall back to using only user-level settings from `~/.claude/settings.json`, which preserves personal preferences but any project-specific customizations would need to be manually recreated
- C) The deletion would corrupt Claude Code's internal state, requiring a complete reinstallation of the tool before it can be used again in any project on that machine
- D) All project-level settings, local overrides, and other Claude Code project configuration would be lost, resetting the project to default behavior and requiring manual reconfiguration of team standards

> **Answer: D** — The `.claude/` directory stores project-level settings, local settings, and other configuration Claude Code needs. Deleting it resets all project configuration to defaults. It should be treated like `.gitignore` or `package.json`—essential project infrastructure.

---

**Q7.** In Claude Code's settings hierarchy, the principle "more specific wins" governs how conflicting settings are resolved. A knowledge worker wants to understand how this principle applies beyond Claude Code. Which analogy most accurately captures the Local > Project > User precedence model?

- A) CSS specificity rules, where inline styles override class selectors which override element selectors, with each level providing increasingly targeted overrides for the same properties
- B) Database normalization, where data is decomposed into smaller tables to eliminate redundancy, with foreign keys linking related records across different levels of abstraction
- C) Load balancing algorithms, where traffic is distributed evenly across servers to prevent any single machine from becoming a bottleneck under high concurrent request volumes
- D) Version control branching, where feature branches diverge from a main branch and eventually merge back, creating temporary parallel codelines that resolve when the work is complete

> **Answer: A** — CSS specificity is the closest analogy: inline styles (local) override class selectors (project) which override element selectors (user). Each level provides increasingly targeted overrides, just as local settings override project settings which override user settings.

---

### Section B: Hooks — Event-Driven Automation

**Q8.** A developer wants to guarantee that Prettier runs on every JavaScript file Claude edits, rather than relying on Claude to remember formatting instructions. She needs automation that fires after file modifications. Which hook event and matcher combination should she configure?

- A) `PreToolUse` with matcher `"Write|Edit"`, which intercepts file operations before they execute and runs Prettier on the file content that Claude is about to write to disk
- B) `SessionStart` with matcher `"Prettier"`, which loads the Prettier configuration at the beginning of each session and applies formatting rules to all subsequent file operations automatically
- C) `PostToolUse` with matcher `"Write|Edit"`, which runs Prettier after Claude completes a file write or edit operation, ensuring every modification gets formatted regardless of Claude's behavior
- D) `UserPromptSubmit` with no matcher, which injects a formatting reminder into every prompt the developer submits, prepending Prettier instructions to Claude's context before processing begins

> **Answer: C** — `PostToolUse` fires after a tool completes, and the matcher `"Write|Edit"` targets file modification tools. This guarantees formatting runs after every edit, turning a suggestion into automated enforcement.

---

**Q9.** Claude Code hooks receive input and produce output through specific mechanisms. A developer is debugging a hook that isn't processing data correctly. Which description accurately captures how hooks receive and transmit information during execution?

- A) Hooks receive JSON data via stdin containing event details like session ID, tool name, and tool input, and they produce output via stdout that gets injected into Claude's context
- B) Hooks receive data through command-line arguments passed by Claude Code, with each event field mapped to a named flag, and they return results by writing to a designated output file
- C) Hooks receive data through environment variables set by Claude Code before execution, with one variable per event field, and they communicate results through their process return code only
- D) Hooks receive data through a local HTTP endpoint that Claude Code exposes on a random port, and they respond by sending a JSON payload back to that same endpoint address

> **Answer: A** — Hooks receive JSON via stdin with fields like `session_id`, `cwd`, `hook_event_name`, `tool_name`, and `tool_input`. Output goes to stdout and is injected into Claude's context. Exit codes control flow: 0 = success, 2 = block action.

---

**Q10.** A developer configures a `PreToolUse` hook with exit code behavior. She wants the hook to block dangerous bash commands containing "rm -rf" while allowing all other commands to proceed normally. Which exit code strategy correctly implements this blocking behavior?

- A) Exit with code 1 to signal a warning, which Claude Code interprets as a soft block that logs the attempt but ultimately allows the dangerous command to execute after recording it
- B) Exit with code 0 and print a warning message to stdout, which Claude Code displays to the user as informational context but does not prevent the command from running
- C) Exit with code 2 and print an error message to stderr, which Claude Code interprets as a hard block that prevents the tool from executing and shows the error to the user
- D) Exit with code 255 to trigger Claude Code's emergency shutdown protocol, which terminates the entire session immediately and prevents any further tool execution until restart

> **Answer: C** — Exit code 2 blocks the action and displays the error message. Exit code 0 means success (action proceeds), and other non-zero codes produce non-blocking warnings. There is no emergency shutdown protocol.

---

**Q11.** Claude Code supports five main hook events for automation. A developer needs to automatically load environment variables and display the current git branch whenever a new Claude Code session begins. Which hook event is designed for this initialization purpose?

- A) `PreToolUse` firing before the first tool executes in the session
- B) `SessionStart` firing when Claude Code initializes a new session
- C) `UserPromptSubmit` firing when the first prompt is entered
- D) `PostToolUse` firing after Claude Code's internal startup tools complete

> **Answer: B** — `SessionStart` fires when Claude Code starts and is specifically designed for initialization tasks like loading environment variables, showing project info, and setting up the session context.

---

**Q12.** A team has multiple hooks configured for the same `PreToolUse` event: one validates Bash commands and another checks file operations before writes and edits. They use different matchers to route to different scripts. How does Claude Code determine which hook script runs when a specific tool is invoked?

- A) Claude Code runs all `PreToolUse` hooks in parallel regardless of matchers, combining their outputs into a single response that merges validation results from every configured hook script
- B) Claude Code runs only the first matching hook and skips all subsequent hooks for the same event, using a first-match-wins strategy similar to firewall rule evaluation
- C) Claude Code presents the user with a selection menu showing all matching hooks, allowing the developer to choose which validation script should execute for the current tool invocation
- D) Claude Code evaluates each hook's matcher pattern against the tool being used, running only the hooks whose matcher matches the current tool name while skipping hooks with non-matching matchers

> **Answer: D** — Different matchers trigger different scripts based on which tool is used. A matcher of `"Bash"` only fires for Bash tool use, while `"Write|Edit"` fires for file modifications. Non-matching hooks are skipped.

---

**Q13.** A developer's hook script isn't executing when expected. She has verified the settings.json configuration is correct and the event is firing. Which debugging step should she try first to isolate whether the problem is in the script itself or the hook system?

- A) Add logging statements throughout the Claude Code source code to trace the hook execution pipeline and identify where the invocation chain breaks before reaching her script
- B) Restart Claude Code with the `--reset-hooks` flag, which clears all hook caches and forces a fresh registration of every configured hook from the settings files
- C) Test the script manually by piping sample JSON input through it on the command line, such as `echo '{"test": "data"}' | bash .claude/hooks/your-script.sh`, to verify the script runs correctly in isolation
- D) Delete all other hooks from the configuration file to eliminate potential conflicts, then add them back one at a time to identify which hook is interfering with the target script

> **Answer: C** — Manual testing with piped JSON input isolates whether the issue is in the script or the hook system. This is the recommended first debugging step. Other steps include checking file permissions (`chmod +x`) and using `claude --debug`.

---

**Q14.** Hooks and skills both extend Claude Code's behavior, but they serve fundamentally different purposes. A developer is deciding whether to create a hook or a skill for a new requirement. What is the core distinction between what hooks provide versus what skills provide?

- A) Hooks run before Claude processes a request while skills run after, creating a pipeline where hooks prepare inputs and skills handle the actual execution of tasks
- B) Hooks add automation to existing workflows by running commands on specific events, while skills add new capabilities that Claude can discover and invoke for tasks it couldn't otherwise perform
- C) Hooks are written in bash scripts while skills are written in markdown, and the language difference determines whether the extension modifies behavior or adds knowledge to Claude's context
- D) Hooks are limited to logging and monitoring functions while skills can modify files and execute commands, giving skills a broader range of actions within the Claude Code environment

> **Answer: B** — Hooks add automation (guaranteeing actions happen on events), while skills add new capabilities (giving Claude abilities it doesn't have natively). They're complementary: hooks automate behavior, skills extend what Claude can do.

---

**Q15.** A developer wants to track every prompt submitted during Claude Code sessions for later analysis. She needs to log each prompt with a timestamp to a JSON file. Which hook event should she use, and what JSON field contains the user's message?

- A) `SessionStart` event with the `initial_prompt` field, which captures the first message sent when the session begins and includes any auto-injected context from CLAUDE.md
- B) `PreToolUse` event with the `tool_input.prompt` field, which captures user messages that are routed through Claude's internal tool dispatch system before processing begins
- C) `PostToolUse` event with the `response.user_message` field, which captures the original prompt after Claude has processed it and includes both the input and Claude's interpretation
- D) `UserPromptSubmit` event with the `prompt` field in the JSON input, which fires every time the user submits a message and contains the raw text of what they typed

> **Answer: D** — `UserPromptSubmit` fires when a user submits a prompt. The JSON input contains a `prompt` field with the user's message. This is the correct event for tracking all user prompts, as demonstrated in the book's track-prompt.sh example.

---

### Section C: Plugins — Discovery, Installation & Distribution

**Q16.** A developer runs the `/plugin` command in Claude Code and sees categories including "Code intelligence," "External integrations," and "Development workflows." She notices the official Anthropic marketplace is already available without any setup. What is the relationship between a plugin and a marketplace in Claude Code's ecosystem?

- A) A plugin is a folder bundling skills, agents, hooks, commands, and MCP configurations into one installable package, while a marketplace is a catalog listing multiple plugins for discovery and installation — analogous to apps and an app store
- B) A plugin is a single skill file with enhanced metadata, while a marketplace is a curated collection of skill files organized by category that Claude Code downloads and indexes on first launch
- C) A plugin is a runtime extension that modifies Claude Code's core behavior, while a marketplace is a version-controlled registry that tracks plugin compatibility across different Claude Code releases
- D) A plugin is a configuration template that generates settings files, while a marketplace is a cloud service that hosts these templates and pushes updates automatically to all connected Claude Code installations

> **Answer: A** — A plugin bundles multiple components (skills, agents, hooks, commands, MCP) into one installable package. A marketplace is a catalog listing multiple plugins. The analogy is apps (plugins) and app store (marketplace).

---

**Q17.** A developer installs the `typescript-lsp` plugin for code intelligence but gets an "Executable not found" error. She has Claude Code and the plugin installed correctly. What additional requirement must be met for LSP-based code intelligence plugins to function?

- A) The TypeScript project must contain a `tsconfig.json` file with LSP-specific compiler options enabled, because the plugin reads configuration directly from the project's TypeScript setup
- B) Claude Code must be running in a VS Code integrated terminal, because LSP plugins require the editor's built-in language server infrastructure to handle protocol communication
- C) The plugin requires a minimum Claude Code version that includes the LSP protocol adapter, which was only added in the most recent major release of the tool
- D) The corresponding language server binary must be installed on the developer's system — for `typescript-lsp`, this means `typescript-language-server` must be available in the system PATH

> **Answer: D** — LSP plugins require the language server binary installed on the system. For `typescript-lsp`, the required binary is `typescript-language-server`. Other LSP plugins similarly require their respective binaries (e.g., `pyright-langserver` for Python).

---

**Q18.** When installing a plugin, Claude Code offers three scope options: User, Project, and Local. A team lead wants to ensure every developer on the repository has access to a PR review toolkit plugin without requiring individual installation. Which scope should she choose, and where does the configuration get stored?

- A) Project scope, stored in `.claude/settings.json`, which is committed to the repository and automatically provides the plugin to everyone who clones or pulls the project
- B) User scope, stored in `~/.claude/`, which requires each developer to install independently but ensures the plugin is available across all their projects on their machine
- C) Local scope, stored in local settings, which provides the plugin to all developers working on the same machine but not across different developer workstations
- D) Global scope, stored in Claude Code's installation directory, which makes the plugin available system-wide to all users and all projects on the machine simultaneously

> **Answer: A** — Project scope stores the plugin configuration in `.claude/settings.json`, which is committed to git. Everyone on the repository gets access automatically. User scope is per-developer, and there is no "Global scope" option.

---

**Q19.** A developer has built custom skills, hooks, and MCP configurations throughout the chapter. She wants to package them as a plugin for her team. What is the minimum required file structure for a valid Claude Code plugin?

- A) A folder containing only a `plugin.yaml` file at the root level with component paths, version constraints, and dependency declarations for each bundled capability
- B) A folder containing a `README.md` and a `manifest.json` at the root level, with all skills, agents, and hooks nested inside a `.claude/` subdirectory following the standard project layout
- C) A folder with a `.claude-plugin/` subdirectory containing a `plugin.json` manifest file, with component directories (skills, agents, hooks) at the root level of the plugin folder
- D) A folder containing a `package.json` with a `claude-plugin` field and standard npm package structure, allowing distribution through both npm and Claude Code's marketplace system

> **Answer: C** — A plugin requires a `.claude-plugin/` subdirectory containing `plugin.json` (the manifest with name, description, version, author). Components like skills, agents, and hooks go at the root level, not inside `.claude-plugin/`.

---

**Q20.** A developer wants to share her plugin with teammates who work across different Git hosting platforms. Some use GitHub, others use GitLab, and some work offline. Which set of distribution methods does Claude Code's marketplace system support?

- A) Only GitHub repositories using the `/plugin marketplace add owner/repo` syntax, with no support for alternative git hosts or local development environments
- B) GitHub repositories via `/plugin marketplace add owner/repo`, GitLab or other git hosts via full URL, and local directories via `./path` — supporting all three distribution scenarios
- C) A centralized Anthropic-hosted registry that mirrors plugins from any git host, requiring developers to submit plugins through a web portal for inclusion in the global catalog
- D) Only the official Anthropic marketplace with no ability to add custom or third-party marketplaces, ensuring all plugins are verified and reviewed before distribution to users

> **Answer: B** — Claude Code supports multiple distribution methods: GitHub (`/plugin marketplace add owner/repo`), GitLab or other git hosts (full URL), and local directories (`./path`). This covers all three scenarios the developer faces.

---

**Q21.** A developer discovers that a plugin she installed is causing issues with her workflow. She wants to temporarily stop using it without losing the configuration in case she needs it later. Which plugin management approach preserves the configuration while stopping the plugin's effects?

- A) Delete the plugin's files from the `.claude/` directory manually, which removes the active components but preserves a backup in Claude Code's internal plugin cache for later restoration
- B) Run `/plugin disable plugin-name@marketplace-name`, which deactivates the plugin without removing its configuration, allowing re-enablement later with `/plugin enable`
- C) Edit `.claude/settings.json` to comment out the plugin's configuration block, which Claude Code interprets as a disabled state while preserving the original settings text
- D) Run `/plugin uninstall plugin-name@marketplace-name` followed by `/plugin cache plugin-name`, which removes the active plugin but stores a snapshot in a local cache directory

> **Answer: B** — `/plugin disable` deactivates a plugin without removing it. `/plugin enable` re-enables it later. This preserves all configuration while stopping the plugin's effects, exactly what the developer needs.

---

**Q22.** A team decides to build versus install when evaluating how to add capabilities to their Claude Code setup. They need GitHub integration, a custom internal workflow specific to their company, and want to learn how plugins work. For each need, which approach does the material recommend?

- A) Install existing plugins for all three needs, since the marketplace likely contains plugins for GitHub integration, customizable workflow templates, and educational example plugins
- B) Build custom solutions for all three needs, since custom development provides the deepest understanding and most precise fit for every requirement regardless of marketplace availability
- C) Install existing for GitHub, build custom for all workflows, and ignore learning since plugin creation knowledge comes naturally from building custom solutions over time
- D) Install existing for GitHub, check marketplace first then build custom for the internal workflow, and install example plugins to study their structure for learning how plugins work

> **Answer: D** — The material recommends: install existing plugins for standard tasks (GitHub), check the marketplace first then build custom for team-specific workflows, and install examples to study structure when learning. The rule of thumb: check marketplace before building from scratch.

---

### Section D: Ralph Wiggum Loop — Autonomous Iteration

**Q23.** Ralph Wiggum Loop uses a specific Claude Code hook to enable autonomous iteration. When Claude attempts to stop after completing a batch of work, this hook intercepts the exit and decides whether to continue. Which hook event powers this mechanism, and what does it check?

- A) The Stop hook intercepts Claude's exit, checks whether the completion promise text appears in Claude's last output using exact string matching, and either allows the stop or reinjects a continuation prompt
- B) The PostToolUse hook monitors every tool execution for completion signals, accumulating evidence across multiple tool runs before deciding whether the overall task has reached its success criteria
- C) The SessionEnd hook fires when Claude Code's session timer expires, evaluating all accumulated outputs against a dynamic completion threshold that adapts based on iteration progress
- D) The PreToolUse hook intercepts the next planned tool execution, comparing it against a list of termination commands to decide whether Claude is attempting to finalize work prematurely

> **Answer: A** — Ralph Loop uses the Stop hook, which fires when Claude is about to exit. It checks if the `--completion-promise` text appears in Claude's output using exact string matching. If not found and iterations remain, it reinjects a prompt to continue.

---

**Q24.** A developer sets up a Ralph Loop with `--completion-promise "0 problems"` to fix all ESLint errors. After 5 iterations, she realizes a better completion signal would be "LINTING_COMPLETE" using the embedded promise pattern. Can she change the completion promise while the loop is running?

- A) Yes, by running `/ralph-loop --update-promise "LINTING_COMPLETE"` in a separate terminal window connected to the same Claude Code session, which hot-swaps the promise parameter
- B) Yes, by typing the new completion promise directly into the chat, which Claude Code detects as a runtime parameter update and applies to subsequent Stop hook evaluations automatically
- C) No, the `--completion-promise` parameter is static and set once at loop start — it cannot be added, modified, or given multiple conditions during runtime, which is why getting it right initially is critical
- D) No, but she can add a secondary completion condition by creating a `.claude/ralph-config.json` file that the Stop hook reads on each iteration to check for additional success markers

> **Answer: C** — The completion promise is static — set once at the initial `/ralph-loop` command and cannot be changed during runtime. There's no update mechanism, no multiple conditions, and no dynamic adaptation. This is why `--max-iterations` serves as the primary safety net.

---

**Q25.** A developer is considering using Ralph Loop for several tasks. Which task is the best fit for autonomous iteration based on the decision criteria for good Ralph Loop use cases?

- A) Upgrading a Next.js application from version 14 to 15 with 40+ expected breaking changes, where `npm run build` produces clear error messages and build success provides an objective completion signal
- B) Choosing the best color scheme and layout design for a new marketing landing page, where the team needs to evaluate visual aesthetics and brand alignment across multiple design options
- C) Writing a comprehensive technical blog post about the company's new API, where the content requires careful tone calibration, audience-appropriate examples, and executive review before publishing
- D) Deciding which three features from a backlog of twenty should be prioritized for the next sprint, where stakeholder preferences, resource constraints, and market timing all influence the decision

> **Answer: A** — Ralph Loop excels when success is objective, verifiable, and deterministic. A framework upgrade with clear build errors and an objective "build succeeds" signal is ideal. The other tasks require human judgment (aesthetics, tone, priorities), making them poor fits.

---

**Q26.** The material describes two approaches for defining completion promises: using natural tool output and using the embedded `<promise>` pattern. A developer is setting up a Ralph Loop for a complex refactoring task. Why does the material recommend the embedded promise pattern over relying on natural tool output?

- A) Natural tool output is typically too verbose for the Stop hook's parser, which has a character limit on the text it can scan, making shorter embedded markers more reliable for detection
- B) The embedded promise pattern runs faster because Claude Code can search for XML-tagged markers using optimized pattern matching instead of scanning the entire output for arbitrary text strings
- C) The embedded promise pattern gives the developer full control over the exact completion signal independent of tool output format, and Claude explicitly knows what to output when the task is complete
- D) Natural tool output requires Claude Code to maintain a database of known tool output patterns for each supported linter, compiler, and test runner, which is impractical to keep updated

> **Answer: C** — The embedded `<promise>` pattern is recommended because it gives full control over the completion signal, is independent of tool output format variations, ensures Claude knows exactly what to output when done, and works reliably with the static completion promise mechanism.

---

**Q27.** A developer starts a Ralph Loop for fixing TypeScript errors but notices after 8 iterations that Claude keeps hitting the same compilation error repeatedly without making progress. The error count hasn't decreased in the last 3 iterations. What should she do?

- A) Increase `--max-iterations` to give Claude more attempts, since complex TypeScript errors often require many iterations before the correct fix emerges from Claude's exploration of different approaches
- B) Cancel the loop with `/cancel-ralph`, investigate the stuck error manually, then restart with a more specific prompt that addresses the particular issue Claude is struggling to resolve
- C) Switch the completion promise to a less strict criterion so the loop can terminate successfully, accepting that some TypeScript errors may remain unfixed in the final output
- D) Add additional context to CLAUDE.md while the loop is running, which Claude will detect on the next iteration and use to inform a different approach to the problematic compilation error

> **Answer: B** — When the same error repeats 3+ times, Claude is stuck. The recommended action is to cancel with `/cancel-ralph`, investigate the issue, and restart with better context. Continuing wastes iterations and API credits without progress.

---

**Q28.** Ralph Wiggum Loop was deliberately designed as a plugin rather than a built-in Claude Code feature. A product manager asks why autonomous iteration isn't included by default. What is the primary reason for this architectural decision?

- A) Built-in features undergo a longer review and testing cycle at Anthropic, and Ralph Loop is still in beta testing, so it will be integrated into the core product once it reaches stable release status
- B) Plugin architecture allows Ralph Loop to receive updates independently from Claude Code's release cycle, enabling faster feature iteration without requiring users to upgrade their core installation
- C) Technical limitations in Claude Code's hook system prevent built-in features from intercepting the Stop event, so the plugin uses an external process that operates outside the normal hook pipeline
- D) Autonomous iteration carries cost and control risks — making it a plugin ensures users opt in deliberately rather than accidentally triggering long-running loops that consume significant API credits

> **Answer: D** — The material explicitly states that autonomous iteration carries cost and control risks. Making it a plugin ensures users opt in deliberately, not accidentally. This prevents unexpected API spend from unintended autonomous loops.

---

**Q29.** A developer wants to use Ralph Loop for a large codebase migration. She estimates 50+ errors to fix and wants to manage costs effectively. The material provides guidelines for setting `--max-iterations` based on task complexity. Which approach to iteration limits does the material recommend?

- A) Set `--max-iterations` to exactly the number of known errors, since each iteration should resolve one error and the limit should match the expected work precisely
- B) Omit `--max-iterations` entirely and rely solely on the completion promise to stop the loop, since a well-crafted promise eliminates the need for an arbitrary numerical safety limit
- C) Start with a conservative limit like 20 iterations, evaluate progress if the limit is hit, then restart with a higher limit or break the task into smaller chunks based on what was learned
- D) Set `--max-iterations` to the maximum allowed value to ensure the loop never stops prematurely, since the completion promise provides sufficient protection against unnecessary continuation

> **Answer: C** — The material recommends starting conservative (20 iterations), then increasing if needed or breaking tasks into smaller loops. `--max-iterations` is the primary safety net since completion promises use fragile exact string matching. Never omit the limit.

---

**Q30.** The material distinguishes between three hidden costs of manual iteration that Ralph Loop eliminates. A developer spends 30 minutes copying linter output back to Claude across 10 iterations. Which set of costs does this manual process impose?

- A) Network latency from repeated API calls, token limit consumption from duplicate context, and model degradation from processing repetitive correction patterns across multiple sequential requests
- B) Waiting time while Claude processes each response, context switching that breaks the developer's flow between checking output and pasting errors, and error transcription from manually copying output
- C) Financial cost of repeated API calls, storage cost of maintaining conversation logs, and opportunity cost of not using Claude for higher-value tasks during the manual feedback loop period
- D) Cognitive load from tracking which errors are fixed, version control complexity from multiple incremental commits, and communication overhead from updating team members on progress

> **Answer: B** — The three hidden costs of manual iteration overhead are: (1) waiting time while Claude processes, (2) context switching that breaks flow as you check output and copy errors, (3) error transcription from manually copying output which can introduce typos and missed details.

---

**Q31.** A developer runs a Ralph Loop with `--completion-promise "All tests passing"` for a test-driven refactoring task. After 12 iterations, Claude outputs "All 47 tests passing. Refactoring complete." The Stop hook detects the completion promise. Why does the exact string matching approach succeed here despite appearing fragile?

- A) The Stop hook uses fuzzy matching that tolerates extra words around the completion promise, so "All tests passing" matches within "All 47 tests passing. Refactoring complete" through substring detection
- B) Claude Code preprocesses the output before the Stop hook evaluates it, stripping numbers and punctuation to normalize the text, which converts "All 47 tests passing" into "All tests passing" for exact comparison
- C) The embedded promise pattern wraps the completion text in XML tags that the Stop hook parses separately from the rest of the output, isolating the completion signal from surrounding context
- D) The Stop hook checks whether the completion promise text appears anywhere within Claude's output as a substring, so "All tests passing" is found within the longer output string through exact string matching

> **Answer: D** — The Stop hook checks if the completion promise text appears in Claude's output. "All tests passing" appears as a substring within "All 47 tests passing. Refactoring complete." This is exact string matching — the promise text must appear somewhere in the output.

---

**Q32.** Ralph Loop's documentation emphasizes that the loop excels when success is "objective, verifiable, and deterministic." A team evaluates four potential tasks. Which task violates ALL three of these criteria?

- A) Redesigning the application's user interface to feel more modern and appealing, where success depends on subjective aesthetic judgment, cannot be verified by automated tools, and varies based on individual taste
- B) Fixing all TypeScript compilation errors, where the compiler provides an objective error count, verification runs automatically, and the same code produces the same result deterministically
- C) Resolving all failing integration tests, where test runners report objective pass/fail results, verification is automated through the test suite, and tests produce deterministic outcomes given the same code
- D) Upgrading database migration scripts, where migration tools report objective success or failure, verification runs through schema comparison tools, and the same migration produces identical results

> **Answer: A** — UI redesign for "modern and appealing" feel is subjective (not objective), cannot be verified by automated tools (not verifiable), and varies by individual taste (not deterministic). All three criteria are violated, making it a poor Ralph Loop candidate.

---

### Section E: Best Practices — The Creator's Workflow

**Q33.** Boris Cherny, Claude Code's creator, describes a principle that unifies all Claude Code best practices. Understanding this principle explains why parallel sessions, subagents for investigation, Plan Mode, and `/clear` between tasks are all recommended. What is this unifying principle?

- A) API cost optimization, where every technique is designed to minimize the number of tokens consumed per task by reducing redundant processing and eliminating unnecessary model interactions
- B) Team coordination, where every technique ensures multiple developers can work simultaneously on the same codebase without creating merge conflicts or duplicating effort across sessions
- C) Model accuracy improvement, where every technique provides Claude with better training signal by structuring interactions in ways that reinforce correct behavior patterns over time
- D) Context window management, where every technique prevents the conversation context from filling up with irrelevant information, since Claude's performance degrades as context becomes cluttered

> **Answer: D** — The fundamental constraint is that Claude's context window fills up fast and performance degrades as it fills. Every best practice — parallel sessions, subagents, `/clear`, Plan Mode — traces back to managing this constraint.

---

**Q34.** The Claude-reviews-Claude pattern uses two separate Claude sessions for different roles. A developer wants to apply this technique to validate an implementation plan. She writes the plan in Session A. What specific advantage does having Session B review the plan with fresh context provide?

- A) Session B has no exposure to the exploration and reasoning that led to the plan, so it evaluates the plan on its own merits without the sunk-cost bias that comes from having invested time developing the approach
- B) Session B uses a different Claude model variant optimized for code review, which applies stricter evaluation criteria and catches architectural issues that the planning-optimized model in Session A might overlook
- C) Session B has access to a broader knowledge base because it hasn't consumed context tokens on the planning task, giving it more capacity to draw on training data for identifying potential issues
- D) Session B can run automated static analysis tools on the plan that Session A cannot access, providing objective metrics about code complexity, security vulnerabilities, and test coverage gaps

> **Answer: A** — Fresh context catches blind spots. Session B hasn't seen the exploration that led to the plan, so it evaluates without sunk-cost bias. A different "persona" surfaces different concerns, and two-pass verification happens before any code is written.

---

**Q35.** Boris's team uses a specific technique to make CLAUDE.md evolve over time. After Claude makes a mistake, they correct it and then invoke a specific practice. What is this practice, and why does the material claim Claude writes better rules for itself than humans typically write?

- A) They run a dedicated `/audit` skill that compares Claude's output against a rules database, generating new CLAUDE.md entries automatically by pattern-matching the error type to a template library of known corrections
- B) They copy the error and correction into a shared team wiki, which a nightly script processes and converts into CLAUDE.md-compatible rules using standardized formatting and categorization logic
- C) They tell Claude "Update your CLAUDE.md so you don't make that mistake again," which works because Claude understands the exact context of the error and knows which variations of the mistake to prevent
- D) They use a PostToolUse hook that automatically detects when Claude's output is corrected by the user, generating a diff-based rule that captures the before and after states of the correction

> **Answer: C** — The magic phrase is "Update your CLAUDE.md so you don't make that mistake again." Claude writes better rules because it understands the exact error context and knows which variations to prevent. Every correction becomes permanent institutional memory.

---

**Q36.** Boris uses Opus 4.5 with thinking for all his work despite it being larger and slower than Sonnet. He claims it's actually faster overall for task completion. A developer skeptical of this claim asks for the reasoning. What counterintuitive insight does Boris's model selection reveal?

- A) Opus 4.5 has a larger context window than Sonnet, which means fewer sessions are needed to complete complex tasks, reducing the total overhead of session management and context switching
- B) A "wrong fast answer" costs more total time than a "right slow answer" because Opus 4.5 requires less correction and iteration, making overall task completion faster despite slower per-response times
- C) Opus 4.5 processes tool calls in parallel while Sonnet processes them sequentially, so despite slower text generation the actual wall-clock time for tool-heavy workflows is shorter
- D) Opus 4.5 generates shorter, more precise responses that consume fewer context window tokens, extending session useful life and reducing the frequency of `/clear` commands needed during work

> **Answer: B** — The counterintuitive insight is that a "wrong fast answer" costs more time than a "right slow answer." Opus 4.5 requires less correction and steering, making total task completion faster despite slower individual responses.

---

**Q37.** The official Claude Code best practices describe several common failure patterns that waste developer time. A developer notices that she corrected Claude three times on the same import path issue, and Claude still gets it wrong on the fourth attempt. What does the material recommend?

- A) Add a more detailed CLAUDE.md rule about import paths with multiple examples, edge cases, and explicit file path mappings for every module in the project directory structure
- B) Run `/clear` and start a fresh session with a more specific initial prompt that incorporates what she learned from the failed attempts, since the context is cluttered with failed approaches
- C) Switch to a more capable model like Opus 4.5, which has better instruction-following capabilities and is less likely to repeat mistakes that have been explicitly corrected multiple times
- D) Create a PreToolUse hook that automatically validates import paths before any Write or Edit tool execution, intercepting incorrect patterns before they reach the codebase

> **Answer: B** — The correction spiral pattern: after 2+ failed corrections on the same issue, `/clear` and rewrite the initial prompt. The context is cluttered with failed approaches, making Claude more confused. A fresh start with a better prompt is faster than continued correction.

---

**Q38.** Boris recommends a specific heuristic for deciding when to create a new skill. A developer wonders whether her repetitive daily tasks justify skill creation or if she's over-engineering. What is Boris's heuristic, and what additional configuration does he recommend for skills that should only run when explicitly invoked?

- A) Create a skill for any task taking more than 5 minutes, and use `auto-invoke: false` in the skill's YAML frontmatter to prevent Claude from discovering and running it without explicit user request
- B) Create a skill for tasks performed weekly or more often, and use `restrict-invocation: manual` in the skill metadata to limit activation to slash command invocation by the user only
- C) Create a skill for tasks repeated more than three times total, and use `model-only: false` in the skill configuration to require user confirmation before Claude executes the skill autonomously
- D) Create a skill for any task done more than once a day, and use `disable-model-invocation: true` in the skill's YAML frontmatter to restrict it to manual invocation via slash command only

> **Answer: D** — Boris's heuristic: "If you do something more than once a day, turn it into a skill." Use `disable-model-invocation: true` to restrict skills to manual invocation only, preventing Claude from auto-triggering them.

---

**Q39.** Boris's team uses subagents strategically to keep the main conversation context clean. A developer needs to investigate how the authentication system handles token refresh across the codebase. Instead of having Claude read files directly in the main session, Boris recommends a different approach. What is this approach, and what principle does it serve?

- A) Use a subagent to investigate the authentication system, which explores in its own context window, reads relevant files, and reports back findings — keeping the main session's context uncluttered with file contents
- B) Use the `/search` command to find all authentication-related files first, then selectively read only the most relevant files to minimize the number of tokens consumed in the main context window
- C) Create a dedicated CLAUDE.md section documenting the authentication system, so Claude can reference the summary instead of reading source files during the main conversation about the feature
- D) Open a separate browser tab with the codebase documentation and paste relevant excerpts into the conversation, which gives Claude the needed context without triggering file read operations

> **Answer: A** — Subagents for investigation keep the main context clean. The subagent explores in its own context window, reads many files, and reports back a summary — all without cluttering the main conversation. This directly manages the fundamental context window constraint.

---

**Q40.** The material identifies a "meta-pattern" behind most Claude Code failure modes observed across many users. A developer experiences multiple issues: cluttered sessions from unrelated questions, failed correction attempts polluting context, and Claude reading too many files during open-ended investigation. What single underlying cause connects all these failures?

- A) Insufficient CLAUDE.md configuration that fails to provide Claude with enough project context, forcing it to compensate through excessive file reading, repeated attempts, and exploratory behavior
- B) Using a model that is too small for the task complexity, which causes comprehension failures that manifest as incorrect corrections, unfocused exploration, and inability to track conversation goals
- C) Context pollution from too much irrelevant information or failed approaches cluttering the conversation, which is why the recommended response to most failures is to start fresh with `/clear`
- D) Network latency causing incomplete responses that Claude then tries to compensate for by re-reading files and re-attempting corrections, creating a cascade of redundant operations in the session

> **Answer: C** — The meta-pattern: most failures stem from context pollution — either too much irrelevant information or failed approaches cluttering the conversation. The kitchen sink session, correction spiral, and infinite exploration patterns all trace back to this. When in doubt, `/clear` and start fresh.

---

## Answer Distribution Verification

```
Total questions: 40

A count: 10 (25%) — Q2, Q7, Q9, Q16, Q18, Q23, Q25, Q32, Q34, Q39
B count: 10 (25%) — Q4, Q5, Q11, Q14, Q20, Q21, Q27, Q30, Q36, Q37
C count: 10 (25%) — Q3, Q8, Q10, Q13, Q19, Q24, Q26, Q29, Q35, Q40
D count: 10 (25%) — Q1, Q6, Q12, Q15, Q17, Q22, Q28, Q31, Q33, Q38

Longest streak of same letter: 2 (within limits)
```

## Word Count Spot-Check

```
Q5:  A: 29w, B: 30w, C: 31w, D: 27w — correct: B, longest: C ✓
Q12: A: 29w, B: 30w, C: 28w, D: 30w — correct: D, longest: B (tie) ✓
Q23: A: 33w, B: 30w, C: 28w, D: 29w — correct: A, longest: A → within 20% of avg (30w), range 24-36 ✓
Q31: A: 33w, B: 30w, C: 28w, D: 32w — correct: D, longest: A ✓
Q38: A: 30w, B: 28w, C: 29w, D: 29w — correct: D, longest: A ✓
```
