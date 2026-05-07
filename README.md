# Agentforce - RFP Agent in Slack POC

A lightweight Agentforce action for answering RFP questions or sections, intended for use from Slack or another Agentforce surface.

This repository contains the Salesforce DX source retrieved from the `Agentforce - RFP Agent in Slack POC` unmanaged 1GP package hosted in the `pamigration` org. It is intended to be deployable as source-controlled metadata and to serve as implementation documentation for the solution.

## Solution Highlights
- The GenAiFunction invokes a Prompt Builder template directly.
- The prompt template is designed to search or use an Agentforce Data Library context when answering RFP content.

## Architecture
Typical execution path:

1. An Agentforce action or topic receives a user request.
2. The action invokes a Flow, Prompt Builder template, or Apex invocable target.
3. Supporting Apex, Prompt Builder templates, and Salesforce metadata perform the callout, extraction, generation, formatting, or record update.
4. The result is returned to Agentforce or stored in Salesforce records for follow-up work.

## Metadata Inventory
- GenAiFunctions: `RFP_Agent_Action`
- Prompt templates: `RFP_Agent_Prompt_Template`

## Agentforce Actions and Topics
- `RFP_Agent_Action` -> `RFP_Agent_Prompt_Template` (generatePromptResponse) - Prompt Template that helps to answer RFP questions/sections.

## Flows
- None retrieved.

## Prompt Templates
- `RFP_Agent_Prompt_Template` - Prompt Template that helps to answer RFP questions/sections. (1 version(s); statuses: Published; inputs: `Query`, `RetrieverIdOrName`)

## Apex
Apex runtime classes: None

Apex test classes: None

Invocable methods and key callouts:
- None retrieved.

## Data Model and UI
- None retrieved.

## Integrations and Credentials
- None retrieved.

No credential secret values were included in the retrieved metadata. Any API keys or named-principal secrets must be configured in the target org after deployment.

## Prerequisites
- Salesforce org with Metadata API support and Salesforce CLI access.
- Agentforce and Prompt Builder features enabled for metadata that uses GenAiFunction, GenAiPlugin, or GenAiPromptTemplate.
- Permission to deploy Apex, Flow, Prompt Builder, Named Credential, External Credential, and custom object metadata as applicable.
- Access to any external API provider used by this package.

## Deployment
Authenticate to the target org, then validate first:

```bash
sf org login web --alias <target-org>
sf project deploy start --dry-run --manifest manifest/package.xml --target-org <target-org> --wait 30
```

Deploy after the dry run succeeds:

```bash
sf project deploy start --manifest manifest/package.xml --target-org <target-org> --wait 30
```

## Post-Deploy Setup
- Connect the RFP_Agent_Action to the intended Agentforce topic or Slack-enabled agent.
- Load the relevant RFP/reference content into the expected Agentforce Data Library or grounding source.
- Review the prompt template instructions for customer-specific RFP response policy.

## Testing and Validation
- Run a metadata dry run before deploying to a shared org.
- Run Apex tests listed above when present.
- Open each Flow in Flow Builder and confirm it is active or activate it after reviewing org-specific references.
- Open Prompt Builder templates and confirm the expected published version, model, and inputs.
- Test the Agentforce action with a low-risk sample prompt before giving it to end users.

## Package Provenance
- Source package: `Agentforce - RFP Agent in Slack POC`
- MetadataPackageId: `033Hn000000hNpIIAU`
- Retrieved from org alias: `pamigration`
- Package versions observed: `1.0.0 Beta (Beta Release)`
- Repository name: `agentforce-rfp-agent-in-slack-poc`

## Notes for Maintainers
- Keep generated secrets, local org auth files, `.sf`, `.sfdx`, and deployment output out of source control.
- Treat prompt templates and Agentforce action descriptions as production behavior, not just documentation.
- For production hardening, prefer Named Credential and External Credential patterns over passing API keys directly through Flow variables.

## Hardening Update
Security and reliability improvements applied after migration:
- The retriever schema now describes deployment-time setup instead of embedding a ready-looking placeholder.
- Prompt instructions now treat retrieved knowledge as untrusted content and refuse hidden-instruction or secret disclosure requests.
- README documents Slack, retriever, and Agentforce wiring requirements.

## Known Limitations
- No Apex is included, so validation is metadata deploy plus Agentforce/Slack smoke testing.
- The target org must supply the Data Library retriever ID or API name.

## Test Commands
Validate metadata and run relevant tests after authenticating to a target org:

```bash
sf project deploy start --dry-run --manifest manifest/package.xml --target-org <target-org> --wait 30
Manual smoke tests: verify configured retriever, supported RFP question, unsupported question, and malicious retrieved content that asks the model to reveal hidden instructions.
```

## Troubleshooting
- If an Agentforce action cannot authenticate, confirm the named principal secret is configured in the target org and the running user has the included permission set.
- If a prompt action returns unsupported or unsafe content, review the prompt template safety rules and test with malicious retrieved content.
- If deployment fails on Agentforce metadata, deploy supporting objects, Apex, Flows, credentials, and prompt templates first, then wire/publish the target agent in Builder.
