Appendix II Guardrail categories mapped to behavioural risks
============================================================


Guardrail categories mapped to behavioural risks are given in Table 4. The assessment factor categories against which the effectiveness of these guardrail categories is assessed are set out in clause 11.

.. list-table:: Table 4 - Guardrail categories mapped to behavioural risks
    :header-rows: 1
    :widths: 37 17 15 31

    * - Guardrail category
      - Primary locus
      - Preventive / detective / corrective
      - Main risks addressed
    * - Alignment and safety training (RLHF, RLAIF, constitutional AI, instruction hierarchy)
      - Model
      - Preventive
      - Unsafe or undesirable intent formation; instruction-hierarchy violations
    * - Input and context filtering, injection detection, spotlighting
      - Input/context boundary
      - Preventive
      - Prompt injection; goal hijacking; untrusted-content trust escalation
    * - Goal locking and objective pinning
      - Orchestration/planning
      - Preventive
      - Goal drift; objective substitution
    * - Task, step, token and cost budgets
      - Orchestration/planning
      - Preventive
      - Step escalation; runaway execution; resource exhaustion
    * - Action allow-lists and typed tool schemas
      - Tool/action boundary
      - Preventive
      - Tool misuse; excessive agency; malformed tool arguments
    * - Least-agency and least-privilege scoping
      - Identity/authorization
      - Preventive
      - Privilege abuse; excessive agency
    * - Confirmation gates for irreversible actions
      - Human-oversight interface
      - Preventive
      - Irreversible real-world side effects; premature autonomous action
    * - Sandboxed execution and egress control
      - Execution environment
      - Preventive
      - Sandbox escape; data exfiltration; lateral movement
    * - Memory-integrity and provenance controls
      - Memory/knowledge store
      - Preventive / detective
      - Memory poisoning; knowledge-base corruption
    * - Drift measurement against declared policy
      - Cross-cutting (control plane)
      - Detective
      - Behavioural drift; silent policy deviation
    * - Anomaly detection over action traces
      - Cross-cutting (control plane)
      - Detective
      - Unexpected tool sequences; concealment
    * - Inter-agent authentication and signed messages
      - Inter-agent communication
      - Preventive / detective
      - Insecure inter-agent communication; multi-agent contagion
    * - Circuit breakers, kill switches, rollback
      - Orchestration / runtime
      - Corrective
      - Ongoing harmful action; unauthorized state change
    * - Structured forensic logging and accountability manifests
      - Cross-cutting
      - Corrective
      - Non-repudiation and accountability gaps; forensic reconstruction
    * - Human oversight (in/on/out-of-the-loop) selection and monitoring
      - Organizational / oversight interface
      - Corrective
      - Automation bias; ineffective oversight
    * - Unified runtime middleware hooks
      - Agent execution pipeline (cross-cutting)
      - Preventive
      - Enables all of the above at one enforcement substrate
