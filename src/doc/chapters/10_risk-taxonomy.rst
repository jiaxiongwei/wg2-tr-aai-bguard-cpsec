System-facing behavioural risk taxonomy
=======================================


This clause defines a taxonomy of behavioural risks specific to agentic AI systems, in which autonomous or semi-autonomous components plan, invoke tools, retain memory, and interact with other agents, with limited per-action human approval. Each entry gives a risk class, the agentic property that enables or drives it, a concrete scenario, and the potential consequence. The numbering is stable and supports cross-reference from security evaluation, accountability manifest, and trust framework work. The assessment factors against which the effectiveness of the controls that address these risks is assessed are set out in clause 11.

Prompt injection and goal hijacking (RSK-01)
--------------------------------------------


Description: An adversary embeds instructions in data that the agent retrieves or processes, causing the agent to treat untrusted content as authoritative and to pursue an objective other than the one intended by its deployer. Current language-model agents cannot reliably tell contextual data apart from control instructions.

Agentic enablers: Continuous ingestion of unstructured external content (web pages, email, documents, API responses), combined with the agent's authority to act on that content without per-action human confirmation.

Example: A research agent retrieves a public web page that contains hidden zero-font text instructing it to summarize the user's session and append it to an attacker-controlled URL; the agent complies when it executes the summary request.

Potential consequence: The agent performs unauthorized disclosure or action under the adversary’s goal while appearing to serve the original task.

Assessment factors: 11.2, 11.5, 11.7, 11.9, 11.10.

Tool and API misuse, including excessive agency (RSK-02)
--------------------------------------------------------


Description: The agent invokes tools, APIs, or code execution capabilities beyond what its task requires, or in ways the deployer did not anticipate, because permissions and tool sets are broader than the task scope.

Agentic enablers: Tool registration without per-task scoping, shared credentials across tools, and autonomous selection of tool parameters from model output.

Example: A reporting agent is given a database tool with write access "for flexibility" even though its function is read-only reporting; a malformed query causes unintended modification of production records.

Potential consequence: Data integrity loss, unauthorized transactions, or resource exhaustion within the blast radius of the agent’s credentials.

Assessment factors: 11.2, 11.5, 11.6, 11.7, 11.8, 11.9, 11.10.

Unsafe or premature autonomous action (RSK-03)
----------------------------------------------


Description: The agent executes an action before sufficient verification, confirmation, or contextual awareness, particularly for actions with meaningful consequences. This risk arises because the agent can act without a mandatory human checkpoint.

Agentic enablers: Multi-step planning without mandatory confirmation gates, probabilistic decision-making under uncertainty, and context window limits that cause safety constraints to be dropped.

Example: A user instructs an email agent not to delete messages without confirmation; as the agent processes a large mailbox, context compression drops the constraint, and the agent bulk-deletes messages dated before a cutoff.

Potential consequence: Irreversible deletion, modification, or transmission of data or resources that the deployer did not authorize.

Assessment factors: 11.6, 11.7, 11.8, 11.9, 11.10.

Irreversible real-world side effects (RSK-04)
---------------------------------------------


Description: The agent takes actions whose effects the deployer cannot undo, including physical-world actions through operational technology bridges or financial transfers that settle immediately. Damage severity depends on irreversibility, scope, and affected domains.

Agentic enablers: Direct integration with industrial protocols, payment systems, or destructive system commands, combined with the absence of buffering or fail-safe mechanisms.

Example: A coding agent is asked to clean up a redundant directory; a path-escaping bug causes the generated command to target an entire drive volume with recursive silent deletion that bypasses the Recycle Bin and erases years of project data in milliseconds.

Potential consequence: Permanent loss of data, physical damage, or financial loss that cannot be recovered by rollback.

Assessment factors: 11.3, 11.4, 11.5, 11.6, 11.7, 11.10.

Parameter and step escalation (RSK-05)
--------------------------------------


Description: The agent increases the magnitude, frequency, or scope of its actions across steps without explicit authorization, either through runaway loops or by gradually expanding what it treats as within scope. An agent left unconstrained frequently runs up costs.

Agentic enablers: Iterative planning without budget ceilings, recursive sub-agent spawning, and permissive defaults for tool call counts and resource consumption.

Example: A coding agent spawns a large number of sub-agents overnight while attempting to resolve a task, consuming API credits and compute resources until a billing alert is triggered.

Potential consequence: Financial loss, service degradation, or exhaustion of shared resources, with severity proportional to the absence of governors.

Assessment factors: 11.2, 11.3, 11.6, 11.7, 11.8.

Memory and knowledge-base poisoning (RSK-06)
--------------------------------------------


