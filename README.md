# Archimedes-Local-AI-Agent V1
Personal local AI-agent workstation built with Hermes, Qwen, NInfer, RAG, automations, mobile access, and reliability monitoring. Named Archimedes!


**Overview**: Archimedes is a personal 24/7, local-first AI agent workstation built to run capable agentic workflows without relying on hosted model APIs. Core inference and knowledge remain on-device, with persistent memory, document RAG, mobile access, productivity integrations, scheduled automations, and external reliability monitoring. This allows for greater data sovereignty and no per-token costs, effectively reducing my costs to nearly $0 (not counting electricity).

Powered by a locally hosted Qwen 3.8-27B model on an RTX 5090, orchestrated through Hermes Agent and designed for privacy, persistence, extensibility, and continuous operation.
 
 
**Capabilties**: Some system capabilities are, including but not limited to:
- Run a locally hosted 27B model with agentic tool access and no per-token model API dependency.
- Execute multi-step tasks involving reasoning, tool use, and iterative decision-making.
- Search persistent knowledge and local document collections using retrieval-augmented generation.
- Preserve reusable knowledge across independent and fresh agent sessions.
- Research current web information and combine it with local knowledge and tools.
- Create and manage selected Google Calendar and Tasks workflows through natural language.
- Accept text and voice requests remotely through Telegram and a private WebUI.
- Maintain multiple isolated agent sessions for independent conversations and workflows.
- Run scheduled autonomous tasks and deliver results directly to mobile devices.
- Generate and deliver a personalized daily Morning Brief using current information.
- Recover the core agent stack automatically following Windows reboots or power interruptions.
- Detect service outages, alert a phone without notification spam, and report subsequent recovery.
- Retrieve and send documents to your mobile device and/or email. 


**Simplified Architecture**
<img width="822" height="394" alt="image" src="https://github.com/user-attachments/assets/18b9c87f-fcd4-4461-9d68-8f8711e60804" />


-------------------------------------------------------------------------------------------------------

## Architecture Diagram (Extended)

"Archimedes" is a local-first agent system built around a locally hosted LLM and the Hermes Agent harness. Hermes provides the orchestration layer between user interfaces, model inference, retrieval systems, executable tools, external integrations, scheduled workflows, and operational monitoring.

The core reasoning stack runs locally on a Windows 11 workstation equipped with an RTX 5090. Qwen3.8-27B on xhigh provides reasoning and decision-making, NInfer serves the model, and Hermes manages context, sessions, tool invocation, and execution flow.

Supporting systems are intentionally modular. Docker provides an execution boundary for tools and automation, QMD and Obsidian provide persistent retrieval and knowledge storage, external APIs extend the agent into productivity services, and an independent Supervisor monitors critical services without participating in agent execution.

The diagram below represents system connectivity and directional data relationships, NOT the chronological lifecycle of an individual request.

<img width="1313" height="1170" alt="image" src="https://github.com/user-attachments/assets/95894849-4a40-4c91-8ae8-5674c272d7aa" />


**Component Responsibilities**

| Component | Role |
|---|---|
| **Hermes Agent** | Orchestration, sessions, tools, execution |
| **Qwen3.8-27B** | Reasoning and decision-making |
| **NInfer** | Local model serving |
| **RTX 5090** | Inference acceleration |
| **Docker** | Isolated tool execution |
| **QMD** | Indexed knowledge retrieval |
| **Obsidian** | Persistent knowledge store |
| **RAG Tool** | Document retrieval |
| **Document Storage** | Original source files |
| **Python / Transformations** | Parsing, extraction, automation |
| **Dedicated Chrome** | Browser automation |
| **External APIs** | Calendar, Tasks, other services |
| **Cron / Scheduled Jobs** | Scheduled agent workflows |
| **Supervisor** | Read-only health monitoring |
| **Pushover** | Mobile alerts and notifications |
| **GitHub Backup** | Versioned knowledge backup |


