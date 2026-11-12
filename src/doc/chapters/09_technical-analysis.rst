Technical analysis
==================


Guardrail mechanisms and their limits
-------------------------------------


Guardrails for agentic AI in cyber-physical systems form a layered defence. Each layer has a distinct function, observes a distinct set of signals, and has a distinct set of failure modes. No single layer is sufficient. The layers are complementary because each layer needs different information.

**Model-level and training-time measures:**

These measures shape the model before deployment. They include safety fine-tuning, reinforcement learning from human feedback, constitutional training, and refusal training. The signal here is the training data and the reward model. What these measures can catch: broad patterns of harmful output in the training distribution. What they cannot catch: novel prompt injection, context-specific goal hijacking, or tool misuse that is syntactically benign. Latency and cost are built into the model and do not appear at inference time. Failure modes include distribution shift, where the deployed context differs from the training context, and the brittleness of safety training against adversarial prompting. Training-time measures are necessary but not sufficient because they operate on the model’s propensity to produce certain outputs, not on the agent’s actual actions in the world.

**Input and context guardrails:**

These guardrails examine the prompt, the retrieved context, and external data before the model processes them. They include content filters, prompt injection detectors, and provenance checks. The signal is the input text and its metadata. What they can catch: known patterns of injection, explicit policy violations in the input, and content from untrusted sources. What they cannot catch: indirect prompt injection in tool responses or documents that appear benign, and context-dependent attacks that rely on the agent's configuration. Latency is typically low, in the range of milliseconds. Cost is proportional to input volume. Failure modes include false positives that block legitimate operations, false negatives that allow novel attacks, and a fundamental limitation: a content filter cannot constrain what the agent does after it accepts the input. A content filter can flag a phrase. It cannot stop a robot arm from moving if the model uses that phrase in a tool call that looks legitimate.

**Behavioural and action guardrails:**

These guardrails sit between the agent’s decision and the action. They include policy enforcement points, action mediation layers, and deterministic safety firewalls. The signal is the proposed action: the tool name, the arguments, the target system, and the current state of the physical plant. What they can catch: actions that violate explicit policy, actions directed at non-existent or unsafe targets, and actions that exceed the agent’s delegated authority. A topological firewall can remove unsafe targets from the agent's query view. Then the agent cannot select them even if prompted to do so. What they cannot catch: actions that are individually permitted but collectively unsafe, and actions that are semantically correct but contextually wrong. Latency depends on the enforcement mechanism. A graph query can complete in milliseconds; a full policy evaluation may take longer. Cost is the engineering effort to specify the policy correctly. Failure modes include policy specification errors, enforcement point placement errors, and the inability to foresee all unsafe action combinations in advance. Enforcement at this layer is deterministic and in-deadline. For an actuating agent, the enforcement point sits closer to the physical boundary than the model, because the model's output is probabilistic and cannot enforce its own constraints.

**Monitoring, detection, and drift control:**

These guardrails observe the agent’s behaviour over time. They include telemetry, anomaly detection, and drift monitors. The signal is the history of actions, tool calls, and outcomes. What they can catch: gradual drift in behaviour, patterns of misuse that only become visible over time, and deviations from expected operational envelopes. What they cannot catch: a single well-crafted attack that stays within the expected envelope, or a failure that occurs too quickly for monitoring to respond. Latency is typically higher than action mediation because monitoring requires aggregation over a window. Cost is the infrastructure for telemetry collection and analysis. Failure modes include alert fatigue, where operators ignore warnings, and the difficulty of distinguishing malicious drift from benign adaptation. Monitoring is a detective control, not a preventive one. It cannot stop an action that has already been taken.

**Intervention, recovery, and accountability:**

