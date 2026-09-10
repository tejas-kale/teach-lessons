# Pi embedding for Teacher and Clarifier

Research for [issue #10](https://github.com/tejas-kale/teach-lessons/issues/10). Facts only; no product contract chosen.

**Package:** `@earendil-works/pi-coding-agent` v0.85.1 ([npm](https://www.npmjs.com/package/@earendil-works/pi-coding-agent), source: `packages/coding-agent` in [earendil-works/pi](https://github.com/earendil-works/pi)).

**Primary docs:** [SDK](https://pi.dev/docs/latest/sdk), [RPC](https://pi.dev/docs/latest/rpc), [Security](https://pi.dev/docs/latest/security), [Containerization](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/containerization.md).

---

## Feasibility

Cleararc can embed Pi for Teacher and Clarifier by running `@earendil-works/pi-coding-agent` on a **Node.js server** (requires Node `>=22.19.0`; [`package.json` engines](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/package.json)). The package is not browser-runnable: it depends on `node:fs`, `cross-spawn`, shell execution, and filesystem-backed credential storage ([SDK installation](https://pi.dev/docs/latest/sdk), [`index.d.ts` exports](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/index.ts)).

The SDK docs recommend in-process `createAgentSession()` for Node/TypeScript integrations; RPC/subprocess is an alternative for isolation or non-Node clients ([SDK — RPC Mode Alternative](https://pi.dev/docs/latest/sdk#rpc-mode-alternative), [RPC intro](https://pi.dev/docs/latest/rpc)).

---

## `createAgentSession()`

Main factory for one `AgentSession` ([SDK — createAgentSession](https://pi.dev/docs/latest/sdk#createagentsession)).

```typescript
import { createAgentSession, ModelRuntime, SessionManager } from "@earendil-works/pi-coding-agent";

const modelRuntime = await ModelRuntime.create();
const { session, extensionsResult, modelFallbackMessage } = await createAgentSession({
  cwd: "/path/to/workspace",
  agentDir: "/custom/agent",           // optional; default ~/.pi/agent
  modelRuntime,
  model,                                 // optional; restored from session or settings
  thinkingLevel: "medium",
  tools: ["read", "bash", "edit", "write"],
  excludeTools: ["ask_question"],
  customTools: [myTool],
  resourceLoader,                        // optional; default DefaultResourceLoader
  sessionManager: SessionManager.inMemory(),
  settingsManager,
});
```

**Return value:** `{ session, extensionsResult, modelFallbackMessage? }` ([SDK — Return Value](https://pi.dev/docs/latest/sdk#return-value)).

**Key options** (from [`CreateAgentSessionOptions`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/sdk.ts)):

| Option | Role |
|--------|------|
| `cwd` | Working directory for tool path resolution and default resource discovery |
| `agentDir` | Global config dir (default `~/.pi/agent` via `getAgentDir()`) |
| `modelRuntime` | Auth + model catalogue; defaults to `{authPath, modelsPath}` under `agentDir` |
| `resourceLoader` | Extensions, skills, prompts, themes, context files, system prompt |
| `sessionManager` | Persistence (file JSONL or in-memory) |
| `settingsManager` | Compaction, retry, transport, default model/tools |
| `tools` / `excludeTools` / `noTools` | Built-in tool allow/deny lists |

If `resourceLoader` is omitted, `DefaultResourceLoader` is constructed and `reload()` is called ([`sdk.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/sdk.ts)).

For session replacement (`newSession`, `switchSession`, `fork`), use `createAgentSessionRuntime()` / `AgentSessionRuntime` ([SDK — AgentSessionRuntime](https://pi.dev/docs/latest/sdk#createagentsessionruntime-and-agentsessionruntime)).

---

## Teacher session: modified teach skill

Skills are loaded by `ResourceLoader`, not passed directly to `createAgentSession()`.

**Injection paths:**

1. **`DefaultResourceLoader` + `skillsOverride`** — merge or replace discovered skills after discovery ([SDK — Skills](https://pi.dev/docs/latest/sdk#skills), [`DefaultResourceLoaderOptions.skillsOverride`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/resource-loader.ts)).
2. **`additionalSkillPaths`** — extra directories/files on the loader ([`DefaultResourceLoaderOptions`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/resource-loader.ts)).
3. **Filesystem placement** — put `SKILL.md` under discovered skill directories (see Skills discovery below).
4. **`Skill` object** — `{ name, description, filePath, baseDir, source }` with `filePath` pointing at the teach skill markdown ([SDK example](https://pi.dev/docs/latest/sdk#skills)).

Skills with `disableModelInvocation: true` are excluded from the system prompt and only invokable via `/skill:name` ([`formatSkillsForPrompt`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/skills.ts)).

**System prompt:** override via `systemPromptOverride` or `appendSystemPromptOverride` on `DefaultResourceLoader` ([SDK — System Prompt](https://pi.dev/docs/latest/sdk#system-prompt)).

**Teacher tools:** default built-ins are `read`, `bash`, `edit`, `write`; allowlist via `tools` ([SDK — Tools](https://pi.dev/docs/latest/sdk#tools)). Custom tools via `defineTool()` + `customTools` ([SDK — Custom Tools](https://pi.dev/docs/latest/sdk#custom-tools)).

---

## Clarifier session: Lesson as context

Pi has no first-class “Lesson” type. Lesson content maps to **context files** and/or **append system prompt** on a per-session `ResourceLoader`.

**Mechanisms:**

| Mechanism | API | Effect |
|-----------|-----|--------|
| Context files | `agentsFilesOverride` on `DefaultResourceLoader` | Injects `{ path, content }` entries merged into agent context ([SDK — Context Files](https://pi.dev/docs/latest/sdk#context-files), [`loadProjectContextFiles`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/resource-loader.ts)) |
| Append system prompt | `appendSystemPromptOverride` | Extra system instructions per session |
| Virtual paths | `agentsFilesOverride` accepts arbitrary `path` strings | Allows Lesson-scoped labels without real files |
| Custom message entries | `SessionManager` / extensions | `custom_message` entries participate in LLM context ([`CustomMessageEntry`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/session-manager.ts)) |

Default context discovery walks from `cwd` upward for `AGENTS.override.md`, `AGENTS.md`, `CLAUDE.md`, plus global `agentDir` ([`loadProjectContextFiles`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/resource-loader.ts)). Context files load **regardless of project trust** ([Security — Project Trust](https://pi.dev/docs/latest/security#project-trust)).

For Clarifier, a dedicated `DefaultResourceLoader` per Lesson (with `agentsFilesOverride` supplying Lesson HTML/Markdown) plus a restricted `tools` list (e.g. read-only or `noTools: "all"`) is the straightforward SDK pattern.

---

## Skills discovery paths

When using `DefaultResourceLoader`, skills come from `DefaultPackageManager.resolve()` plus `loadSkills()` ([`resource-loader.ts` reload](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/resource-loader.ts), [`skills.ts` loadSkills](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/skills.ts)).

**Default locations** ([SDK — Directories](https://pi.dev/docs/latest/sdk#directories), [`package-manager.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/package-manager.ts)):

| Scope | Path | Notes |
|-------|------|-------|
| User (Pi) | `~/.pi/agent/skills/` | Always loaded |
| User (agents) | `~/.agents/skills/` | Always loaded; separate `baseDir` |
| Project (Pi) | `<cwd>/.pi/skills/` | Requires **project trust** |
| Project (agents) | `<cwd>/.agents/skills/` and **ancestor** `.agents/skills/` up to git repo root (or filesystem root) | Ancestor walk via `collectAncestorAgentsSkillDirs`; project `.agents` requires trust |
| CLI | `--skill` paths | Temporary |
| Settings | `skills` array in `settings.json` | Package/git/npm skill sources |
| Extensions | `extendResources({ skillPaths })` | Runtime extension skill paths |

**Discovery rules** ([`loadSkillsFromDir`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/skills.ts)):

- Directory containing `SKILL.md` → one skill; no further recursion into that directory.
- Otherwise recurse subdirectories for `SKILL.md`; root may also contain direct `.md` files (non-`SKILL.md`) if they have frontmatter `description`.
- Respects `.gitignore` / `.ignore` / `.fdignore`; skips `node_modules`.
- Name collisions: first wins; later paths produce `collision` diagnostics.

**Skills in system prompt:** `formatSkillsForPrompt()` emits XML `<available_skills>` listing name, description, and filesystem `location` ([`skills.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/skills.ts)). Model is instructed to `read` the skill file when relevant.

When a custom `ResourceLoader` is supplied, `cwd` and `agentDir` **no longer control resource discovery** (they still affect session naming and tool paths) ([SDK — Directories](https://pi.dev/docs/latest/sdk#directories)).

---

## `ResourceLoader`

Interface ([`ResourceLoader`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/resource-loader.ts)):

- `getExtensions()`, `getSkills()`, `getPrompts()`, `getThemes()`, `getAgentsFiles()`
- `getSystemPrompt()`, `getAppendSystemPrompt()`
- `extendResources(paths)` — add extension-provided skill/prompt/theme paths without full reload
- `reload(options?)` — re-discover everything

`DefaultResourceLoader` is the standard implementation. Override hooks: `skillsOverride`, `agentsFilesOverride`, `systemPromptOverride`, `appendSystemPromptOverride`, `extensionsOverride`, etc.

Call `await loader.reload()` before `createAgentSession({ resourceLoader: loader })`.

---

## Auth (`~/.pi/agent/auth.json`)

**Default path:** `join(getAgentDir(), "auth.json")` where `getAgentDir()` → `~/.pi/agent` unless `PI_CODING_AGENT_DIR` is set ([`config.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/config.ts), [`getAuthPath`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/config.ts)).

**Storage:** `AuthStorage` implements `CredentialStore`; file-backed JSON with locking ([`auth-storage.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/auth-storage.ts)).

**Resolution priority** ([SDK — API Keys and OAuth](https://pi.dev/docs/latest/sdk#api-keys-and-oauth)):

1. Runtime overrides (`modelRuntime.setRuntimeApiKey()` — not persisted)
2. Stored credentials in `auth.json` (API keys or OAuth tokens)
3. Environment variables (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, etc.)
4. Fallback resolver (custom provider keys from `models.json`)

**Custom locations:**

```typescript
const modelRuntime = await ModelRuntime.create({
  authPath: "/my/app/auth.json",
  modelsPath: "/my/app/models.json",
});
// Or inject InMemoryCredentialStore / any CredentialStore
```

**Server implication:** provider credentials must not reach the browser. Auth file, env vars, and `ModelRuntime` belong on the server. Runtime API keys suit per-request overrides without disk writes.

---

## `ModelRuntime`

Canonical auth + models facade ([`model-runtime.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/model-runtime.ts), [SDK — Model](https://pi.dev/docs/latest/sdk#model)).

```typescript
const modelRuntime = await ModelRuntime.create({
  allowModelNetwork: true,       // opt-in network refresh at create
  modelRefreshTimeoutMs: 15_000,
  authPath, modelsPath,
  credentials: customStore,      // optional
});

const available = await modelRuntime.getAvailable();  // models with valid auth
const model = modelRuntime.getModel("anthropic", "claude-opus-4-5");
await modelRuntime.setRuntimeApiKey("anthropic", "sk-...");
await modelRuntime.login(providerId, ...);
```

- Model catalogue cached in `models-store.json` (default beside `models.json`) ([SDK — Model](https://pi.dev/docs/latest/sdk#model)).
- `PI_OFFLINE` disables model network access ([SDK — Model](https://pi.dev/docs/latest/sdk#model)).
- `createAgentSession` uses `modelRuntime.streamSimple()` for LLM calls ([`sdk.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/sdk.ts)).

**Model selection in session:** explicit `model` option → restore from session → settings default → first available ([`sdk.ts` findInitialModel path](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/sdk.ts)).

---

## Streaming events to a web client

Pi does not ship a browser WebSocket server. Streaming is **in-process** via `session.subscribe(listener)`.

**Event types** ([SDK — Events](https://pi.dev/docs/latest/sdk#events), [`AgentSessionEvent`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/agent-session.ts)):

| Event | Use for UI |
|-------|------------|
| `message_update` + `text_delta` | Token streaming |
| `message_update` + `thinking_delta` | Thinking stream (if enabled) |
| `tool_execution_start` / `_update` / `_end` | Tool progress |
| `message_start` / `message_end` | Message boundaries |
| `agent_start` / `agent_end` | Turn lifecycle |
| `turn_start` / `turn_end` | LLM response + tool results |
| `queue_update` | Steering/follow-up queue |
| `compaction_*`, `auto_retry_*` | Background operations |
| `bash_execution_update` | Shell output streaming |

**Browser pattern:** server holds `AgentSession`; on each event, serialise and push over SSE/WebSocket. RPC mode emits the same events as JSON lines on stdout ([RPC — Protocol Overview](https://pi.dev/docs/latest/rpc)).

**RPC alternative:** `RpcClient` spawns `pi --mode rpc` and exposes `onEvent()` ([`rpc-client.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/modes/rpc/rpc-client.ts)). JSONL framing rules apply ([RPC — Framing](https://pi.dev/docs/latest/rpc)).

**Prompt API:** `await session.prompt(text, { streamingBehavior?, images?, preflightResult? })` blocks until the accepted run finishes ([SDK — Prompting](https://pi.dev/docs/latest/sdk#prompting-and-message-queueing)). Use `session.subscribe` for incremental output during the await.

---

## Concurrent sessions

The SDK exposes **no global session pool**. Concurrency model:

- Each `createAgentSession()` returns an independent `AgentSession` with its own `Agent`, `SessionManager`, and subscriptions.
- **`ModelRuntime` may be shared** across sessions (single auth/catalogue instance).
- **`ResourceLoader` may be shared or per-session** (Teacher vs per-Lesson Clarifier loaders).
- Subscriptions attach to a specific `AgentSession`; after `AgentSessionRuntime.newSession()` / `switchSession()`, re-subscribe ([SDK — AgentSessionRuntime](https://pi.dev/docs/latest/sdk#createagentsessionruntime-and-agentsessionruntime)).
- Each session has `session.sessionId`, `session.isStreaming`, `session.abort()` ([SDK — AgentSession](https://pi.dev/docs/latest/sdk#agentsession)).

Cleararc would map Teacher and each active Clarifier to separate `AgentSession` instances (or RPC subprocesses) on the server, keyed by application session IDs.

---

## `cwd`, sandbox, and tools

**`cwd`:**

- Passed to `createAgentSession({ cwd })`; default `process.cwd()` ([`sdk.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/sdk.ts)).
- Built-in tools (`read`, `bash`, `edit`, `write`, `grep`, `find`, `ls`, `powershell`) are constructed for that `cwd` ([SDK — Tools with Custom cwd](https://pi.dev/docs/latest/sdk#tools-with-custom-cwd)).
- `SessionManager.inMemory(cwd)` / `SessionManager.create(cwd)` store `cwd` in session header ([`SessionHeader`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/session-manager.ts)).

**Built-in tools** ([SDK — Tools](https://pi.dev/docs/latest/sdk#tools)):

| Tool | Requires |
|------|----------|
| `read`, `edit`, `write` | Filesystem under `cwd` |
| `bash` / `powershell` | Host shell (`cross-spawn`) |
| `grep`, `find`, `ls` | Filesystem |

**No built-in sandbox** ([Security — No Built-in Sandbox](https://pi.dev/docs/latest/security#no-built-in-sandbox)): tools run with the Pi process’s OS permissions. Project trust only gates loading of project-local resources, not tool execution ([Security — Project Trust](https://pi.dev/docs/latest/security#project-trust)).

**Isolation options** ([Containerization](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/containerization.md)):

- Run whole Pi process in Docker/VM/sandbox
- Gondolin extension routes built-in tools into a micro-VM while Pi stays on host
- Credentials can stay on host while tools run isolated (Docker Sandboxes pattern)

For Cleararc localhost-first: Teacher likely needs a dedicated workspace `cwd` (Lesson authoring tree); Clarifier may use a read-only or empty `cwd` if Lesson content is injected via `agentsFilesOverride` rather than filesystem reads.

---

## What cannot run in the browser

| Component | Why server-only |
|-----------|-----------------|
| `@earendil-works/pi-coding-agent` package | Node `>=22.19.0`; `node:fs`, `cross-spawn`, shell tools ([`package.json`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/package.json)) |
| `ModelRuntime` / LLM streaming | Provider API keys, OAuth, `auth.json` ([SDK — API Keys](https://pi.dev/docs/latest/sdk#api-keys-and-oauth)) |
| Built-in tools | Filesystem and shell access ([Security](https://pi.dev/docs/latest/security)) |
| Extension loading | TypeScript modules via `jiti`; same process permissions ([Security](https://pi.dev/docs/latest/security)) |
| `DefaultResourceLoader` discovery | Filesystem scans of `~/.pi`, `.pi`, `.agents` |
| `InteractiveMode` TUI | Terminal UI (`@earendil-works/pi-tui`); not for browser embedding ([SDK — InteractiveMode](https://pi.dev/docs/latest/sdk#interactivemode)) |

**Browser role:** render streamed events and send prompts; no Pi package import.

**Export `./client`:** package exports `./client` with `source` only (not built in published `dist`) ([`package.json` exports](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/package.json)) — not a browser SDK for the coding agent.

---

## Session persistence

**Format:** append-only **JSONL** tree (`id` / `parentId`) ([SDK — Session Management](https://pi.dev/docs/latest/sdk#session-management), [`SessionManager`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/session-manager.ts)).

**Factories:**

| Factory | Persistence |
|---------|-------------|
| `SessionManager.create(cwd, sessionDir?)` | New file under default `~/.pi/agent/sessions/<encoded-cwd>/` |
| `SessionManager.continueRecent(cwd)` | Resume latest in that dir |
| `SessionManager.open(path)` | Open specific `.jsonl` |
| `SessionManager.inMemory(cwd?, options?, entries?)` | No filesystem; optional seed entries |
| `SessionManager.inMemory(cwd, { id }, entries)` | Restore from DB-held JSONL ([SDK](https://pi.dev/docs/latest/sdk#session-management)) |

**AgentSession** auto-persists via `SessionManager` on message events ([`agent-session.ts` header comment](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/agent-session.ts)).

**Env overrides:** `PI_CODING_AGENT_DIR`, `PI_CODING_AGENT_SESSION_DIR` ([`config.ts`](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/config.ts)).

Cleararc may store Teacher/Clarifier transcripts in SQLite by using `SessionManager.inMemory` + exporting entries, or by pointing `sessionDir` at app-controlled paths — Pi does not prescribe app DB schema.

---

## Integration summary for Cleararc roles

| Concern | Teacher | Clarifier |
|---------|---------|-----------|
| Entry | `createAgentSession({ cwd: authorWorkspace, ... })` | `createAgentSession({ cwd, resourceLoader: lessonLoader, ... })` |
| Teach skill | `skillsOverride` / filesystem / `additionalSkillPaths` | Typically no author skill; optional read-only skills |
| Lesson context | N/A (authoring) | `agentsFilesOverride` and/or `appendSystemPromptOverride` |
| Tools | Default or full coding set | Restricted (`tools: ["read"]` or `noTools: "all"`) |
| Auth | Shared server `ModelRuntime` | Same |
| Streaming | `session.subscribe` → SSE/WS | Same |
| Persistence | `SessionManager` + Cleararc Lesson store | Short-lived or persisted per Learner thread |

---

## Open facts (not decided here)

- Whether Teacher and Clarifier share one `ModelRuntime` or one process — SDK allows either.
- Whether Clarifier sessions are persisted across page reloads — Pi supports it; product choice pending ([issue #9](https://github.com/tejas-kale/teach-lessons/issues/9)).
- Sandbox strictness for localhost Teacher authoring — Pi default is full user permissions; container patterns exist if needed.

---

## References

- SDK: https://pi.dev/docs/latest/sdk
- RPC: https://pi.dev/docs/latest/rpc
- Security: https://pi.dev/docs/latest/security
- Containerization: https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/containerization.md
- Source: https://github.com/earendil-works/pi/tree/main/packages/coding-agent
- npm: https://www.npmjs.com/package/@earendil-works/pi-coding-agent