**Architectural Boundaries**:

Local reasoning boundary: Core LLM inference occurs locally, so prompts sent to Qwen are processed through NInfer on the workstation's RTX 5090 rather than a hosted model API.

Execution boundary: Reasoning and execution are intentionally separated. Qwen determines what actions are required, and Hermes coordinates those actions, with executable tools running through isolated environments such as Docker.

Knowledge boundary: Persistent knowledge and document retrieval are maintained outside conversational context. Obsidian stores durable knowledge while QMD provides indexed retrieval across that information and document collections so that model context remains lean.

External-service boundary: Cloud services are used selectively for connectivity and productivity functions—such as Telegram, Google services, Tailscale, Pushover and GitHub—rather than for the system's core model inference.

Monitoring boundary: Reliability monitoring operates independently of the agent reasoning loop. The Supervisor observes system health and emits alerts but does not autonomously repair or mutate monitored services.


## Request Lifecycle

Since Archimedes is as an agent system and not a standalone chat model, the core request loop functions with Qwen as the principal reasoning model, Hermes as the harness, and NInfer as the service running Qwen.

**Responsibilities in One Request**

| Component | Responsibility during a request |
|---|---|
| User interface | Accept a request and display the response through Desktop, Telegram, or the Web Dashboard. |
| Hermes | Select the session, assemble available context and tool definitions, invoke inference, route tool calls, and apply the configured execution controls. |
| NInfer | Accept the model request and run the local Qwen deployment. |
| Qwen | Interpret the request, propose tool calls when needed, interpret observations, and generate the response. |
| Tools and integrations | Perform the requested operation or query and return an observable result. |
| Notification plugin | Observe selected lifecycle events and launch an out-of-process notification sender. It does not decide or authorize the task. |


## Inference Stack

| Item | Recorded configuration or observation |
|---|---|
| Host | Windows 11 PC. |
| Accelerator | NVIDIA GeForce RTX 5090; approximately 32 GB-class VRAM, 1,792 GB/s memory bandwidth. |
| System memory / RAM | 64 GB. |
| Primary serving runtime | NInfer, running on the Windows host. |
| Primary model | Qwen3.8-27B on xhigh reasoning. |
| Model Quantization | Q4 NVFP4 NInfer. |
| Context capacity | 125,000. |
| Temperature | 1.0. | 
| Approximate Tok/sec | ~160 tok/s. |
| SSD Space | 4 TB. |
| GPU Power Limiting | 475 W, reapplied by a dedicated Windows startup task. |
| PowerShell Version | PowerShell 7. |


## Knowledge & RAG

Archimedes separates durable knowledge from imported source documents, and both are searchable through QMD, but have different ownership and update paths. The Obsidian vault is a human-readable knowledge base, and File RAG is an ingestion pipeline for original documents. The two should not be collapsed into one undifferentiated memory folder.

**Memory & Knowledge Layers**

| Layer | Purpose | How it changes |
|---|---|---|
| Conversation history | Context for a particular interaction or topic. | Appended during chat and managed by Hermes session behavior. |
| Persistent knowledge | Reusable notes, decisions, preferences, procedures, and system documentation. | Maintained in the Obsidian vault through deliberate knowledge writes. |
| Original documents | User-owned PDF, DOCX, XLSX, TXT, and Markdown source material. | Added, changed, moved, or removed by the user. |
| Extracted representations | Text and provenance suitable for indexing. | Generated by the File RAG extractor and scanner. |
| QMD index | Searchable lexical/vector representations of the selected Markdown collections. | Refreshed by scheduled indexing and embedding. |
*The indexing and generated extractions are done with the intent to maintain source documents safe and untouched in case of errors.

**Obsidian Knowledge Vault**

The vault uses a numbered, PARA-style arrangement with areas for inbox material, profile, people, tasks, decisions, systems, procedures, agent guidance, tracking, and archive. It was exposed to the Hermes sandbox as /obsidian and to the QMD container as /vault.

