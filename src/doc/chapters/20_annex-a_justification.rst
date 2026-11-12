.. _chapter-annex-a-justification:

Annex A Justification for the proposed new Technical Report
===========================================================

This annex provides the justification for the proposed new Technical Report TR.aai-bguard-cpsec, in the form required for a proposed new deliverable in FG-TIDA.

.. list-table:: Justification form
    :header-rows: 1
    :widths: 30 70

    * - Field
      - Content
    * - **Working Group/Task Group**
      - WG2
    * - **Work item type**
      - Proposed new Technical report
    * - **Meeting Venue/Time**
      - Paris, 30 November – 1 December 2026
    * - **Reference and title**
      - TR.aai-bguard-cpsec “Analysis of behavioural guardrails for agentic AI: safety and security for cyber-physical systems”
    * - **Base text**
      - XXX
    * - **Target date of completion**
      - 12-2027
    * - **Editor(s)**
      - Xiongwei Jia, China Unicom, jiaxw9@chinaunicom.cn
    * - **Approval process**
      - Agreement
    * - **Purpose and scope (Define what this document will address and its intent or objectives in order to indicate the limits of its applicability)**
      - This Technical Report analyses behavioural guardrails for agentic AI from the system side, with emphasis on safety and security for cyber-physical systems. It addresses the risks created when an autonomous agent acts on tools, application programming interfaces, data, other agents and physical plant, and the controls by which those risks can be bounded. The object of protection is the external cyber-physical system, not the agent, its model or its provider's infrastructure. Safety concerns harm to persons and physical assets from agent action; security concerns adversarial compromise of the agent or its supply chain converted into harm to the external cyber-physical system. The report does not address protection of the agent itself, except where that protection bounds the risk the agent poses.

        The scope includes:

        - The characterisation of system-facing behavioural risks, and of the attack surfaces through which they arise;
        - The current state of the art, industrial practice and reported results;
        - The development trends expected to shape the next three to five years;
        - The assessment factors and the collection methods by which the effectiveness of a guardrail deployment is judged;
        - A general, vendor-neutral assessment framework for behavioural guardrails in agentic AI for cyber-physical systems, including deterministic, in-deadline and fail-closed enforcement;
        - The elements that appear standardizable.

        The Technical Report is addressed to standards developers, integrators, operators and regulators concerned with agentic AI in cyber-physical systems.
    * - **Summary (provides a brief overview of the proposal)**
      - Agentic AI is moving from generating content to taking actions in the physical world. When an autonomous agent controls a cyber-physical system—such as a robot, vehicle, or industrial plant—its actions have real-world, often irreversible consequences. This creates two distinct problems. Behavioural safety concerns harm caused by an agent that is not under adversarial control, while behavioural security concerns harm caused when an adversary compromises the agent, its tools, or its supply chain and turns its actions against the physical plant. Traditional content filters, model alignment, and text-based guards cannot constrain physical actions. In a control loop, a guardrail that is probabilistic or too slow to meet the deadline is effectively useless. Existing functional safety frameworks do not account for autonomous, learning-based agents, leaving a critical gap between regulatory duties and implementable technical controls. A technical report is needed to analyze system-facing risks, examine how guardrails work and fail, and propose a vendor-neutral framework for deterministic, in-deadline, and fail-closed enforcement that protects the external cyber-physical system.

        The Technical Report characterizes system-facing risks, classifies attack surfaces, analyzes guardrail limits, presents a risk taxonomy and assessment factors, and proposes an assessment framework covering enforcement layers, trust boundaries, and integration with functional safety, along with standardizable elements.
    * - **Relations to ITU-T Recommendations, FG-TIDA Deliverables, or other documents (approved or under development)**
      - ITU-T XSTR.magAI-behavior, X.sr-taimas, X.aaia-sec, X.Max-trust
    * - **Liaisons with other study groups or with other standards bodies**
      - ITU-T SG2, SG11, SG13, SG17, SG20, SG21, FG-EAI
    * - **Supporting organizations that are committing to contributing actively to the work item**
      - China Unicom, CICT China, ZTE Corporation