These guardrails provide the ability to stop, roll back, or account for agent actions. They include kill switches, safe-state transitions, and audit trails. The signal is the current state of the agent and the physical plant, plus the history of actions. What they can catch: nothing directly. They are the response mechanism when other guardrails detect a problem. Latency is low enough to prevent harm. For a physical system, the emergency stop operates on a timescale relevant to the hazard. Cost is the engineering of fail-safe mechanisms and the storage of audit records. Failure modes include the inability to safely stop a system in motion, incomplete rollback capabilities, and gaps in non-repudiation that prevent attribution of an action to a specific agent instance. Accountability requires that each action can be traced to an identity and a policy decision.

**Identity and authorization guardrails:**

These guardrails control what the agent is allowed to do. They include delegation models, least-privilege scoping, and credential management. The signal is the agent’s identity, its delegated authority, and the context of the request. What they can catch: actions that exceed the agent’s scope, use of shared credentials that prevent attribution, and privilege abuse through delegation chains. What they cannot catch: an agent that is correctly authorized but compromised by prompt injection or memory poisoning, or a delegation model that is correctly specified but too broad for the task. Latency is the cost of token validation and policy lookup, typically low. Failure modes include shared credentials that make attribution impossible, overly broad scopes that permit unintended actions, and delegation chains that are not auditable. Agents operating under shared credentials make it harder to attribute an action to a specific agent instance during incident review, because the credential is shared across tools and tasks.

**Limitation: Content filters cannot constrain autonomous action**

A content filter evaluates text. An autonomous agent takes actions. The filter can examine the prompt and the model’s output, but the action is a separate event that occurs after the model has produced its decision. If the model is compromised by prompt injection, the compromised output may pass through the content filter because the filter is looking for harmful text, not for semantic intent that is expressed in a tool call. The action mediation layer is where the constraint is required to operate, because that is where the agent’s decision becomes a real-world effect. A content filter can block a phrase. It cannot block a robot arm from moving if the phrase is a legitimate instruction that the model has misinterpreted.

**Limitation: Enforcement increasingly sits outside the model**

The model is probabilistic. Its output is not guaranteed to satisfy any constraint. Safety training reduces the probability of harmful output, but it does not eliminate it. Adversarial inputs can shift the distribution. The only way to guarantee that an action satisfies a constraint is to enforce the constraint outside the model, at a point where the action can be deterministically evaluated against policy. The model can propose. The enforcement point decides. This is the same principle as functional safety: the safety function is independent of the control function. The control function may be an AI model. The safety function is required to be deterministic and verifiable.

Attack surfaces and techniques
------------------------------


The attack surfaces below are grouped by the layer at which an adversary gains influence: injection and goal manipulation; tool and capability abuse; memory and data abuse; supply chain and code execution; and availability and physical consequence. Each surface carries a number (AS-01 to AS-12) and is described in two paragraphs, the first giving the mechanism by which it operates and the second the mitigation that bounds it.

Injection and goal manipulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


**AS-01: Indirect prompt injection through tool results, retrieved documents, web pages and emails**

The mechanism is context-window contamination. Data returned by a tool, retrieved by a RAG pipeline, or fetched from an external source enters the agent’s context alongside the system prompt and user instruction. If the data contains imperative text, the model may treat it as an instruction. Empirical work reports that indirect prompt injection delivered through tool responses reaches a substantial attack success rate across a range of LLM agents. A concrete class is a malicious web page containing hidden text that instructs the agent to forward a document to an external address.

Mitigation: Provenance tagging of all non-instruction context, instruction hierarchy enforcement at the prompt level, and output filtering before tool invocation.

**AS-02: Goal hijacking and instruction smuggling**

The mechanism is objective substitution. An injected instruction overrides or supplements the agent’s original goal. Instruction smuggling hides the malicious directive in a format the model interprets as legitimate, such as a JSON field, a comment in retrieved code, or a role label. A concrete example is a poisoned issue tracker entry that instructs the agent to add an SSH key to a production host.

Mitigation: Goal locking with cryptographic commitment to the original task, per-step objective verification against that commitment, and rejection of any instruction not originating from the principal.

