.. raw:: latex

    \pagestyle{plain}
    \thispagestyle{plain}
    %% Suppress all TOC entries for the front matter (sections, subsections and
    %% any addcontentsline) — restored just before \sphinxtableofcontents so the
    %% numbered clauses appear in the TOC normally.
    \let\osiaorigaddcontentsline\addcontentsline
    \renewcommand{\addcontentsline}[3]{}

.. _front-matter:

Analysis of behavioural guardrails for agentic AI: safety and security for cyber-physical systems
=================================================================================================

.. only:: html

    **Draft Technical Report TR.aai-bguard-cpsec — Analysis of behavioural guardrails for agentic AI: safety and security for cyber-physical systems**

.. raw:: latex

    \begin{center}\textbf{Draft Technical Report TR.aai-bguard-cpsec — Analysis of behavioural guardrails for agentic AI: safety and security for cyber-physical systems}\end{center}

Abstract
--------

This document proposes a new Technical Report on analysis of behavioural guardrails for agentic AI from the system side, with emphasis on safety and security for cyber-physical systems.

Background and proposal
-----------------------

Agentic AI is shifting the paradigm from advising to acting. While generative AI produces content for human interpretation, agentic AI plans, invokes tools, and executes actions. When deployed in cyber-physical systems (CPS) like industrial control, robotics, or vehicles, the agent's output is consumed directly by actuators. This transition fundamentally changes the safety and security landscape. Actions produce physical and sometimes irreversible effects. Errors compound across multi-step plans, and a single compromised instruction can move from altering text to moving a physical machine. The object of protection here is the external cyber-physical system, not the AI model itself.

The necessity of this study arises from the limitations of current practices. Traditional model-level alignment and content filtering are probabilistic and cannot guarantee that an action falls within safe bounds. In a CPS, the control loop imposes strict timing constraints; a guardrail that cannot reach a deterministic verdict within the deadline is not available at all. Furthermore, security threats evolve: prompt injection becomes action injection, and a compromised agent can inherit broad delegated authority to harm the physical plant. Existing functional safety regimes (e.g., IEC 61508, ISO 26262) assume deterministic controllers, not autonomous agents. There is a clear gap between high-level regulatory duties and the enforceable technical controls needed to bound agent behaviour in physical environments. Current industry practice is fragmented, with identity and authorization controls relatively mature but behavioural conformance checking remaining immature.

To address these gaps, this Technical Report analyzes the technical mechanisms of system-facing guardrails and the attack surfaces they must address. It categorizes attack surfaces into twelve groups (AS-01 to AS-12), covering injection, tool abuse, memory poisoning, supply chain compromise, and physical consequences. It also defines a system-facing behavioural risk taxonomy (RSK-01 to RSK-16) and establishes ten assessment factor categories (AF-01 to AF-10) along with their collection methods to judge guardrail effectiveness.

The main research direction is to propose a general, vendor-neutral assessment framework. This framework introduces five enforcement layers ordered by proximity to the physical boundary, explains trust boundaries, and emphasizes the "rule of proximity"—the principle that authoritative control must sit closer to the physical boundary as the consequence of failure rises. It addresses integration with the existing functional safety case, ensuring the agentic layer is treated as additional rather than substitutive. Most importantly, it specifies that for actuating agents, enforcement must be deterministic, in-deadline, and fail-closed.

Finally, the Technical Report identifies standardizable elements, including terminology, reference architecture, machine-readable guardrail policies, drift measurement, identity and delegation, testing methodologies, and unified runtime middleware hooks. The core conclusion is that safety is not achieved by making the model more reliable, but by placing external, independent, and mostly deterministic controls at every trust boundary the agent crosses. This Technical Report provides a structured foundation for standards developers, integrators, and regulators to secure agentic AI in cyber-physical systems.

Summary
-------

Agentic AI is moving from generating content to taking actions in the physical world. When an autonomous agent controls a cyber-physical system—such as a robot, vehicle, or industrial plant—its actions have real-world, often irreversible consequences. This creates two distinct problems. Behavioural safety concerns harm caused by an agent that is not under adversarial control, while behavioural security concerns harm caused when an adversary compromises the agent, its tools, or its supply chain and turns its actions against the physical plant. Traditional content filters, model alignment, and text-based guards cannot constrain physical actions. In a control loop, a guardrail that is probabilistic or too slow to meet the deadline is effectively useless. Existing functional safety frameworks do not account for autonomous, learning-based agents, leaving a critical gap between regulatory duties and implementable technical controls. A technical report is needed to analyze system-facing risks, examine how guardrails work and fail, and propose a vendor-neutral framework for deterministic, in-deadline, and fail-closed enforcement that protects the external cyber-physical system.

The Technical Report characterizes system-facing risks, classifies attack surfaces, analyzes guardrail limits, presents a risk taxonomy and assessment factors, and proposes an assessment framework covering enforcement layers, trust boundaries, and integration with functional safety, along with standardizable elements.

