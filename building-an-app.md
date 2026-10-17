# Building a reference app

Principles and entities for an app that stores images, labels them with this taxonomy and searches by mechanism. No interface direction is given on purpose.

## Principles

- **Import** from uploads, URLs and exports. Keep the source (URL, author, post text) beside the file. Record dimensions, size, a content hash and a palette. Importing twice must not create copies.
- **Deduplicate in two layers.** Exact copies share a content hash. Near copies (resizes, crops) are found with a perceptual fingerprint: a 64-bit difference hash plus a feature fingerprint from an image model. Flagging a pair when similarity is at least 0.9, or when the hash distance is 4 bits or less and similarity is at least 0.85, is a reasonable start. Tune on your data. Never delete on a match: queue the pair, let a person pick the copy to keep, merge tags, collections, labels and notes into it, move the other to a restorable trash, and remember the answer.
- **Index in passes.** A run is one labeling pass (a person, an AI, or both) tied to a taxonomy version. A check records that a run looked at an image, including when it found nothing, so "labeled none" differs from "never looked". Work in batches: fetch the next N unchecked images, label, write back. Writing enforces the rules in SKILL.md (three labels at most, valid ids, valid look values, confidence in range). Pilot first: run two passes on a sample, compare, fix unclear technique notes, then label everything.
- **Review.** A person can confirm, reject or correct any label. Rejection marks the row rejected and keeps it. Log every change. Show the carrier beside each label.
- **Search** by technique (best examples first), by look field, by palette (store colors in CIELAB so "near this color" tracks what people see), by text (title, notes, description, text in the image, source post), and by visual similarity. Make them combinable.
- **Scoped sets.** Let a brief draw only from a chosen scope: a collection, a board, a project. Make the active scope explicit and apply it to every query. Save each brief's path as a trail so the reasoning can be replayed.
- **Expose it to an AI assistant** through a small tool interface such as MCP: list and set scope; list references as compact text with filters; view a capped selection of images; find similar; read the taxonomy; list references by technique; fetch the next batch and write results; save a brief and a trail. Text first, images on demand, to keep context small.
- **Never lose data silently.** Soft-delete with a trash, take snapshots, store the taxonomy version on every run, and load the taxonomy from `taxonomy.json` as validated data. Make concurrent writers wait for locks rather than fail.

## Entities

- **Reference:** id, library, kind (image or video), file extension, thumbnail flag, title, source URL and source type, notes, width, height, file size, palette, palette in CIELAB, AI description (kept apart from notes), generation prompt if known, OCR text, source-post metadata, content hash, view count, last viewed, hidden-at, deleted-at, created-at.
- **Library:** a top-level body of work (id, name, position). It scopes everything else.
- **Scope:** a saved choice of libraries, collections or boards that a brief may draw from. One is active at a time.
- **Collection:** a nestable folder (name, color, parent, cover, position), linked many-to-many to references.
- **Tag:** free-form label unique within a library, linked many-to-many to references. For subject and mood, not techniques.
- **Board and board item:** a freeform canvas. Items have a type (reference, note, section label), position, size, rotation, stacking order and a data blob.
- **Text index:** full-text search over title, notes, OCR and source text.
- **Fingerprint:** per reference, a difference hash and a feature fingerprint.
- **Duplicate candidate and review:** a pair, its hash distance, similarity and found-at; and the person's answer for that pair.
- **Run:** id, kind (pilot, mapping, manual), label, note, taxonomy version, created-at.
- **Label:** reference, technique, operation, domain (the path actually used), carrier, what changed, confidence, source (assistant or person), run, status (active, pilot, rejected), timestamps.
- **Check:** one row per reference per run (found flag, note, time).
- **Look:** one row per reference: medium, angle, framing, lighting, source, run, updated-at, each from the fixed lists.
- **Trail:** brief, one-sentence reading of what the viewer must do, scope, chosen techniques with reasons and reference numbers, rejected techniques with reasons, outcome.
- **Event:** append-only log of label added, rejected, restored and taxonomy changed, with actor, reference, technique, summary and time.