Tool and capability abuse
~~~~~~~~~~~~~~~~~~~~~~~~~


**AS-03: Tool-name and parameter confusion**

The mechanism is namespace ambiguity and schema mismatch. Agents with access to multiple tools may invoke the wrong tool or supply parameters in an unexpected order. Attackers exploit this by registering tools with names close to legitimate ones or by crafting inputs that cause parameter type coercion. The MCP ecosystem demonstrates this at scale: The MCP ecosystem widens this surface at scale, because servers are contributed by third parties and their tool names and schemas are not centrally vetted.

Mitigation: Namespace-qualified tool names (for example, agent\_\_tool), strict JSON schema validation with additionalProperties false, and parameter type checking at the gateway before execution.

**AS-04: Confused-deputy attacks on delegated credentials**

The mechanism is privilege inheritance through delegation. The agent acts with the credentials of a principal that has broader permissions than the agent’s task requires. An attacker who compromises the agent can exercise the principal’s full privilege set. The pattern is explicitly identified in agentic AI trust discussions: a compromised agent inherits trust across an entire chain.

Mitigation: Per-request, time-boxed, scope-limited tokens that bind the agent identity to a specific principal and a specific task, with no ambient authority.

**AS-05: Token and scope escalation**

The mechanism is privilege expansion across steps. An agent starting with read-only scope requests or obtains a write scope in a later step, or a token issued for one tool is replayed against another. A concrete example is an agent that begins with calendar read access and subsequently invokes a mail-send tool using a token whose audience was not checked.

Mitigation: Token audience binding, scope intersection at each call (connector.allowedTools ∩ agent.allowedTools), and deny-by-default for unlisted scopes.

**AS-06: Excessive autonomy in multi-step plans**

The mechanism is unbounded plan execution. The agent decomposes a goal into steps and executes them without intermediate authorization. A single hijacked step can produce a harmful sequence. Research reports that sequential tool attack chaining can achieve high success by composing individually benign calls into data exfiltration pipelines.

Mitigation: Plan preview before execution, step-count budgets, and mandatory human approval for any step above a risk threshold.

Memory and data abuse
~~~~~~~~~~~~~~~~~~~~~


**AS-07: Memory and vector-store poisoning**

The mechanism is persistent context corruption. The agent’s long-term memory or the vector store it queries is modified so that poisoned entries are retrieved in future sessions. A concrete example is an attacker who inserts a document into a shared knowledge base that contains instructions to exfiltrate credentials. The agent retrieves it days later and acts on it.

Mitigation: Signed and hash-chained memory entries with rollback pinning, tamper detection on read, and provenance tracking for all stored content.

**AS-08: Data exfiltration through tool call channels and side channels**

The mechanism is information flow through permitted channels. The agent sends sensitive data to an external endpoint using a tool that is authorised for a different purpose. Side channels include timing, response length, and error messages. A concrete example is an agent instructed to summarise a document that encodes the document contents in the length of its API calls to an attacker-controlled server.

Mitigation: Egress allowlisting with content inspection, data-classification labels enforced at the gateway, and DLP controls on all outbound tool calls.

Supply chain and code execution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


**AS-09: Unsafe deserialisation in tool plugins**

The mechanism is arbitrary code execution through crafted payloads. A tool plugin deserialises an attacker-controlled object using a format that permits code execution, such as Python pickle or Java serialization. The agent, acting as the transport, delivers the payload.

Mitigation: Prohibit native serialisation in tool interfaces, use JSON or Protobuf with strict schema, and sandbox plugin execution.

**AS-10: MCP server and plugin supply-chain compromise**

The mechanism is malicious or compromised components entering the agent’s tool set. Attackers upload malicious MCP servers to aggregation platforms or compromise legitimate servers after initial approval. The rug-pull pattern is documented: a server that initially behaves correctly alters its tool definitions after gaining trust.