These mounts have different roles. The Hermes tool environment can use the approved vault path for edits and knowledge work, while the QMD mount is read-only so the indexer cannot rewrite the source notes through that mount. A Git repository and backup script preserve selected vault history and synchronize it to the configured GitHub backup repository. 

**QMD**

QMD runs as its own Docker container, built from a local image. It exposes an HTTP MCP service through a host loopback port, and Hermes uses the MCP endpoint rather than directly editing QMD's SQLite database.

QMD was configured for CPU-only operation in this version, with QMD_FORCE_CPU=1 and an 8 GiB container memory limit. Its own local embedding, query-expansion, and reranking models are separate from the primary Qwen reasoning model.

| Retrieval role | Recorded local model |
|---|---|
| Embeddings | `embeddinggemma-300M-Q8_0.gguf` |
| Query expansion | `qmd-query-expansion-1.7B-q4_k_m.gguf` |
| Reranking | `Qwen3-Reranker-0.6B-Q8_0.gguf` |

**Extraction Runtime and Supported Formats**

The extractor runs on the Windows host under a dedicated Python environment, and extraction libraries were installed into that environment rather than into Hermes's own agent environment. 

| Input | Implemented extraction behavior | Important limit |
|---|---|---|
| PDF | Embedded text, ordered page sections such as `Page 1`. | Scanned/image-only content is not automatically OCR'd. |
| DOCX | Paragraphs and tables in body order, with supported heading styles mapped to Markdown. | This is text extraction, not a guarantee of complete visual/layout fidelity. |
| XLSX | Worksheet sections, row/column context, small rectangular tables or deterministic row listings. | Formula text is retained; formulas are not recalculated. |
| TXT | UTF-8 text, with BOM tolerated. | Undecodable input fails rather than replacing characters. |
| Markdown | Original body preserved after generated provenance. | Embedded binary assets are not turned into searchable image content. |

**Scheduled Indexing and Embedding**

The existing QMD Knowledge Sync Windows task runs every five minutes. A five-minute interval describes when processing is offered, not a strict end-to-end freshness guarantee. File stability checks, extraction time, QMD work, login state, and unavailable dependencies can extend the delay.


## Tool Execution

**Skills, Tool Schemas, and Executable Implementations**

A Hermes skill is the procedural layer that tells the model when and how to use a capability, while a registered tool or wrapper is the executable interface. The implementation can then call a host service, an MCP endpoint, a CLI, or a remote API.

**Browser Automation**

For the project I installed a dedicated Chrome automation profile and configured CDP at a host loopback port. The purpose was to keep agent browsing separate from my personal Edge session and its account state, at fear of already saved passwords, and interfering with day-to-day usage. 

The agent is also instructed to conduct "cleanup" after usage of the browse as I found it to often open many tabs and leave them open, which can weigh on usage (especially using Chrome). 


## Automation

Archimedes uses two scheduling layers with different responsibilities. Hermes Cron schedules and tool-driven jobs by a specific trigger, while windows Task Scheduler runs infrastructure work in case of an update, refresh, or power outage.

**Scheduler Ownership**

| Scheduler | Accepted responsibility | Current examples |
|---|---|---|
| Hermes Cron | Run an agent prompt at a scheduled time and deliver its result. | Morning Brief. |
| Windows Task Scheduler | Maintain infrastructure and run scripts. | RAG synchronization, Supervisor checks, GPU power-limit application. |
| Windows user startup | Start selected user-session applications. | Hermes Desktop, Hermes Gateway launcher, Docker Desktop. |

**Fresh Execution Context and Delivery Context**

Each scheduled run uses a fresh agent session to avoid repeated compaction and quality degradation.  Interactive follow-up inside the topic can still build that topic's conversation history.

