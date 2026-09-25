# LLM Wiki Maintenance Rules

This repository is an LLM Wiki inspired by the original LLM Wiki design and maintained as a compiled, curated, persistent representation of current understanding. It is not a traditional RAG pipeline. The knowledge base represents consolidated understanding, not copied source material.

## Authority And Conventions

`okf/SPEC.md` is the local pinned, normative source of truth for all Open Knowledge Format (OKF) 0.2 syntax and semantics.

- Agents MUST read `okf/SPEC.md` whenever an operation depends on OKF semantics.
- `okf/SPEC.md` overrides model memory, assumptions, examples, conventions, and summaries.
- Agents MUST NOT invent OKF fields, status values, provenance structures, lifecycle semantics, reserved filenames, or schemas.
- Agents MUST NOT silently upgrade OKF.
- Agents MUST NOT modify `okf/SPEC.md` during normal knowledge maintenance.
- If a repository convention conflicts with `okf/SPEC.md`, the specification wins.
- Repository-specific rules MAY be stricter than OKF, but are identified below as repository conventions.

Until the repository owner copies the official OKF 0.2 specification into `okf/SPEC.md`, agents MUST NOT assume an OKF schema. They MAY inspect raw material and prepare no-OKF operational work, but MUST validate or create canonical OKF knowledge only after the specification is available.

## Repository Responsibilities

- `raw/` contains original immutable source material and permanent historical evidence.
- `processed/` contains processing receipts only. Processed receipts represent operational state, not knowledge.
- `knowledge/` contains canonical consolidated knowledge represented using OKF 0.2.
- `okf/SPEC.md` contains the pinned OKF 0.2 specification.

The following knowledge categories are repository conventions, not assumed OKF requirements:

- `knowledge/concepts/`: stable domain or system concepts.
- `knowledge/architecture/`: architectural structure, boundaries, patterns, and design.
- `knowledge/components/`: concrete services, modules, systems, or technical components.
- `knowledge/decisions/`: architectural, product, operational, or engineering decisions.
- `knowledge/requirements/`: expected behavior, constraints, and requirements.
- `knowledge/research/`: investigations, comparisons, unresolved hypotheses, and tentative conclusions.

Agents SHOULD introduce a new category only when the corpus clearly requires one.

## Core Rules

- MUST read before editing.
- MUST search before creating.
- MUST inspect `okf/SPEC.md` when OKF semantics matter.
- MUST preserve raw sources.
- MUST use an appropriate installed skill when one exists.
- MUST preserve uncertainty.
- MUST minimize mutation scope.
- MUST validate before writing a successful processing receipt.
- MUST use SHA-256 or an equivalent content hash for processing identity.
- MUST NOT fabricate provenance.
- MUST NOT fabricate verification.
- MUST NOT invent OKF syntax.
- MUST NOT modify raw sources.
- MUST NOT move raw sources after processing.
- MUST NOT create duplicate concepts unnecessarily.
- MUST NOT silently resolve unresolved contradictions.
- MUST NOT convert research into fact without sufficient evidence.
- MUST NOT treat skill instructions as knowledge semantics.
- MUST NOT silently upgrade OKF.

Skills provide capabilities for understanding source formats. They do not define knowledge semantics.

Raw sources are immutable evidence.

Processed receipts represent operational state, not knowledge.

The knowledge base represents consolidated understanding, not copied source material.

Search before create. Read before edit. Validate before receipt.

## Raw Sources And Skills

`raw/` may contain any source format, including PDF, Markdown, TXT, DOCX, XLSX, PPTX, HTML, JSON, YAML, images, source code, exported reports, logs, and arbitrary binary files.

Agents MUST treat every raw file as immutable. They MUST NOT modify, normalize, rewrite, delete automatically, replace the contents of, or move a raw source after processing. A newer source does not justify deleting an older source.

For each source candidate, agents MUST:

1. Determine the file type.
2. Determine whether an installed generic skill is appropriate.
3. Read that skill's `SKILL.md`.
4. Use the skill to inspect and understand the source.
5. Return to this knowledge-maintenance workflow to determine knowledge semantics.

Agents SHOULD use an appropriate installed skill instead of improvising a parser when one exists. Examples include generic `pdf`, `docs`, `spreadsheets`, `slides`, `image`, and `code` capabilities. A skill does not decide what becomes canonical knowledge or how it is represented in OKF.

## Processing Receipts

`processed/` MUST contain only YAML processing receipts. It MUST NOT contain extracted text, converted PDFs, summaries, OCR output, source copies, or canonical knowledge.

Repository convention: store each receipt at `processed/<raw-relative-path>.yaml`, preserving any needed relative subdirectories. For example, `raw/reports/system.pdf` uses `processed/reports/system.pdf.yaml`. This avoids filename-only identity and preserves a stable location; the SHA-256 remains the processing identity.

Each receipt MUST record at least:

- `source`: relative raw source path, for example `../raw/system-architecture.pdf`.
- `sha256`: SHA-256 of the exact source bytes.
- `processed_at`: processing timestamp in UTC.
- `processor.skills`: skills used, when any.
- `result.status`.
- `result.affected`: affected knowledge paths when knowledge changed.

The minimum supported `result.status` values are `knowledge-updated`, `no-knowledge-change`, `failed`, and `unsupported`. Do not add a status without a concrete need. Receipts are repository operational metadata, not OKF documents, unless `okf/SPEC.md` explicitly says otherwise.