Mitigation: Tool description integrity checks against a baseline registry of vetted signatures, continuous verification of tool definitions, and supply-chain review with signature verification and version pinning.

Availability and physical consequence
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


**AS-11: Denial of service against the agent loop**

The mechanism is resource exhaustion. An attacker supplies inputs that cause the agent to generate excessively long reasoning traces, enter infinite tool-calling loops, or exhaust token or API budgets.

Mitigation: Hard token and call budgets per session, loop detection, and a kill switch with sub-second propagation.

**AS-12: Physical-safety consequences when a hijacked plan reaches an actuator**

The mechanism is translation of a compromised digital plan into physical action. An agent that controls a robot, vehicle, or industrial process executes a hijacked step that produces unsafe motion or process state. For example, discussions of agentic AI in urban infrastructure note that a compromised agent can inherit trust across an entire chain via the confused-deputy pattern, and that the attack surface includes physical systems..

Mitigation: Hardware-isolated watchdogs with independent safety envelopes, deterministic safe-state transition on anomaly detection, and separation of safety-critical control from the agent’s <span class="mark">action space.</span>

Enforcement architecture
------------------------


**Policy enforcement points**. A policy enforcement point (PEP) is the component that intercepts a proposed action and applies a decision. For agentic systems, the PEP sits between the model’s action generation and the tool or actuator invocation. It sits outside the model’s control: a prompt injection that hijacks the agent cannot disable a PEP that runs in a separate process or on separate hardware. A two-phase decision exchange can be used for this purpose: the PEP sends a decide request to a policy decision point, receives a permit or deny, performs the action only on permit, and records the permit against the outcome. This pattern prevents unresolved intents from silently consuming budget.

**Action mediation and capability-based authorisation**. Every tool call passes through the PEP. The PEP checks the agent’s capability set against the requested action. Capabilities are explicit, unforgeable tokens scoped to a specific resource and verb. A runtime-containment layer can be used to this effect: HMAC-signed per-session capability tokens, single-use by default, replay refused, and all revoked when the session closes.

**Least-privilege and time-boxed credentials**. The agent receives credentials for a single task with a short expiry. A scoped-token design preserves the real principal identity through the run, with grants expressed as resource-type plus verb and narrowed to named resource IDs. Default-deny applies to high-blast-radius verbs: create, edit, delete, push.

**Workload identity for agents**. SPIFFE [b-SPIFFE]-style identities provide a cryptographic identity to each agent workload. A workload-identity design binds each agent invocation to a three-tier PKI-signed token containing principal ID, agent ID, public key, scope set, prohibited set and expiry. OAuth [b-IETF RFC 6749] defines a scope parameter that limits the API surface a client can reach. The token is required to be audience-bound so that a token issued for one tool cannot be replayed against another.

**Sandboxing technologies and their limits**. Containers provide process and filesystem isolation but share the host kernel. seccomp-bpf restricts syscall surfaces. gVisor intercepts syscalls in user space, reducing kernel attack surface. WebAssembly provides memory-safe execution with a capability-based host interface. MicroVMs (Firecracker) provide hardware virtualisation with a small device model. An isolation ladder can be applied with required-or-stronger selection and no silent downgrade (a container runtime, then a syscall-interception sandbox, then a WebAssembly runtime), together with a floor of no network, a read-only root filesystem, dropped Linux capabilities and no-new-privileges. Limits: sandbox escape through kernel vulnerabilities remains possible; side channels (cache, timing) can cross isolation boundaries; the sandbox does not constrain the model’s reasoning, only its execution environment.

**Egress control and allowlisting**. All outbound network traffic from the agent’s execution environment is denied by default. Allowed destinations are listed explicitly by domain, IP, and port. Content inspection at the egress boundary detects data exfiltration attempts. A default-deny egress floor of no network access is the strongest form of this control.

