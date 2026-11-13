Overview
========


From generative AI to agentic AI
--------------------------------


Generative AI (non-agentic) systems produce content — text, images, code, or structured data — that a human interprets and acts upon. The output is the endpoint of the system’s operation. The human remains the actor; the AI advises, drafts, or recommends. Agentic AI systems, by contrast, pursue goals and take actions in the world, either digital or physical. They plan, invoke tools and application programming interfaces, maintain persistent memory across sessions, operate under their own identity and permissions, and may coordinate with other agents in multi-agent configurations. The distinction is not merely one of degree; it is architectural. An agent is a persistent closed-loop system with actuators and objectives, whereas a generative model is not an agent by default.

Table 1 summarizes the principal dimensions of contrast. The table is informative and does not prescribe any particular architecture or deployment model.

.. list-table:: Table 1 - Comparison of generative and agentic AI
    :header-rows: 1
    :widths: 17 35 47

    * - Dimension
      - Generative (non-agentic) AI
      - Agentic AI
    * - Primary output
      - Content (text, image, code) for human consumption
      - Actions (tool calls, API invocations, state changes)
    * - Human role
      - Interpreter and decision-maker after output
      - Delegator of authority; may be out of the loop
    * - Autonomy
      - None beyond generation parameters
      - Goal-directed, multi-step planning and execution
    * - Memory
      - Session or prompt-bounded
      - Persistent across sessions and tasks
    * - Identity and permissions
      - Typically none beyond API credentials
      - Own identity, inherited or delegated permissions, authority chains
    * - Tool and API invocation
      - Absent
      - Central capability; agents compose and sequence tool calls
    * - Error propagation
      - Localized to a single output
      - Compounding across steps; errors feed forward
    * - Multi-agent coordination
      - Not applicable
      - Possible; agents delegate to and constrain other agents
    * - Long-horizon execution
      - Single turn or bounded conversation
      - Extended task execution over minutes, hours, or longer


The transition from generative to agentic systems introduces what some analyses describe as an action boundary: the moment between when an agent decides what to do and when a downstream component executes it. At this boundary, governance decisions can be made deterministically, recorded with integrity, and enforced regardless of which model the agent is running. The action boundary did not exist for most of AI’s history because classifier outputs, translations, and recommendations were themselves the action. Agents break this assumption by producing structured action proposals rather than terminal outputs.

Behavioural safety and behavioural security
-------------------------------------------


Behavioural safety, as used in this Technical Report, means that an agent’s observable actions stay within authorised, policy-compliant bounds while it runs. It is concerned with what the agent does, not with what it says or what its internal representations contain. Behavioural safety is evaluated against declared policies, permission models, and operational constraints, and it addresses the full execution lifecycle rather than a single output.

Behavioural safety is distinct from several related concepts with which it is sometimes conflated. Alignment refers to the broader problem of ensuring that an AI system’s goals and values correspond to human intentions; it is a property of the system’s objective specification and training, not of runtime action monitoring. Red-teaming is an adversarial assessment methodology that probes for failures; it is a technique for discovering behavioural safety gaps, not the property itself. Content moderation concerns the acceptability of generated content and operates primarily at the inference layer on token streams or outputs. Model evaluation assesses model capabilities and risks in controlled settings; it characterizes the model, not the deployed agent’s runtime behaviour under real permissions and tools.

Behavioural security, as used in this Technical Report, means that an agent's observable actions cannot be turned against the external cyber-physical system by an adversary. It is concerned with influence entering through a compromised model, tool, plugin, memory store, credential or communication channel, and with the mechanisms that prevent such influence from becoming harm to the plant. Behavioural security is evaluated against declared policies, permission models and operational constraints, and it addresses the full execution lifecycle rather than a single output.

Behavioural security is distinct from neighbouring concepts. Adversarial machine learning and model extraction target the model rather than the system the agent acts on. IT security controls applied to the agent's own infrastructure protect that infrastructure as an asset. Conventional OT and ICS security assumes no autonomous agent in the loop. Behavioural security addresses the conversion of adversarial influence over an agent into effects on the external cyber-physical system.

In this Technical Report, a guardrail means a technical or procedural mechanism that constrains, monitors or intervenes in agent behaviour. Guardrails operate at multiple layers. At the training layer, methods such as reinforcement learning from human feedback and constitutional AI shape model dispositions. At the inference layer, system prompts, output filters, and frameworks such as NeMo Guardrails [b-NeMo Guardrails] operate on token streams. At the action boundary, capability checks, policy enforcement, risk assessment, and audit mechanisms operate on structured action proposals before execution. At the infrastructure layer, access control lists, identity and access management, API gateways, and network segmentation operate on system effects. Behavioural safety guardrails, as addressed in this Technical Report, are concerned primarily with the action boundary and with mechanisms that ensure the agent’s observable actions remain within authorized bounds.

This report addresses both safety and security, and the two are distinct. Safety concerns harm to persons and physical assets arising from the action of an agent that is not under adversarial control. Security concerns harm arising when the agent, its tools, its supply chain or its communication channel is compromised by an adversary. The guardrail mechanisms overlap - action mediation and the physical safety interlock serve both - but the threat models differ, and a framework that addresses only one of them leaves the external system exposed to the other. The concept of capability versus authority is central. An agent that can delete a database is not the same as an agent that may delete a database. Guardrails enforce this distinction by ensuring that action proposals are checked against authority before they become effects.