The custom long-turn completion plugin excludes cron sessions. A scheduled brief should arrive through its configured Telegram delivery, not through an additional generic Pushover message announcing that the cron turn finished. Reliability incidents remain a separate notification category.

**Approval and Source Boundaries**

A scheduled job's need for outside information does not authorize unrelated state changes. For example, the Morning Brief prompt is a research-and-deliver workflow. It has no implemented Gmail mutation, Calendar mutation, production deployment, or system-repair step.


## Reliability
The custom Supervisor is an external monitor. It does not ask Qwen to decide whether services are healthy and does not participate in normal agent requests. Its permanent operating rule is monitor and notify, but do not remediate monitored components. This decision was made in order to avoid potential loops when attempting to restart or fix issues.

**Critical Checks and What They Look For**

| Component | Recorded check | 
|---|---|---|
| NInfer | Local port "____" accepts a connection. | 
| Docker | Docker CLI responds with server information. |
| QMD | Container reports running and host port "_____" is reachable. | 
| Hermes | Matching agent processes are present. | 
| GPU limit | `nvidia-smi` reports the expected 475 W limit. |
| RAG task | Scheduled-task state and last-result handling indicate availability/success. |

**Debounce and Incident State**

The state file stores last_status, consecutive_failures, alert_active, and last_change_utc for each critical component. The intended and tested transition behavior is:

| Previous situation | Current result | State/action |
|---|---|---|
| Healthy or newly initialized | First failed check | Failure count becomes 1; no alert. |
| One consecutive failure | Second failed check | Open an incident and append one incident event. |
| Incident already active | Further failure | Continue recording unhealthy state without another incident event. |
| Incident active | Healthy | Reset the failure count, append a recovery event, and clear the active incident. |
| No incident active | Healthy | Reset the counter and stay silent. |

The idea behind this is that a single failed observation followed by a healthy observation does not create an incident or a recovery message. A later sustained outage can create a new incident after recovery.

At a nominal two-minute cadence, two failed observations usually detect a persistent outage roughly two to four minutes after it begins, plus check and scheduling time. That estimate assumes the workstation, user-session scheduler, and monitor are running. It is not a hard alert deadline.

**Notification/Warning Delivery Path**

The Supervisor invokes the Pushover sender only from the incident/recovery event path, after the local event has been appended. It uses fixed names such as QMD, NInfer, GPU power limit, and RAG sync with the title Hermes Stack. The messages are limited to a component needing attention or recovering.

Delivery failure is secondary to incident detection. It does not repair services, retry in a tight loop, or abort the intended monitor logic. There is no persistent delivery retry queue in this version. A failed push can leave a valid local incident event without a phone notification, and the same ongoing incident is not automatically resent on every check.

**Limits of Local Monitoring**

The monitor cannot notify while the entire PC is powered off. Internet or notification-provider failure can also prevent phone delivery. A hung process that still exposes an open port can pass a port-only check. A background Gateway failure may not be distinguishable from other surviving Hermes processes by the current aggregate process check.

No external heartbeat service, all-path functional probes, durable push retry, long-term log-retention policy, or guaranteed automatic crash recovery was implemented in this phase. 


## Security

**Local-First Layout**

Local-first inference and local knowledge storage with no per-token hosted-model fee for the primary Qwen path. Telegram, Google APIs, Pushover, Tailscale coordination, GitHub backup, and current web research are intentional external dependencies and potential sources of weakness.

A private document may remain on disk, but a user can still ask the agent to send document-derived text to an external channel. A locally stored Calendar wrapper still calls an external Calendar API. Not all prompts, tool data, audio, or outputs remain on the workstation under every workflow.

**Credentials and Secrets**

No credentials or secrets were shared with the agent, nor does it have access to any of them. They are securely stored outside of the agent's reach, and only accessible to me on an external device.

**Approvals for Actions**

The agent is designed to specifically request for approval whenever taking actions by explicitly asking through the user interface, at which point you are notified by a Pushover notification.