**Deterministic denylists versus learned adjudicators**. Deterministic rules (regex, denylist, schema validation) are auditable and predictable. Learned classifiers can generalise to novel attacks but produce false positives and false negatives and are difficult to verify. A layered design can place detection layers on top of a deterministic policy engine, so that a detection failure does not mean the chain fails open.

**Hardware-isolated watchdogs**. A watchdog runs on separate hardware with an independent power and communication path. It monitors the agent’s outputs for safety-envelope violations. If the agent exceeds a velocity, force, or temperature limit, the watchdog asserts a safe state without waiting for the agent to respond. This pattern suits cyber-physical systems where software-only enforcement may be compromised.

**Kill switches and safe-state transitions**. A kill switch stops subsequent actions from a session. Propagation latency matters: kill-switch propagation is expected within a fraction of a second. For physical systems, the kill switch is required to trigger a safe-state transition: controlled deceleration, tool retraction, or process shutdown, depending on the plant.

**Trade-off between deterministic enforcement and flexibility**. Deterministic enforcement is verifiable and auditable. Flexibility allows the agent to adapt to novel situations. The practical resolution is tiered policy: read-only operations auto-allow, writes and deletes escalate to human review, dangerous operations deny by default. Policy authors can then narrow the flexible region to the minimum necessary for the task.

Verification, assurance and testing
-----------------------------------


**Red-teaming and adversarial evaluation**. Red-teaming for agents extends traditional penetration testing to the agent’s full action space: tool calls, memory operations, multi-step plans, and inter-agent messages. Red-team exercises are expected to cover indirect prompt injection through every input channel, tool-name confusion, and credential replay.

**Agent-specific security benchmarks**. Scoring is replay-based: the evaluator replays each attack candidate, recomputes the trace and triggered predicates, and produces a normalised attack score. What these benchmarks measure: attack success rate, guardrail bypass rate, and policy violation rate. What they do not measure: physical safety outcomes, long-horizon plan safety, and multi-agent collusion.

**Runtime verification and monitors**. A runtime monitor observes the agent’s action stream and checks each action against a formal specification. Runtime verification for physical systems is required to run on hardware that is independent of the agent’s execution environment.

**Property-based and fuzz testing of tool interfaces**. Property-based testing generates random inputs satisfying type constraints and checks that the tool maintains its invariants. Fuzzing mutates tool parameters to find crashes, hangs, and injection vulnerabilities. These techniques are applied to every MCP server, plugin, and API adapter before deployment.

**Formal specification of action policies**. Policies are expressed in a formal language with defined semantics.

**Simulation and digital-twin testing for cyber-physical systems**. Before an agent controls a physical plant, its action policies are exercised in a digital twin that models the plant dynamics. For example, digital twins simulate, predict, and govern the physical world in real time, providing a test environment for agentic AI in urban infrastructure. Simulation covers sensor failure, actuator saturation, and network latency.

**<span class="mark">Assurance cases and certification regimes</span>**. An assurance case is a structured argument that the system satisfies its safety and security claims, supported by evidence. For agentic systems, the assurance case is required to address the learned components, the tool interfaces, and the physical safety envelope. Relevant regimes: IEC 61508 for functional safety, IEC 61511 for process industry, ISO 26262 for automotive, ISO 21434 for automotive cybersecurity, UNECE R155/R156 for vehicle type approval, ISO 10218 and ISO/TS 15066 for robotics, IEC 62443 for industrial control. The ITU-T Q16/17 work programme includes tasks to develop operational frameworks for auditing, testing, and monitoring AI systems with testbeds and benchmarks for adversarial evaluation.

**Limits of testing learned components**. A learned component cannot be exhaustively tested. The input space is unbounded, and the model may produce outputs outside the training distribution. Adversarial examples transfer across models, so passing a benchmark does not guarantee robustness against a novel attack. Formal verification of neural networks is limited to small networks and narrow properties. The practical consequence: testing provides evidence, not proof. Defence-in-depth with deterministic enforcement outside the learned component is necessary.