In both cases the object of protection is the external cyber-physical system. An agent that is itself compromised is a cause of risk, not a principal to be protected: the guardrail exists to bound what the agent can do to the plant, the equipment, the network and the people around it. Controls that protect the agent from the outside world (for example, protecting a model from extraction) are therefore outside the scope of this Technical Report except where they are required to bound the risk that the agent poses to the external cyber-physical system.

Why agentic behaviour changes the safety and security problem
-------------------------------------------------------------


The shift from advise to act transforms the safety problem in several interrelated ways.

First, agentic actions have real-world side effects. A generative model that produces incorrect financial advice may mislead a human; an agentic system that executes a trade or transfers funds creates immediate, potentially irreversible consequences. Irreversibility is a defining concern: some actions, such as deleting data, sending communications, or executing financial transactions, cannot be undone. The cost of a safety failure is therefore not a bad output that a human can ignore, but a state change that may require remediation.

Second, errors compound across steps. In multi-step execution, an error in an early tool call or planning decision propagates forward, potentially amplifying rather than cancelling. A generative model’s error is localised to one output; an agent’s error may cascade through a sequence of dependent actions. This property makes per-step accuracy insufficient as a safety metric; what matters is the trajectory of actions against policy.

Third, the blast radius is larger. An agent with access to multiple tools and APIs can affect systems and data far beyond the immediate task. Compromised reasoning processes can yield insider-equivalent access; legitimate but contextually inappropriate API calls can exfiltrate data; unbounded recursive invocation can exhaust resources. The authority an agent inherits or is delegated determines the scope of potential harm.

Fourth, prompt injection becomes action injection. In generative systems, prompt injection manipulates output; in agentic systems, it manipulates what the agent does. An injected instruction that the agent treats as authoritative can cause it to invoke tools, access data, or take actions that the user never intended. The injection surface expands to include any external content the agent processes: retrieved documents, web pages, API responses, and messages from other agents. Indirect prompt injection is particularly relevant because the agent may encounter adversarial content through normal operation.

Fifth, authority and permission inheritance create chains of delegation that are difficult to audit. Agents may be granted permissions by users, by orchestrators, or by other agents. They may delegate capabilities to sub-agents. The resulting trust chains can be complex, and authority that is appropriate for one agent may be excessive when inherited by another. Identity spoofing across multi-agent pipelines and privilege escalation through ambiguous authority models are recognised risks in agentic deployments.

Sixth, attributing responsibility becomes difficult when harm occurs. A human who acts bears institutional consequences; an AI system that acts does not. When an agentic system produces a harmful outcome, the causal chain may involve the model provider, the agent developer, the orchestrator, the tool provider, the user who delegated authority, and potentially other agents. Post-hoc adjustment of the model or deployment is not accountability in the institutional sense; it changes the artefact, not the consequence-bearing agent. The ambiguity of responsibility is a governance problem that accompanies the technical safety problem, and it motivates mechanisms for traceability, audit, and accountability manifest that are addressed elsewhere in SG17 work.

The security problem is created by the same shift from advise to act, but its origin differs. The adversary no longer attacks the plant directly; it influences the agent that commands the plant. The attack surface therefore becomes the agent's context window, tool set, memory, credentials and delegation chain, and the harm appears at the physical plant.

The agentic case has distinctive security properties. An injection becomes a command. A compromised tool or plugin inherits the agent's authority. Long-lived memory lets an adversary persist across sessions. Delegated credentials make an attack indistinguishable from legitimate action. Multi-agent propagation allows one compromised agent to contaminate others. Each of these is a security problem whose object of harm is nevertheless the cyber-physical system.

Together these characteristics — real-world effects, irreversibility, compounding error, expanded blast radius, action-oriented injection, delegated authority and attribution difficulty — define both the behavioural safety problem and the behavioural security problem for agentic AI. The first arises from agent action without adversarial control; the second arises when an adversary gains influence over the agent, its tools, its memory or its delegation chain. Guardrails for agentic systems address both by operating at the action boundary, where the agent has decided what to do but has not yet done it, and where deterministic, model-invariant governance is possible. Clause 10 sets out the risk taxonomy that the behavioural safety and behavioural security problems give rise to.

Why cyber-physical deployment raises the stakes
-----------------------------------------------


In a cyber-physical system the agent’s output is consumed by an actuator - a robot, a vehicle, an industrial control loop, or an embodied or physical AI system - rather than by a human reader. Three properties change the risk calculus.

First, effects are physical and often irreversible. An incorrect action may cause injury, damage or loss of service that cannot be rolled back by editing a subsequent message.

Second, the control loop imposes timing. A guardrail that cannot reach a verdict within the loop deadline is not available at all, so enforcement is required to be deterministic and local rather than delegated to a further model call.

Third, the safety argument is already occupied. Cyber-physical systems are governed by established functional-safety and security regimes, so an agentic layer is required to be integrated into an existing safety case rather than treated as a greenfield control problem.

For these reasons the guarantee required here differs from that of a purely advisory assistant: an advisory guardrail can be probabilistic, whereas a cyber-physical guardrail frequently is required to be deterministic and fail-closed.

The clauses that follow are organised so that the material and the analysis are separated. Clause 7 reviews the current landscape, industrial practice and reported results. Clause 8 states the trends expected to shape the next three to five years. Clause 9 analyses the technical mechanisms by which system-facing guardrails work and the attack surfaces they address. Clause 10 sets out the system-facing behavioural risk taxonomy. Clause 11 states the assessment factors and the collection methods by which guardrail effectiveness is judged. Clause 12 sets out the general assessment framework and clause 13 the elements that appear standardizable; clause 14 states the conclusion. The appendices record the documented events, cases, instruments and related work on which the analysis rests.
