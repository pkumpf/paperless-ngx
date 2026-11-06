# Project Knowledge Base – Paperless-ngx (owner's fork)

> **Scope:** Decisions, verified findings, open questions and working practices for the owner's Synology deployment. This is **fork-specific** project knowledge, **not upstream Paperless-ngx documentation**.
>
> Last reviewed: 2026-11-06. Update this file whenever a decision changes or a meaningful new fact is confirmed.

## Goal and acceptance criteria

Build a reliable, efficient, low-maintenance document management system using the **existing Paperless-ngx capabilities first**.

Success means:

- Documents are imported, OCR-processed and safely retained.
- Documents can be found easily using Paperless UI/app and search.
- Metadata (e.g. title, correspondents, document types, tags) is as accurate and automatic as practical.
- Questions about stored documents can be answered accurately, with references to source documents, if supported by the deployed version and configured models.
- The installation remains straightforward to back up, restore and upgrade.

**Explicitly not a goal:** Recreating a category-based directory tree for Synology DS File. Documents may remain in a single folder or Paperless-managed storage. Storage paths, folder names, and storage-path classification are **not project acceptance criteria**. Paperless UI/app is the primary access method. Do not create new storage-path rules or restructure files just for DS File. Do not delete or migrate existing data, paths or workflows solely because of this decision; first inspect dependencies and backups.

Useful filenames are welcome, but they must not drive extra storage architecture at the expense of reliability and simplicity.

## Standing architecture decisions

| ID    | Decision                                                                                                                                                               | Reason / consequence                                                                                  |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| D-001 | **Native first**: verify standard Paperless functionality, configuration and supported integration points before new code or infrastructure.                           | Avoid duplicate systems, cost and maintenance.                                                        |
| D-002 | External AI/embedding endpoints are acceptable _only when needed_ for an otherwise native workflow; compare local and hosted options for cost, privacy and simplicity. | Native Paperless integration is the goal; not a blanket ban on model inference outside the container. |
| D-003 | Qdrant, independent chunking pipelines and custom vector databases are **not assumed requirements**.                                                                   | Reconsider only after proving that native indexing/retrieval cannot meet a concrete need.             |
| D-004 | Physical DS File folder layout and storage-path categorization are **not required**; use Paperless UI/app.                                                             | Optimize for finding and using documents, not a parallel filesystem taxonomy.                         |
| D-005 | Work on a separate branch and PR; keep personal deployment decisions out of upstream PRs.                                                                              | Changes remain reviewable, recoverable and independently testable.                                    |
| D-006 | Reassess assumptions **at every meaningful milestone**, not only when asked.                                                                                           | Prefer evidence and the smallest working solution over an inherited plan.                             |

## Known context versus verification needed

**Historical context (do not interpret as a live inspection):**

- Synology DS718+ with Docker is the target installation.
- Existing import/OCR workflows and automatic metadata have been discussed and tested.
- Earlier experiments indexed documents using a Qdrant-based solution; experiments do not establish a production requirement.
- An AI classification contribution, [upstream PR #14462](https://github.com/paperless-ngx/paperless-ngx/pull/14462), was **closed without merge** on 2026-10-15. The maintainers attributed the reported tool-call omission to model/provider behavior.
- The fork's `docs/advanced_usage.md` documents an integrated **LLM index / document chat** using a configured embedding backend. It says Paperless manages the index, updates it on a schedule, and can link to source documents in chat. This describes source-branch capability, **not proof that the running Synology instance has it enabled or is on that version**.

**To verify before implementation:** exact running Paperless version and container image, existing Docker Compose environment (without copying secrets), active settings/workflows, actual index/chat availability, backups and restore procedure. Do not assert a live version from old conversation notes; version reports have differed.

## Delivery workflow

1. At the start of every task, state the **user-visible outcome** rather than start with a technology.
2. Check the exact deployed version, current official documentation, upstream issues/PRs and (when useful) similar users' experience. Community reports inform hypotheses but are not sufficient evidence on their own.
3. Ask critically: Is this already supported natively? Is there a simpler workflow? Are we duplicating index/storage/classification? Does the proposed change make backups or upgrades harder? Are we solving a real observed problem?
4. Record findings and source links in the relevant PR or here. Distinguish **confirmed**, **hypothesis** and **not yet checked**.
5. Implement only the smallest justified change, with tests or a reproducible verification step. Use fork-only branches and PRs; do not alter production documents or deployments without a verified rollback path.
6. Re-evaluate after a milestone or a failed test. Revise the architecture if evidence changes. Update this knowledge base and `AGENTS.md` when appropriate.

## Next concrete milestone: native capability audit

A **read-only** inspection of the Synology installation and the matching documentation:

- Identify actual image/version and configuration flags relevant to built-in OCR, automated metadata, workflows, search, LLM index and document chat.
- Inventory what is already active, unused, redundant or missing; do not mistake GitHub `dev` capabilities for deployed capabilities.
- Determine whether storage-path classification rules can simply be **left unused**; remove or change rules only after identifying any reliance on them.
- Recommend a minimal configuration-only path to test native retrieval / chat with traceable sources. Document embedding-model dependencies and resource implications for the DS718+.
- Define small tests for import, metadata, search, RAG grounding / source links, and fallback behavior. Avoid introducing new databases by default.

**Stop condition:** If native Paperless does not support a required feature on the deployed version, explicitly compare _upgrade / built-in extension / external component_ before writing custom code.

## Decision and observation log

| Date       | Item                                                                                              | Status                                     |
| ---------- | ------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| 2026-10    | Native-first architecture already recorded in `AGENTS.md`.                                        | Confirmed in fork                          |
| 2026-11-06 | DS File directory taxonomy and storage-path classification removed from project objectives.       | Decision                                   |
| 2026-11-06 | Native LLM index and document chat documented on fork `dev`; deployment still needs verification. | Documentation verified; live state unknown |
| 2026-11-06 | Code-implementation responsibility temporarily moves from Copilot to ChatGPT acting via GitHub.   | Working agreement                          |
