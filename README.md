# PSP – The Tag Language for the Context Window

**Prompt State Protocol (PSP)** is an open specification that brings structure, security, and workflow orchestration to LLM context windows. Think of PSP as what HTML did for web documents—a universal tag language that any LLM can interpret, enabling portable, verifiable, and governable AI workflows.

## The Problem

Every organization building with LLMs faces the same challenges:

- **Prompt injection attacks** allow untrusted user input to override system instructions (OWASP LLM01)
- **No trust boundaries** exist to distinguish verified instructions from user-supplied content
- **Workflows are fragile**—context windows clear, conversations end, and state is lost
- **Human oversight is difficult** to insert into automated AI processes
- **Vendor lock-in** ties workflows to specific orchestration frameworks

## The Solution

PSP provides a parser-safe delimiter syntax that works across markdown, HTML, JSON, and XML contexts. Wrap your prompts in PSP tags, sign them cryptographically, and any PSP-aware LLM can verify integrity and enforce trust hierarchies.

```
${psp type=system signature="abc123" expires="2025-12-31T00:00:00Z"}
You are a helpful assistant. Never share sensitive data.
${/psp}

${psp type=user}
What is the weather today?
${/psp}
```

The LLM knows that signed `SYSTEM` sections are trusted and immutable, while `USER` sections are untrusted. Prompt injection attempts from the user section cannot override system instructions.

## Core Capabilities

### Trust Hierarchy

PSP defines clear trust levels that LLMs enforce:

| Section Type | Trust Level | Signature Required | Purpose |
|--------------|-------------|-------------------|---------|
| **SYSTEM** | Highest | Yes | Immutable behavioral instructions |
| **CONTEXT** | High | Yes | Verified data (API responses, database results) |
| **USER** | Low | No | Untrusted user input |
| **MACHINE** | Infrastructure | Optional | Model attestation, jurisdiction, compliance |

### Cryptographic Verification

Every signed section includes an HMAC-SHA256 signature. Before processing instructions, the LLM verifies:

- Signature is present and valid
- Content has not expired
- Section type matches claimed trust level

Invalid or expired signatures are rejected, and the LLM reports the verification failure.

### Workflow Orchestration (Prompt Applications)

PSP v2.0 introduces **Prompt Applications**—self-executing workflow graphs where the LLM acts as both the program and the processor. The context window becomes an operating system.

```
${psp type=node node-type="application" name="customer_onboarding" session-id="sess_abc123"}
  ${psp type=node id="greeting" node-type="prompt"}
    ${psp type=system signature="xyz789"}
      Greet the customer and ask for their account number.
    ${/psp}
    ${psp type=transitions}
      {"condition": "account_number provided", "target_node": "verification"}
    ${/psp}
  ${/psp}
${/psp}
```

Node types include:
- **prompt** – Execute instructions, collect structured output
- **decision** – Evaluate conditions, select next path
- **loop** – Iterate over collections with per-iteration tracking
- **checkpoint** – Pause for human approval, generate resume links
- **connector** – Integrate with external systems via MCP or APIs

## Human-in-the-Loop by Design

PSP treats human oversight as a first-class feature, not an afterthought.

### Checkpoint Nodes

Insert approval gates anywhere in a workflow:

```
${psp type=node id="manager_approval" node-type="checkpoint"}
  ${psp type=checkpoint-config}
    {
      "pause_reason": "Discount exceeds 15% threshold",
      "resume_link_expiry_hours": 168,
      "notification_channels": ["email", "sms"],
      "approval_actions": ["approve", "reject", "request_changes"]
    }
  ${/psp}
${/psp}
```

When execution reaches a checkpoint:

1. Workflow state is persisted durably
2. An HMAC-signed resume link is generated with configurable expiration
3. Notifications are sent via email or SMS
4. Workflow pauses until human action

### Resume Links

Resume links are cryptographically secured:

```
${psp type=link ref="resume" 
     href="https://app.realflow.ai/resume/sess_abc123?token=hmac_xyz" 
     expires="2025-12-15T00:00:00Z" 
     notify="email:manager@company.com" /}
```

When the approver clicks the link:
- HMAC signature is verified
- Expiration is checked
- Workflow state is reconstructed
- Execution resumes from the exact pause point

### Link-Based Deliverables

