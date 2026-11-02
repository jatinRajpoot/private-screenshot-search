# Private Screenshot Search - Project Scope

Build an Android-first application that finds screenshots by their visible text and helps users review clutter while keeping images and indexes on device. Portable backup is part of the product so a phone change does not strand the searchable library.

## Users and problem

Students, shoppers, travelers, and knowledge workers accumulate visually similar screenshots that are difficult to retrieve. Initial validation targets people with at least 1,000 screenshots and repeated searches for receipts, recipes, products, travel details, or code.

## MVP workflow

1. Explain access and let the user select screenshots or grant supported media access.
2. Index allowed screenshots using on-device optical character recognition (OCR).
3. Search text and filter by capture date, collection, or available source metadata.
4. Review duplicate groups and old temporary screenshots; explicitly approve cleanup.
5. Export an encrypted portable library and restore it on another installation.

## Included in the MVP

- Screenshot discovery through supported Android media APIs plus manual import for ambiguous files.
- Offline OCR, incremental indexing, retryable failures, and visible progress with pause/resume.
- Text search, date filters, user tags, and manual collections.
- Exact duplicate detection plus a review-only near-duplicate prototype if accuracy is sufficient.
- Age-based cleanup suggestions and user-marked temporary items; expiry inference remains advisory.
- Image preview with matched text, basic image metadata, and source-app labels only when reliable metadata exists.
- Encrypted backup/restore containing selected images, OCR text, tags, collections, and stable identifiers.
- Explicit deletion review through supported system flows; rescans reconcile removed or changed images.

## Deferred features

iOS, desktop companions, cloud OCR, automatic semantic classification, background monitoring beyond supported access, cross-device live sync, and automatic deletion are outside the MVP. Do not promise source-app identification when the OS or image metadata cannot supply it.

## Proposed architecture and data

Use Kotlin, Jetpack Compose, Room/SQLite with full-text search, WorkManager for resumable indexing, and a bundled on-device OCR engine. Choose ML Kit or Tesseract after a small offline accuracy and packaging spike. Keep media discovery, OCR, indexing, duplicate analysis, cleanup planning, and backup as separate components.

Persist Screenshot, OcrResult, Collection, Tag, IndexJob, and BackupManifest entities. Track media references, content hashes, dimensions, timestamps, OCR version/language, and index status. External media references must be remapped on import so restored entries do not depend on the old device's paths.

## Privacy, permissions, and cost

Explain scoped-media access before requesting it, support partial grants, and avoid uploading images or OCR text. Bundle the OCR resources needed for supported languages so first-use indexing can operate offline. Protect the local index with platform-supported storage controls. Use authenticated encryption for backups, keep keys separate, and explain password-loss recovery limits.

No account, hosted database, paid inference, or cloud image pipeline is required for core functionality. Store distribution and optional payment costs are separate from runtime infrastructure.

## Acceptance criteria

- After installation, supported-language OCR, indexing, and search work in airplane mode.
- A representative 1,000-image library indexes incrementally and resumes after termination without duplicate entries.
- Media access revocation or missing images produces recoverable states instead of crashes.
- A labeled query set measures retrieval accuracy and elapsed time against the user's normal gallery.
- Duplicate suggestions never delete automatically; file deletion requires explicit review and system approval where applicable.
- An encrypted export restores selected images and their searchable metadata into a clean installation.
- Wrong-password or corrupted archives fail before modifying the existing library.
- Logs and background jobs do not expose screenshot text or image contents to external services.

## Delivery and validation

| Phase | Deliverable | Exit condition |
|---|---|---|
| 1 | Media access and offline OCR spike | Representative screenshots produce useful searchable text. |
| 2 | Search, collections, and cleanup review | Users find old captures faster and safely review clutter. |
| 3 | Portable backup and pilot | A second installation restores a usable library. |

Budget 5-8 solo-developer weeks as a planning estimate. Test with 15-20 people holding 1,000+ screenshots; compare task completion time with their normal gallery and record failed queries. Native gallery competition and OCR accuracy are the largest risks. A later one-time Pro unlock could cover advanced similarity review, bulk workflows, or a desktop companion; willingness to pay remains untested.

## Scope basis

This scope is derived from the supplied report, "Recent Verified User Pain Points Suitable for Zero-Backend SaaS, Mobile Apps, and Browser Extensions" (research window ending November 3, 2026). Market demand, estimates, and monetization are hypotheses to validate; the report's internal citation markers are not independently verified references. This repository starts with this scope document only; implementation has not started.
