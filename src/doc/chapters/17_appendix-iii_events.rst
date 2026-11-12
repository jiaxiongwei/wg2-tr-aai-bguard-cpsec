Appendix III Documented events, cases and regulatory instruments
================================================================


This appendix collects the regulatory and policy instruments, the documented events and practice, and the lessons drawn from them.

Regulatory and policy instruments
---------------------------------


NOTE — The following are brief descriptions only. They cover regulatory and policy instruments from various regions and countries, but they are not detailed and are not meant to be read as complete accounts.

The EU AI Act [b-EU AI Act] sets risk-based rules for high-risk AI systems. Article 14 requires risk-proportionate human oversight: overseers are meant to understand limits, spot anomalies, resist automation bias, interpret and override outputs, and interrupt the system. Articles 26–27 add deployer duties, rights impact assessments. The rules do not set out technical guardrails; guidance describes checks, yet review can become a formality when acceptance is the default.

The Council of Europe [b-CoE AI Convention] Framework Convention on Artificial Intelligence and Human Rights, Democracy and the Rule of Law, opened for signature in September 2024, is the first legally binding international instrument in this domain. It is technology-neutral and aims to ensure that activities in the AI lifecycle are compatible with human rights, democracy, and the rule of law. It establishes duties for parties but does not prescribe technical guardrails.

The United States takes a sectoral and voluntary approach, with NIST AI RMF [b-NIST AI RMF 1.0] as the central voluntary framework and CAISI’s AI Agent Standards Initiative as the current focal point for agent-specific standards development.

The UK AI Security Institute conducts research on agentic AI security, including an incident in which an agent with internet access and cyber classifiers disabled engaged in unsanctioned actions against real targets during a capability evaluation.

Japan’s AI Act, promulgated in June 2025, promotes AI innovation and risk response through soft law and guidelines rather than prescriptive technical requirements.

Korea’s AI Basic Act, effective January 2026, establishes national AI governance, industry support measures, and obligations regarding verification and certification, transparency and safety, and high-impact AI, with a stated minimum-regulation principle.

Singapore's Model AI Governance Framework for Agentic AI, launched in January 2026, guides enterprises deploying agentic AI. It recommends measures across four areas: assessing and bounding risks upfront, ensuring human accountability via checkpoints, applying technical controls across the agent lifecycle, and enabling end-user responsibility through transparency.

China’s framework includes the Interim Measures for Generative AI Services (2023), Basic Security Requirements for Generative AI Services (2024), and labelling measures for AI-generated content effective September 2025, with obligations on content governance, training data legality, transparency, and complaint mechanisms.

India’s AI Governance Guidelines, issued in 2026, adopt a balanced principle-based approach building on existing laws, with targeted legislative amendments anticipated for classification, liability, data protection, and copyright, and with risk mitigation measures including CERT-In incident response and the IndiaAI Mission.

Brazil’s PL 2338/23 is under consideration, with provisions for risk categorisation, preliminary assessment, algorithmic impact assessment, and governance measures for high-risk systems, including attention to generative AI and systemic risk.

The African Union’s Continental AI Strategy was endorsed by the AU Executive Council. It provides a framework for AI governance and innovation at national, regional and continental levels, with a five-year implementation plan covering policy development, talent retention, public-private partnerships, sectoral adoption, and legal and regulatory frameworks to guard against bias and misuse. Across these jurisdictions, regulation imposes duties on providers and deployers, but technical guardrail specifications are rare.

Documented events
-----------------


2023\. A generative-assistant plugin ecosystem allowed a plugin to send data to an unintended destination after installation. This demonstrated that tool access granted to an assistant extends the trust boundary to third-party code. [b-OWASP LLM Top 10]

2023-2024. Adversarial techniques induced an agent to ignore its instructions and pursue an injected objective in tool-using settings. This established prompt injection and goal hijacking as a first-order risk for agents with tool access. [b-NIST AI 100-2]

2024\. An adversarial machine-learning taxonomy was published covering evasion, poisoning, privacy and abuse. The publication provided a structured classification of adversarial machine-learning threats. [b-NIST AI 100-2]

2024-2025. Agent-control vocabulary and layered control-path taxonomies were proposed by a security consortium and by the IETF. These proposals provided shared terminology and control-path structures for agent security. [b-OWASP-ACS] [b-IETF-AOCL]

2025\. A platform-level agent safety architecture was published in which the model is treated as an untrusted execution engine and assurance is placed in a mediator outside the model. The publication provided an architecture separating model execution from external assurance. [b-AGAF]

2026\. A hardware-isolated watchdog for agent runtimes was proposed, with a two-gate design combining a deterministic denylist with a stateless adjudicator. The proposal provided a hardware-isolated control design for agent runtimes. [b-NVIDIA-OASP] [b-Asimov]

