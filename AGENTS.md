# Agent instructions for this fork

These instructions govern decisions for the owner's Paperless-ngx deployment and related work in this fork. They are **not** proposed requirements for the upstream Paperless-ngx project.

Read and keep [PROJECT_KNOWLEDGE_BASE.md](PROJECT_KNOWLEDGE_BASE.md) up to date; it contains the current goals, decisions, verified findings and open questions. Do not treat historical notes as live configuration facts.

## Architecture principle: native first

1. **Use what is already available.** Before proposing or implementing any feature, check whether the installed Paperless-ngx version or existing infrastructure already provides it through standard functionality, configuration, APIs, or supported extensions.
2. **Verify capabilities, do not assume them.** Check the actual deployed version and current official documentation, then validate important claims with a small test. Do not assume that an upstream feature is present in the installed release.
3. **Introduce additional technology only for a demonstrated gap.** External services, databases, plugins, separate indexing pipelines, and custom code require a specific unmet requirement. Explain the native alternative, the gap, and the cost in setup, maintenance, upgrades, and reliability before recommending them.
4. **Prefer the smallest maintainable solution.** Minimize components, configuration, manual work, and ongoing operating costs. Favor simple upgrades, backups, and understandable failure modes.
5. **Keep the user goal in focus.** Priorities are reliable PDF ingestion and OCR, meaningful automatically generated document titles/filenames, searchable content, and accurate document-grounded question answering (RAG) with source references. Paperless UI/app access is sufficient. A separately organized DS File view, category-based directory tree, storage-path taxonomy, and automatic storage-path classification are **not** goals. Documents may be stored together in Paperless-managed storage. Do not delete existing folders or storage rules without checking dependencies and backups.
6. **Re-evaluate existing prototypes.** Qdrant, embedding services, custom indexers, and other previously tested components are not mandatory architecture choices. Keep or extend them only if verified built-in options cannot meet the requirements.
7. **Protect upstream contributions.** Do not mix personal deployment decisions or additional components into unrelated upstream pull requests.
8. **Challenge the approach independently and repeatedly.** Before each implementation and at meaningful milestones, assess whether the feature is still necessary, already available, achievable with configuration, or better addressed by an upgrade. Consult current documentation, relevant upstream issues and field experience; validate before relying on third-party claims. Proactively challenge your own previous design decisions.
9. **Work with evidence and PRs.** Clearly distinguish deployed facts, upstream/documented capabilities and hypotheses. Make the smallest change on a dedicated branch, verify it, record conclusions in the project knowledge base, and open a PR. Do not confuse coding access with deployment access.

**Decision rule:** As much supported standard functionality as possible; as little custom development as necessary.