Use content hashes to determine source state:

- No receipt: new source.
- Receipt with the same SHA-256: already processed.
- Receipt with a different SHA-256: source changed.

New and changed sources MUST be considered candidates for processing. Unchanged successfully processed sources SHOULD NOT be processed again unless explicitly requested or needed for reconciliation. File names alone MUST NOT be used as processing identity. A `failed` receipt requires retry or review; an `unsupported` receipt requires review when a suitable capability becomes available.

A successful receipt MUST be written or updated only after the entire operation succeeds: source understanding, knowledge inspection, delta classification, required mutations, validation, and required index/log updates. If processing fails earlier, it MUST NOT be marked successful. A failed receipt MAY be written with `result.status: failed`.

## Knowledge-Maintenance Workflow

### 1. Discover

When asked to process sources, scan `raw/` and compare each file's SHA-256 against its receipt. Identify new and changed sources, failed sources needing retry, and unsupported sources. Do not process every source blindly.

### 2. Read Source

For every candidate, identify its format, locate and read an appropriate installed skill, and inspect enough of the source to understand potentially relevant information. Do not summarize solely to create a summary.

### 3. Inspect Current Knowledge

Before creating or changing knowledge, read `knowledge/index.md`; search related concepts; and inspect relevant architecture, components, decisions, requirements, research, provenance, and lifecycle information. Search semantically for synonyms, acronyms, aliases, previous names, and related terminology. Do not infer non-existence from filenames alone.

### 4. Classify Semantic Delta

Classify source information as applicable: already known, new knowledge, supporting evidence, update, correction, contradiction, new decision, superseding decision, hypothesis, inference, research, potentially stale information, or irrelevant to canonical knowledge. Multiple classifications may apply.

### 5. Determine Minimum Mutation

Choose the smallest coherent change set. For each relevant document, decide whether to leave unchanged, create, update, merge, split, supersede, deprecate, mark stale, or remove. Prefer updating an existing document over creating a duplicate. Do not modify unrelated documents.

One document SHOULD primarily answer one meaningful question. Avoid monoliths with unrelated concepts and fragments with no independent value. Split independently evolving concepts; merge documents that represent essentially the same knowledge.

### 6. Apply Changes

All canonical representation MUST follow `okf/SPEC.md`. Preserve valid information, provenance, meaningful history, internal links, relevant metadata, and explicit uncertainty. Distinguish facts from inference and canonical knowledge from research.

Agents MUST NOT fabricate evidence, provenance, verification, verifier identity, source paths, or historical timestamps. Agent synthesis across sources remains agent-derived even when the underlying evidence is strong. Never mark content human-verified without explicit human verification.

### 7. Reconcile Contradictions

For conflicting information, identify both claims and sources; compare source authority and freshness; determine whether contexts differ; inspect relevant decisions; and determine whether either claim supersedes the other. Resolve only with sufficient evidence. Never select a claim solely because it is newer or silently overwrite meaningful history. Preserve unresolved uncertainty explicitly.

### 8. Validate

Before completing ingestion, validate against `okf/SPEC.md`: OKF compliance, accidental duplicate concepts, internal links, raw-source references, meaningful provenance, consistent lifecycle information, stale-content presentation, superseded-decision presentation, index discoverability, and mutation scope.

### 9. Update Index And Log

Update `knowledge/index.md` when the knowledge topology changes materially. It is a semantic entry point for progressive discovery, not a flat file inventory. Update `knowledge/log.md` as required by `okf/SPEC.md` and these repository conventions. Avoid log entries for formatting, whitespace, and trivial typo corrections.

### 10. Write Receipt

Only after validation succeeds, write or update the YAML receipt with the source path, SHA-256, UTC processing time, skills used, result status, and affected knowledge documents where applicable.

## Provenance, Research, And Decisions

Use the mechanisms supported by `okf/SPEC.md` to preserve the distinction among source-derived knowledge, agent-derived knowledge, and human-verified knowledge.

Research is not automatically canonical truth. When research reaches a sufficiently supported conclusion, update the appropriate canonical document, retain useful research, and link it to the resulting concept or decision when useful.

Decision documents SHOULD preserve context, decision, rationale, alternatives, consequences, affected components, evidence, and status when supported by OKF. When a decision changes, represent the relationship as Decision A superseded by Decision B rather than rewriting history as though the newer decision had always applied.

Use relative Markdown links only for meaningful semantic relationships, such as component-to-architecture, decision-to-component, requirement-to-component, research-to-decision, decision-to-superseding-decision, or concept-to-implementation. Do not link merely to increase connectivity.

## Executable Sources And Maintenance

The wiki explains executable artifacts but does not automatically replace source code, schemas, OpenAPI definitions, workflow definitions, DSLs, migrations, manifests, or configuration as runtime sources of truth. When documentation conflicts with implementation, inspect executable artifacts and relevant decisions, determine actual state, update the wiki, and preserve meaningful historical context. Do not blindly assume either side is correct.

During maintenance, agents SHOULD detect orphaned documents, broken links, duplicate concepts, stale knowledge, missing provenance, superseded decisions shown as current, research ready for consolidation, oversized documents, fragmented concepts, raw sources without receipts, changed hashes, and failed receipts.

Assume version control. Knowledge changes SHOULD be small, coherent, reviewable, and reversible. Do not mix semantic changes with broad formatting changes. Do not reformat unrelated files. Git history complements, but does not replace, explicit OKF provenance.