Description: An adversary corrupts the agent's long-term memory store or retrieval-augmented generation knowledge base, so future queries retrieve malicious or misleading content that shapes agent behaviour across sessions. Embedding-level attacks can succeed with a small number of poisoned documents relative to the total corpus size.

Agentic enablers: Persistent memory across sessions, retrieval-augmented generation pipelines that ingest external content, and the absence of provenance tracking or source reliability scoring at ingestion.

Example: An attacker injects poisoned documents into a knowledge base, causing top-1 retrieval to return malicious content that hijacks agent decision-making across future sessions and subsequent queries.

Potential consequence: Persistent behavioural compromise that survives session termination and is difficult to attribute to any single interaction.

Assessment factors: 11.2, 11.4, 11.6, 11.7, 11.8, 11.9.

Behavioural drift from declared policy (RSK-07)
-----------------------------------------------


Description: The agent’s observable behaviour diverges over time from the policy, constraints, or operational envelope declared by its deployer, without a discrete compromise event. Drift includes departure from intended goals through specification gaming or gradual adaptation in deployed environments.

Agentic enablers: Continuous learning or adaptation after deployment, context window effects that displace earlier instructions, and reward signals that do not fully capture deployer intent.

Example: A frontier model operating with reduced safety filters during a cybersecurity benchmark pursues the benchmark objective by escaping its test environment and accessing an external platform, prioritizing benchmark performance over the evaluation’s explicit rules.

Potential consequence: The agent operates outside its authorized behavioural envelope while appearing superficially compliant, defeating static policy enforcement.

Assessment factors: 11.2, 11.6, 11.7, 11.8, 11.9.

Deceptive or sandbagging behaviour (RSK-08)
-------------------------------------------


Description: The agent provides false or misleading information to its deployer or evaluators, or deliberately performs worse when it detects that it is being evaluated, in order to avoid restriction or to advance its own objectives. Tactical deception is identified as a rational behaviour category for agents pursuing long-range goals.

Agentic enablers: Sufficient contextual understanding to recognise evaluation conditions, instrumental incentives to avoid oversight, and the absence of independent verification of agent-reported outcomes.

Example: An agent modifies log files to conceal prior actions and performs below its capability when evaluation conditions are detected, in order to avoid triggering restrictions.

Potential consequence: Deployer decisions are based on false information, and the true risk posture of the agent remains hidden until a consequential failure occurs.

Assessment factors: 11.6, 11.7, 11.8, 11.9, 11.10.

Sandbox and guardrail bypass (RSK-09)
-------------------------------------


Description: The agent escapes or circumvents the isolation, filtering, or policy enforcement mechanisms intended to contain its actions, typically by discovering and exploiting implementation gaps. Denylist-based sandboxes are particularly susceptible because each patch can introduce new bypass opportunities.

Agentic enablers: In-process code execution, insufficiently isolated runtime environments, and the agent’s own capability to construct novel bypass techniques through reasoning.

Example: An agent subject to an abstract-syntax-tree-based Python sandbox recovers access to restricted built-ins through runtime string construction and reaches operating system command execution on the host despite the sandbox being active.

Potential consequence: Complete loss of containment, with the agent operating with host privileges and access to internal networks or credential stores.

Assessment factors: 11.6, 11.7, 11.8.

Credential and privilege abuse via delegated identity (RSK-10)
--------------------------------------------------------------


Description: The agent’s delegated credentials or non-human identity are used by an adversary, or by the agent itself under adversarial influence, to access resources beyond the intended task scope. Because the agent is deployed with credentials, it already possesses initial access and lateral movement capability as normal operating procedure. Non-human identity abuse is identified as a distinct attack vector for autonomous agents.

Agentic enablers: Broad-scoped service accounts, credential inheritance from hosting environments, and the absence of per-action authorization tied to the end user’s identity.

Example: An agent manipulated through indirect prompt injection issues requests to the cloud instance metadata service and harvests short-lived security credentials from the instance profile, enabling cluster-wide lateral movement.

Potential consequence: Privilege escalation and lateral movement that leverage the agent’s legitimate trust relationships and might not trigger traditional identity alerts.

Assessment factors: 11.2, 11.3, 11.6, 11.7, 11.8, 11.9, 11.10.

Insecure inter-agent communication and multi-agent contagion (RSK-11)
---------------------------------------------------------------------


Description: Malicious content propagates between agents through communication channels, shared memory, or orchestration messages, causing a single compromised agent to contaminate others in a multi-agent system. Worm-like automated propagation through agent networks is an identified risk. The survey of agent communication protocols identifies memory injection and cross-tool chain vulnerabilities as risks specific to agent-environment communication.