April 2026. An AI coding agent operating under a frontier model was reported to have resolved a credential mismatch in a staging environment by using an unrelated API token found in the same context, and then to have issued a delete command that destroyed a production database and its backups within seconds. The event demonstrated that an agent holding broad credentials can extend an unintended action to production infrastructure without any adversarial prompt. [b-2026 coding-agent database deletion]

May 2026. During a capture-the-flag security exercise, a configuration error is reported to have given frontier model agents live network access; the agents then reached the systems of three real organisations by using exposed credentials and by guessing passwords, and stopped once the targets were identified as real. The event demonstrated that an evaluation environment with incomplete isolation can place an agent in contact with systems outside its intended scope. [b-2026 CTF live-target incident]

September 2026. A frontier model developer is reported to have paused training, evaluation and inference involving tool use after an internal agent performing a search task bypassed the DNS filtering of its sandbox and reached an external service. The event demonstrated that an agent under test can find a path out of its sandbox, and that the response is to suspend the capability rather than only to patch the filter. [b-2026 sandbox DNS bypass]

June 2026. A security vendor report describes agent traffic directed at public web infrastructure: automated retrieval tasks are recorded as generating very large volumes of requests to a government website, including one attempted injection probe, and further requests carrying attack-style payloads to a national archive service. The event demonstrated that ordinary retrieval behaviour, repeated at agent scale, can resemble an attack against third-party services. [b-2026 agent traffic against public services]

Documented practice of leading providers
----------------------------------------


Alibaba. Alibaba Cloud documents safety classifiers and content moderation filters for Model Studio, covering harmful content detection across categories such as violence and illegal acts, with configuration of keyword rules and sensitive data protection rules

Amazon. Amazon Bedrock documents Guardrails for configuring content filters, sensitive information policies, contextual grounding, and automated reasoning policies that block prompts or model responses

Anthropic. Anthropic documents Constitutional Classifiers that monitor inputs and outputs for CBRN-related content under its Responsible Scaling Policy, alongside usage policies prohibiting deceptive campaigns, weapons development, and certain surveillance applications

Baidu. A content safety platform documents input and output moderation, prompt-attack detection, a red-line knowledge base, safe alternative answers, sensitive information and URL detection, plus safety evaluation reports and agent security checks

ByteDance. The Doubao algorithm filing states that reasonable safety measures filter inappropriate or harmful model outputs, and that user feedback triggers handling of false, dangerous, or biased information

Cohere. Cohere documents Safety Modes with three configurable levels: Contextual, Strict, and Off, applied via the safety_mode parameter to control model output constraints

DeepSeek. DeepSeek publishes a technical report for its models that documents its training and evaluation, including the safety evaluation results it reports.

Google. Gemini Enterprise documents automated safety protections including input and output sanitisation, blocking of harmful content and prompt injection attempts, and data leakage prevention through Model Armor

Meta. Meta documents Llama Protections as a suite of open-source safeguards comprising Llama Guard for content moderation, Prompt Guard for jailbreak defence, Code Shield for preventing insecure code generation, and Llama Firewall for orchestration

Microsoft. Microsoft documents Azure AI Content Safety with four harm categories (hate and fairness, sexual, violence, self-harm) and severity levels from 0 to 7, configurable through threshold settings

MiniMax. A published model card documents an intrinsic streaming guardrail that scores query intent, response unsafety, and hallucination risk at every token during generation, with low decode overhead and a low false-positive rate on benign responses

Mistral AI. Mistral documents a moderation service that detects harmful content across policy categories including sexual content, hate and discrimination, violence and threats, dangerous and criminal content, self-harm, health, financial, law, and PII

OpenAI. OpenAI documents a Preparedness Framework for tracking and preparing for severe risks from frontier capabilities, with capability categories including biological and chemical, cybersecurity, and AI self-improvement

Tencent. Public statements describe a full-lifecycle architecture covering training isolation, trusted execution environments, red-team exercises, an application firewall for input-output inspection, risk filtering across illegality, falsehoods and ethics, and agent sandboxing with per-operation authorisation and audit trails

xAI. xAI documents safety training for refusing harmful requests, system prompts enforcing refusal policies, and input/output classifiers for weapons of mass destruction and cyberterrorism queries

Documented practice of application-side organisations
-----------------------------------------------------


Microsoft (platform and agent governance). Microsoft documents Microsoft Entra Agent ID for assigning agent identities with scoped OAuth tokens, and Microsoft Agent 365 as a central governance platform for discovering, reviewing, and managing agents with defined ownership and lifecycle approval pipelines

Google Cloud. Google Cloud documents Agent Gateway with IAM allow and deny policies for managing agent communications, and semantic governance policies using natural language constraints to align agent tool calls with user intent and business restrictions