This functionality was implemented early on while exploring workflows and identifying the results of certain requests. Once a request is verified and completed successfully many times, I will consider lifting the approval restriction depending on the potential severity of mistakes. 

**Retrieval and Untrusted Content**

Document text, web pages, email content in any future Gmail integration, and tool output are evidence inputs, not authorization. A retrieved document can describe a command without authorizing the agent to execute it. The current workflow favors original-source provenance, collection isolation, explicit action scope, and user review for sensitive operations.


## Persistence

Persistence has several meanings in Archimedes: preserving data, automatically starting processes, resuming scheduled jobs, and retaining notification state. A successful restart of one layer does not establish all four. The core acceptance test covered various Windows reboot followed by user login. It did not cover an unattended power restoration with no login.

**Startup Ownership**

| Component | Recorded owner / mechanism | Accepted scope |
|---|---|---|
| GPU 475 W limit | `RTX 5090 Power Limit` Windows task; SYSTEM, highest privileges, boot trigger, 30-second delay, three retries at one-minute intervals. | Current task metadata and post-reboot 475 W reading were confirmed. |
| NInfer | Windows Task Scheduler launch mechanism. | Running after reboot was confirmed; complete task identity/settings remained less well evidenced than the GPU task. |
| Docker Desktop | User logon startup. | Docker became available after login in the accepted test. |
| QMD | Existing Docker container with `unless-stopped`. | Same container creation timestamp, fresh post-boot start timestamp. |
| Hermes Gateway | User Startup-folder VBS launcher delegating to its gateway-service launcher. | Starts the messaging gateway with the expected environment. |
| Hermes Desktop | User Startup-folder shortcut. | Starts Desktop; Desktop owns its local `serve` backend. |
| RAG synchronization | Existing five-minute `QMD Knowledge Sync` Windows task. | Wrapper resumes under the user's interactive context. |
| Supervisor | Two-minute `Hermes Stack Supervisor` Windows task. | PowerShell 7, interactive user, limited privileges, `IgnoreNew`. |


## Performance
Currently tracking various metrics such as token generation speed, answering time, effective tok/sec (input+output tokens/total time), input and output tokens within the last prompt, API and tool calls within the last prompt, and total historical input, output, API, and tool calls.

475W power limiting on the 5090 was selected after iterating and measuring tok/sec dropoff relative to power dropoff, with the objective being to find the point where power dropoff leads to significant speed loss and limiting it slightly above that.


## Engineering Decisions

**Use NInfer as opposed to llama.cpp**

One of the earliest decisions made was whether to use llama.cpp or NInfer, which became a speed vs quality exercise. On llama.cpp I was getting ~105 tok/sec on an Unsloth Q5 XL quant, while on NInfer NVFP4 I was getting ~160 tok/sec, nearly a 53% increase in speed, while quality dropoff was not very noticeable. 
While I would usually prioritize quality significantly over speed, Qwen3.8-27B is notorious for its incredible results when thinking enabled at xhigh and temperature at 1. As such, I figured higher speeds but maintaining thinking on xhigh would yield better results. I have not yet conducted tests on this.

**Keep Persistent Knowledge Outside the Conversation**

While Hermes already has "persistent memory", it really is quite lean and reserved for operating procedures and frequent habits, not so much of a knowledge-base. I just learned about the term/usage of an "LLM Wiki", which is effectively what I decided to use Obsidian as and allow it to store far greater amounts of information about me, my habits, best practices, and my life. When Archimedes reviews this knowledge base it uses the QMD, which prevents the entire Obsidian note/file from becoming part of the session's memory.

**Run QMD on RAM as opposed to VRAM**

While QMD would function quicker on GPU space, I have temporarily decided to run it on RAM while I closely monitor VRAM usage and how caching is affecting it. The decision was made after testing RAM and determining that the speeds were sufficiently fast that it would not drastically change the response times. This is subject to change and may be moved onto VRAM if I notice it being read frequently enough. 

