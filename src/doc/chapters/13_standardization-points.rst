Standardization points
======================


This clause collects the standardizable elements of behavioural guardrails for agentic AI. It is deliberately plural: behavioural safety and security for agentic AI is being addressed concurrently by standards bodies, industry consortia, national regulators and research groups.

Two scope conditions frame every point below. First, the object of protection is the external cyber-physical system: the standardizable elements are those that bound what an agent can do to the plant, the equipment, the network and the people around it, not those that protect the agent itself. Second, both safety and security are in view: each element is assessed for whether it reduces harm from an agent that is not under adversarial control, and for whether it reduces harm from an agent, a tool, a supply chain or a channel that an adversary has compromised.

The candidate standardization domains are set out in 13.1 to 13.9 and consolidated in 13.10 (see Table 3). The survey of the instruments that already exist is given in Appendix IV.1, and Table 3 records, for each domain, the gap and the suggested deliverable type.

Terminology and taxonomy
------------------------


The terminology used across the agentic security domain overlaps and varies between sources. A standard vocabulary and taxonomy are needed so that behavioural guardrails, safety constraints, alignment mechanisms and trust controls carry consistent meanings. Clear terms allow designers of cyber-physical systems to state precisely what an agent is permitted to do and to compare guardrail approaches across implementations.

Reference architecture and guardrail placement model
----------------------------------------------------


A reference architecture is needed to show where guardrails sit within an agentic system. Such a model identifies the components, interfaces and trust boundaries that surround an autonomous agent. For cyber-physical systems this placement determines whether a guardrail can intervene before an action reaches actuators, and therefore whether harm is preventable.

Guardrail policy specification and interoperability
---------------------------------------------------


A machine-readable format for guardrail policies is needed so that rules travel between platforms without loss of meaning. Interoperability allows a policy written for one agent runtime to be interpreted by another. In cyber-physical settings this reduces the risk that a safety constraint is misread or silently dropped when an agent or its hosting platform changes.

Drift measurement, metrics and reporting formats
------------------------------------------------


Consistent metrics and reporting formats for behavioural drift are required. Drift occurs when the behaviour an agent exhibits diverges from the behaviour its policy declares. Measurement alone is insufficient without agreed units, thresholds and report structures. For cyber-physical systems, drift detection provides an early signal that an agent may act outside its permitted envelope.

Identity, delegation and authorization requirements for agents
--------------------------------------------------------------


Requirements for identity, delegation and authorization of agents need specification. An autonomous agent acts on behalf of others, and its authority to do so requires clear bounds. Without defined delegation rules, a cyber-physical system cannot determine whether an action originates from a legitimate agent, nor limit the scope of what that agent may affect.

Testing, evaluation and red-teaming methodology for agentic systems
-------------------------------------------------------------------


A methodology for testing, evaluation and adversarial probing of agentic systems is required. Such a methodology describes how guardrail behaviour is exercised across scenarios, including attempts to circumvent constraints. For cyber-physical systems this provides evidence that a guardrail holds under conditions beyond those anticipated in routine operation.

Assurance, conformity and mapping to regulation
-----------------------------------------------


Assurance and conformity approaches need specification so that guardrail implementations can be assessed against regulatory expectations. Mapping between technical evidence and legal obligations allows an operator to demonstrate that an agent is bounded. In cyber-physical contexts this supports accountability when an autonomous action has consequences for people or infrastructure.

Assessment of guardrail effectiveness
-------------------------------------


Assessment factors for guardrail effectiveness require definition. Such factors describe what counts as an effective guardrail and how its performance is judged over time. For cyber-physical systems, a common assessment basis allows operators to compare guardrails and to decide whether a given constraint remains adequate as an agent's tasks evolve.

Unified runtime middleware hooks
--------------------------------


Runtime middleware hooks require specification so that guardrails can be deployed declaratively on an agent execution pipeline. Both guardrail families can be mounted in this way. A defined hook model allows policy to be applied consistently across platforms, which matters when an agent controls equipment whose safe operation depends on timely intervention.

Consolidated standardization points
-----------------------------------


A consolidated set of standardization points is needed to give a coherent view of the gaps identified across this clause. Such a consolidation supports planning without prescribing a work programme. For behavioural guardrails in cyber-physical systems it clarifies which elements require specification and where dependencies between them lie.

.. list-table:: Table 3 - Standardization points and the gaps they address
    :header-rows: 1
    :widths: 28 47 25

    * - Standardization point
      - What is missing today
      - Deliverable type
    * - Terminology and system-facing risk taxonomy
      - Common terms for agentic risk and guardrail categories
      - Recommendation or Technical Report clause
    * - Guardrail placement reference model
      - A shared placement model over architectural loci and lifecycle stages
      - Recommendation
    * - Middleware hook set and policy interface
      - A portable way to attach guardrail policy to an agent execution pipeline
      - Technical Report or specification
    * - Guardrail policy specification and profiles
      - Machine-readable policy with jurisdictional profiles
      - Recommendation
    * - Agent identity, delegation and authorization
      - Credential format, delegation semantics, revocation and lifecycle
      - Recommendation
    * - Testing, evaluation and red-teaming
      - Benchmarks and adversarial methods for agentic systems
      - Technical Report
    * - Assurance, conformity and regulatory mapping
      - Mapping guardrail evidence onto regulatory duties
      - Technical Report
    * - Assessment of guardrail effectiveness
      - Common assessment factors for guardrail effectiveness and a reporting format
      - Technical Report
    * - Cyber-physical limits and fail-safe behaviour
      - Deterministic limit enforcement for actuating agents
      - Recommendation
