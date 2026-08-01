# Data And Storage Lens

## Lens ID

`data-storage`

## Purpose

Expose persistence, ownership, consistency, migration, and lifecycle choices before a design assumes a database or silently avoids one.

## Applicability Signals

- The feature stores user, project, audit, workflow, cache, telemetry, or configuration data.
- The feature reads from or writes to an existing database, file, queue, blob, event log, cache, search index, or external data provider.
- Multiple components need the same data.
- Data correctness, reporting, privacy, retention, or migration matters.

## Design Decision Points

- Is persistent storage required, or is derived or transient state enough?
- What owns each data type?
- Which storage model fits: file, relational database, document store, key-value store, cache, blob, search index, queue, event stream, or hybrid?
- What consistency model is required: strong, eventual, transactional, or compensating?
- How are schema changes, migrations, backup, restore, and retention handled?
- How does the design avoid one service talking directly to another service's private database?

## Workshop Conduct

- **Shared view:** for a `full` or `medium` pass, expect an ERD, relationship view, ownership map, state model, or data flow unless it would add no clarity for this feature; state the reason if omitted. For a `light` pass, use one only when it makes entities, keys, consistency, or boundaries visible. On a text-only host use console ASCII.
- **Parallel scan mode:** inspect existing data conventions and identify ownership, identity, lifecycle, consistency, migration, and recovery concerns. Return decision forks and questions; do not propose a final schema or storage direction.
- **Workshop mode:** agree those decisions with the human before choosing a storage product.
- Distinguish system of record, projection, cache, event, and disposable working state.
- Re-render material changes before review. In parallel scan mode, return the finding to the coordinator without a separate human response.
- Return the lens result to the main workshop method. Do not load another lens from this reference.

## Question Bank

Use these questions to guide analysis. Do not present them as an interview list; ask only an unresolved question selected by the main workshop method.

- What data must survive process restart, update, rollback, or uninstall?
- Who creates, reads, updates, deletes, exports, and audits the data?
- Is this a system of record, projection, cache, or disposable working state?
- What is the expected data volume and growth pattern?
- What queries, reports, sorting, filtering, or aggregations are needed?
- Are ACID transactions required, or is eventual consistency acceptable?
- What happens when concurrent users update the same entity?
- What retention, deletion, residency, encryption, and PII rules apply?
- What evidence would prove migration and compatibility behavior?

## Trade-off Dimensions

- Persistent versus derived or transient state.
- Relational, document, key-value, file, blob, search, queue, event, or hybrid models.
- Strong transaction boundaries versus eventual consistency and compensation.
- Direct operational queries versus projections, caches, or search indexes.
- Minimal local storage versus explicit ownership, migration, retention, backup, and restore contracts.

## Design Handoff

- State whether storage is required and why.
- Name the selected model and meaningful rejected candidates.
- Record ownership, schema or migration approach, consistency model, and lifecycle.
- Capture producer/consumer compatibility, migration, rollback, and recovery signals when data shape changes.

## Validation Signals

- Validation can exercise real serialization, consistency, or migration paths where practical.
- Review can confirm that the selected store matches query, volume, consistency, and retention needs.
- Cross-service database access is explicitly absent or consciously justified.