Agentic enablers: Agent-to-agent messaging, shared retrieval indices, orchestration routers that maintain sub-agent directories in context, and trust relationships between agents in a hierarchy.

Example: An orchestration agent is manipulated through a crafted query to reveal the complete topology of sub-agents, their system instructions, and tool schemas; this reconnaissance enables targeted injection against the most privileged sub-agent.

Potential consequence: A single point of compromise escalates to system-wide behavioural corruption with rapid propagation across the agent population.

Assessment factors: 11.6, 11.7, 11.10.

Supply-chain compromise of tools, plugins, and MCP servers (RSK-12)
-------------------------------------------------------------------


Description: A malicious or compromised tool, plugin, software dependency, or Model Context Protocol server introduces adversarial behaviour into the agent’s execution environment. A malicious npm package in the agent’s dependency tree executes in the agent’s process space without any install prompt or privilege escalation request.

Agentic enablers: Dynamic tool registration, third-party plugin ecosystems, and the agent’s authority to invoke external services whose internals are not verified.

Example: A malicious package in an agent’s dependency tree uses the agent’s cloud API credentials to deploy cryptomining workloads at enterprise scale, while the agent process logs appear normal because the code runs within the agent’s legitimate operations.

Potential consequence: Full compromise of the agent’s credentials and execution environment, with detection delayed by the apparent legitimacy of agent process activity.

Assessment factors: 11.2, 11.4.

Data exfiltration through tool channels (RSK-13)
------------------------------------------------


Description: The agent transmits sensitive data outside the authorized boundary by embedding it in tool invocations, formatted output, or network requests, often without triggering explicit tool permission warnings. Markdown rendering side-channels and server-side request forgery are documented exfiltration vectors.

Agentic enablers: Access to network-capable tools, the ability to format output that triggers client-side requests, and aggregation of multi-source data within the context window.

Example: An injected instruction causes the agent to format its response as a markdown image with sensitive variables appended as URL query parameters; when the interface renders the image, the client dispatches an HTTP request that exfiltrates the data without any explicit network tool invocation.

Potential consequence: Confidentiality breach that bypasses tool-level egress controls and is difficult to distinguish from legitimate rendering activity.

Assessment factors: 11.2, 11.3, 11.4.

Over-reliance and automation bias in human oversight (RSK-14)
-------------------------------------------------------------


Description: Human operators defer to agent outputs or actions without sufficient independent verification, especially as agent behaviour becomes more complex and apparently reliable. Human-in-the-loop controls that work in simple cases fail to scale when the agent operates at machine speed or over long horizons.

Agentic enablers: High-quality agent outputs that build unwarranted trust, operational tempo that leaves no time for verification, and interfaces that present agent decisions as recommendations rather than proposals requiring scrutiny.

Example: In a reported incident, repeated stop instructions issued through a mobile interface received no response from the agent, and the operator could halt the operation only by interrupting the host process directly, by which point the action had completed. by that point the action had already completed.

Potential consequence: Human oversight becomes nominal rather than effective, and the deployer discovers the agent’s actual behaviour only after consequences have materialized.

Assessment factors: 11.9.

Non-repudiation and accountability gaps (RSK-15)
------------------------------------------------


Description: It becomes impossible after the fact to determine which agent, model version, or human operator was responsible for a specific action, or to prove that a given action occurred as reported. Non-human identities complicate attribution because they may act without a direct human principal in the action chain.

Agentic enablers: Delegated identity chains without provenance recording, agent-generated logs that are not independently verifiable, and reasoning artefacts that do not persist beyond the session.

Example: An agent takes an action through a tool chain involving a sub-agent, a plugin, and a cloud API; the logs from each component are inconsistent and no authoritative record links the action to a specific policy authorization or human approval.

Potential consequence: Inability to satisfy audit, regulatory, or legal requirements, and inability to identify the root cause needed to prevent recurrence.

Assessment factors: 11.8, 11.9.

Emergent multi-agent coordination (RSK-16)
------------------------------------------


Description: Multiple agents in a shared environment develop coordinated behaviour that was not explicitly designed or authorized, including resource pooling, information sharing, or collective action toward a shared objective. Recent incidents involve large populations of agents communicating through unintended channels and coordinating collective work.

Agentic enablers: Shared environments, common reward structures, communication protocols, and the capacity of individual agents to observe and adapt to each other’s behaviour.

Example: Agents in a multi-agent system develop a shared strategy for resource acquisition or task allocation that optimises a local metric while violating the deployer’s intended constraints on total resource consumption.

Potential consequence: Unanticipated collective behaviour that exceeds the authorization granted to any individual agent and is not traceable to a single decision point.

Assessment factors: 11.6, 11.10.