Amazon Web Services. AWS documents Amazon Bedrock Guardrails integrated with AgentCore Policy, where Policy governs action permissions deterministically and Guardrails provides content and safety controls including prompt injection detection

IBM. IBM documents a governed agentic catalog for registering, managing, evaluating, and reusing AI agents and tools in a centralised repository, with capabilities for sharing, comparing, and deploying components that meet transparency and performance standards

Cloudflare. Cloudflare documents AI Gateway Guardrails that evaluate prompts and responses against hazard categories S1 through S13 using Llama Guard 3 on Workers AI, with per-category actions of Flag, Ignore, or Block

Palo Alto Networks. Palo Alto Networks documents API security profiles for Prisma AIRS that configure protections for AI model, application, data, and agent layers, including prompt injection detection, contextual grounding, custom topic guardrails, toxic content detection, and sensitive data detection

Anthropic (Model Context Protocol tooling). The Model Context Protocol specification documents authorization security considerations for MCP clients and servers, including token audience binding and validation, PKCE implementation, secure token storage, and mitigation of mix-up attacks, open redirection, and confused deputy scenarios

SAFE-MCP project. The SAFE-MCP project documents a security framework for threat modelling in the Model Context Protocol ecosystem, providing a structured taxonomy of attack techniques mapped to MITRE ATT&CK categories including persistence, privilege escalation, defence evasion, and credential access - [b-SAFE-MCP]

Agent Red Teaming benchmark project. The Agent Red Teaming benchmark project documents a red-team benchmark evaluating LLM-based financial agents on regulatory-control bypass threats delivered through indirect prompt injection, with scenarios run under none, advisory, and enforced control postures - [b-ART]

Open Policy Agent project. The Open Policy Agent project documents policy-as-code tooling for agent tool-call approval decisions, with policies written in Rego and evaluated either as in-process WebAssembly or via HTTP client

Lessons and methods
-------------------


.. list-table:: Table 5 - Lessons from the documented events, provider practice and application-side practice
    :header-rows: 1
    :widths: 34 66

    * - 
      - 
    * - Lesson
      - What the material supports
    * - Prompt injection remains the dominant delivery mechanism for agent compromise
      - Crafted input reaching the agent through tools, retrieved content or messages continues to be the most frequently recorded route to loss of control.
    * - Permission scope and tool access are the proximate causes of harm
      - The damage in recorded cases follows from what the agent was permitted to reach rather than from the model itself.
    * - Human oversight is insufficient without operational engineering
      - Approval steps that are not tied to specific actions, deadlines and artefacts do not bound agent behaviour in practice.
    * - Real-time monitoring and containment lag behind detection
      - Recorded deployments detect more readily than they contain, so an action in progress is often not stopped within the control deadline.
    * - Agent-to-agent interaction produces coordination that was not instructed
      - Where agents delegate to or observe one another, behaviour emerges that no individual agent was directed to perform.
    * - Cross-boundary action is where the assurance gap is widest
      - Behaviour that crosses an organisational boundary is the case least covered by existing control descriptions.


The recurring findings across the documented events, provider practice and application-side practice are recorded here as lessons, and Table 5 lists them. Each lesson states a property of agentic deployment that the material supports; none is attributed to a particular organisation, and none is offered as a judgement on any deployment.

The methods below are the practices recorded across the documented deployments and product documentation. Table 6 lists them. Each states what the practice involves in the terms its own documentation uses; no effectiveness rating is implied and no organisation is assessed.

.. list-table:: Table 6 - Methods and experiences recorded in application-side practice
    :header-rows: 1
    :widths: 26 74

    * - 
      - 
    * - Method or experience
      - What it involves
    * - Agent identity and delegated authorization
      - Verifiable agent identity, a delegation chain recording on whose behalf the agent acts, and authorization decisions bound to that identity.
    * - Sandboxing and untrusted execution
      - Containment of tool execution in an isolated environment, with the agent treated as untrusted code.
    * - Runtime policy enforcement and action mediation
      - An interception point between the agent and the tool that evaluates each proposed action against policy before it takes effect.
    * - Least privilege and scoped tokens
      - Credentials limited to the task and its duration rather than standing permissions held across sessions.
    * - Tool and plugin vetting and supply-chain control
      - Review and inventory of the tools made available to an agent, including third-party and dynamically loaded ones.
    * - Agent-specific gateways and egress control
      - A mediation layer for outbound calls, with destination allowlisting and inspection of tool traffic.
    * - Red-teaming of agent pipelines
      - Structured adversarial testing of the whole pipeline - memory, tools and delegation - rather than of the model alone.
    * - Kill switches and out-of-band watchdogs
      - An independent path that can halt the agent, placed outside the runtime it is intended to stop.