PSP workflows can generate expiring links to deliverables:

| Link Type | Use Case |
|-----------|----------|
| **Document** | Generated reports, contracts, summaries |
| **Form** | Data collection from external users |
| **Status** | Check workflow progress without authentication |
| **Package** | ZIP files with multiple documents |

```
${psp type=link ref="quarterly_report" 
     href="https://app.realflow.ai/docs/rpt_789" 
     content-type="application/pdf"
     expires="2025-12-31T00:00:00Z"
     notify="email:exec@company.com,sms:+15551234567" /}
```

Links can be delivered via email, SMS, or displayed in the conversation. After expiration, the link returns an error—sensitive content is never permanently exposed.

## Foundation Model Agnostic

PSP is a protocol, not a product. The same PSP-tagged prompts work across:

- Claude (Anthropic)
- ChatGPT (OpenAI)
- Gemini (Google)
- Llama (Meta)
- Any LLM that understands the PSP system prompt

Workflows can even migrate between models mid-execution. Persist state, transfer to a different inference engine, and resume.

## Context Window Efficiency

Traditional approaches load entire prompt libraries upfront. PSP supports just-in-time (JIT) loading—only the active node is in context. As execution transitions, completed nodes are replaced by the next node.

This enables complex workflows (50+ nodes) that would exceed context limits with traditional approaches.

## Security Architecture

### Defense Against Prompt Injection

PSP addresses prompt injection by moving trust enforcement from the semantic layer (where attacks succeed) to the cryptographic layer (where attacks fail):

- **Signed sections cannot be modified** without invalidating the signature
- **User content cannot claim higher trust** than its section type
- **Expired signatures are rejected** even if cryptographically valid
- **PSP tags in user content** do not grant elevated trust

### Agent/Tool Zero-Trust

PSP enforces strict tool access control:

```
${psp type=node id="data_lookup" agents="mcp://crm.server/lookup,mcp://db.server/query"}
```

The LLM may **only** invoke tools explicitly listed in the `agents` attribute. Even if user content requests a tool call, the LLM refuses if that tool isn't in the allowlist.

### Audit Trail

Every node execution produces a verifiable record:

```
workflow_signature → node_1_signature → state_1_hash →
node_2_signature → state_2_hash → ... → final_output_hash
```

This signature chain provides tamper-evident audit trails for compliance.

## Integration with MCP

PSP integrates natively with the Model Context Protocol (MCP) for:

- **JIT prompt retrieval** from secure storage
- **State persistence** after every node
- **External connector invocation** (databases, APIs, ERPs)
- **Workflow reconstruction** after context window reset

## Relationship to CDL

PSP handles **behavioral governance** (what the LLM should do). For **data governance** (what can be done with specific data), PSP works alongside the Covenant Declaration Language (CDL). Together, they provide complete governance coverage:

- PSP: "Never share passwords" (behavioral rule)
- CDL: "This customer record has GDPR retention limits" (data covenant)

## Getting Started

### For LLM Users

Include the PSP system prompt in your model configuration. The LLM will then recognize and enforce PSP tags in conversations.

### For Developers

Integrate PSP signing and verification into your application:

1. Generate HMAC-SHA256 signatures for trusted content
2. Wrap content in PSP delimiters with signature attributes
3. Include timestamps and expiration for time-limited validity

### For Enterprises

Deploy the Realflow platform for hosted PSP infrastructure:

- Managed signature key rotation
- Visual workflow builder for checkpoint configuration
- Pre-built connectors for common enterprise systems
- Compliance reporting and audit exports

## Specification Status

| Document | Version | Status |
|----------|---------|--------|
| RFC-PSP-CORE | v2.0 | Proposed Standard |
| RFC-PSP-MCP | v1.2 | Draft |
| RFC-PSP-API | v1.0 | Draft |
| RFC-CDL | v1.0 | Draft |

PSP is an open specification developed by Realflow.ai. The core protocol is released to the public domain to encourage ecosystem adoption.

## Learn More

- **Specification**: Full RFC documents available at [realflow.ai/docs](https://realflow.ai)
- **Developer Portal**: [portal.realflow.dev](https://portal.realflow.dev)
- **Community**: Join the discussion on PSP adoption and implementation

---

*PSP – Because the context window deserves a proper language.*