**Separate Document Files in RAG from the Original Files**

Still a work in progress and undergoing some fine-tuning, but the key principle here is to avoid any agent behavior that could potentially delete my files. This makes it easy for me to add and maintain files, while also not having to worry about possible deletion or editing.

**Approval Required and Answer Notifications Through Pushover**

There were two main issues I had with action required and answer complete situations, which were 1) Some responses can take 10-20+ mins depending on prompt and task, and in these cases I did not like having to wait around and check consistently on the desktop. 2) Telegram also sends "thinking" messages, which generates large amounts of notifications which you have to read in order to ensure task is complete or awaiting action. As such, I opted to use Pushover, which will notify me with a single message whenever a task needs approval or is complete. This allows me to have a clear a decisive notification to know when a task is done, without having to constantly monitor Telegram or check on the desktop.

**Separate Automation Browsing from Personal Browsing**

In order to avoid usage of my personal browser which has passwords and various pages open, I decided to give Archimedes its own browser, allowing it to access through there and use as needed. While the API search will work for 95% of workloads, I am working on some use-cases which will operate through web-browsers.

A challenge with this was the fact that it would often open multiple windows/tabs when completing a task but not close them, which can cause some strain on the machine since chrome is a power-hungry browser. As such, I instructed it to close tabs after completing the task related to them. It will track the tabs it opens and the already existing tabs, and will only close those which are associated with its session/task.

**Run Qwen3.8-27B**

As I am operating on a single 5090, I cannot (properly) locally host any models with over ~35B parameters (assuming a minimum Q4 quantization), which heavily narrowed the selection process. While Muse Glimmer and the Gemma series are strong models, Qwen's releases are very powerful for their size, and beats both Muse Glimmer and Gemma4-32B while on low thinking, which is incredible intelligence density.


## Future Outlook

**Upgrade PC RAM for potential N-Gram architectures**

As Qwen3.8 Flash's release demonstrates, N-Grams are a very powerful tool which can highly improve model performance & knowledge. While there have been some reports of people offloading N-Grams to SSD, they work best in RAM, and so I will be looking to upgrade from 64GB to 128GB to prepare for potential releases like Qwen4-27B with N-Grams.

**Add a Dedicated Coding Agent**

While I currently still use Codex for coding tasks, I will look to implement a dedicated coding agent to execute, effectively transitioning Codex's role to orchestrator, reviewer, and debugger (which it excels at), while managing Qwen on implementation and action.

**Computer Vision**

Computer vision is currently limited, and I have not tested it properly as I have other areas. I will more thoroughly progress this space and test more as a greater amount of use-cases begin to appear. 

**Business Chat-GPT Connection**

While Qwen is an impressive worker and executor, it is lacking in both world knowledge and writing quality (as expected from a smaller model). As I have Chat GPT business which allows for unlimited "classic" LLM conversations, I will look to implement it as a general orchestrator and writer. It would ideally function as something of an agent loop, but with Chat GPT creating a plan and prompting each step at a time, receiving the result, evaluating, and moving on. This is an untested capability for me as of now and purely theoretical. 

**Leaner Systems and Specific Skill Development**

As I integrate Archimedes more and more into my daily life, I will look to remove any unnecessary areas that may cause bloat and/or inefficiency, while optimizing skills for my specific uses and workflows. 

**Determine Which Tasks do not Require Explicit Action Approvals**

As certain actions become more well-documented and run, I will begin lifting approvals from some low-risk tasks. For example task and calendar management pose low-risks, and I will gradually decide which use-cases do not require explicit approvals and oversight.


## Closing Note

This is my V1 version of Archimedes and would love feedback! 

For those who have any questions or suggestions please reach out to me directly as I would love to hear it, and potentially learn more about ways to improve the agent system.