History
-------

.. list-table::
    :widths: 10 26 18 14 32

    * - Version
      - Document
      - Approval
      - Focus Group
      - Working Group

To access this Document as a PDF, open the published documentation site in your web
browser and click the PDF download link, or navigate directly to the PDF target of
this repository.

Authors
-------

.. list-table::
    :header-rows: 1
    :widths: 30 40 30

    * - Name
      - Affiliation
      - Contact
    * - Xiongwei Jia
      - China Unicom
      - jiaxw9@chinaunicom.cn

Keywords
--------

Agentic artificial intelligence (AI), behavioural guardrails, cyber-physical system, safety, security

Foreword
--------

Agentic AI systems now act as autonomous entities capable of reasoning, planning, and
executing tasks across services, infrastructures, and organizational boundaries. This
fundamentally changes the nature of identity in digital systems: identity is no longer
limited to human users and static machine instances but must also encompass autonomous,
dynamic, and goal-oriented agents. And identity alone is insufficient — the core question
extends beyond "what an entity is" to "whether, and under what conditions, that entity
should be trusted to act".

The Focus Group on Trust and Identity for humans and agentic AI (FG-TIDA) addresses trust
management and interoperable digital identity infrastructure for humans and for agentic AI,
supporting the development of secure, trustworthy digital ecosystems in which humans and
agentic AI can safely interact and collaborate.

FG-TIDA was established under Recommendation ITU-T A.7 (Focus groups: Establishment and
working procedures), with ITU-T Study Group 17 as its parent group. Further details are
available in the group's terms of reference.

For more news and updates about the TIDA Community, including its supporter organizations,
please refer to the community's published resources.

Note
----

*[Editor: add any note on the status of this Technical Report.]*

.. Intellectual Property Rights — section commented out. To restore it, remove the
   leading ".." and the indentation below. Do not put a ".. raw:: latex" block with
   "\osiacenternextsection" before the heading: the front matter ends with chapters,
   so the patch stays pending and is consumed by the first \section, leaving clause 1
   centred and unnumbered.

   Intellectual Property Rights
   ----------------------------

   ITU draws attention to the possibility that the practice or implementation of this
   Recommendation may involve the use of a claimed Intellectual Property Right. ITU takes no
   position concerning the evidence, validity or applicability of claimed Intellectual Property
   Rights, whether asserted by ITU members or others outside of the Recommendation development
   process.

   As of the date of approval of this Recommendation, ITU had not received notice of intellectual
   property, protected by patents/software copyrights, which may be required to implement this
   Recommendation. However, implementers are cautioned that this may not represent the latest
   information and are therefore strongly urged to consult the appropriate ITU-T databases
   available via the ITU-T website at http://www.itu.int/ITU-T/ipr/.
   Implementers should also be aware that the organization that originated the technically
   equivalent document listed in the Bibliography may have received notices of intellectual
   property required for the implementation of this Recommendation.

.. raw:: latex

    %% Copyright notice commented out. To restore, uncomment the lines below.
    %% \vspace{4em}
    %% \begin{center}\textcopyright{} ITU 2026\end{center}
    %%
    %% \medskip
    %% All rights reserved. No part of this publication may be reproduced, by any means whatsoever, without the
    %% prior written permission of ITU.

    %% Restore \addcontentsline so the TOC below (and Chapter 1 onward) populate.
    \let\addcontentsline\osiaorigaddcontentsline
    \sphinxtableofcontents
    \osiamainmatter
    %% Clauses are \section under an unnumbered chapter, so LaTeX would print
    %% them 0.1, 0.2, ...  Number them from 1, and restart the counter here:
    %% the unnumbered front-matter subsections step it before this point.
    \makeatletter
    \renewcommand{\thesection}{\arabic{section}}
    \renewcommand{\thesubsection}{\thesection.\arabic{subsection}}
    \renewcommand{\thesubsubsection}{\thesubsection.\arabic{subsubsection}}
    \makeatother
    \setcounter{section}{0}

.. toctree::
    :maxdepth: 3
    :numbered:
    :hidden:

    chapters/01_scope
    chapters/02_references
    chapters/03_definitions
    chapters/04_abbreviations
    chapters/05_conventions
    chapters/06_overview
    chapters/07_current-state
    chapters/08_development-trends
    chapters/09_technical-analysis
    chapters/10_risk-taxonomy
    chapters/11_assessment-factors
    chapters/12_assessment-framework
    chapters/13_standardization-points
    chapters/14_conclusion

.. toctree::
    :maxdepth: 3
    :hidden:

    chapters/15_appendix-i_key-terms
    chapters/16_appendix-ii_guardrail-mapping
    chapters/17_appendix-iii_events
    chapters/18_appendix-iv_instruments
    chapters/19_bibliography
    chapters/20_annex-a_justification
