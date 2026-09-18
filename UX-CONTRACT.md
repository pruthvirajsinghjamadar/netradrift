# Canonical interface contracts

Locale en-IN, INR, ISO dates stored and UTC date-only display. Dataset as-of date is explicit and never silently advanced.

| Capability | Owner | Variants | Verification |
|---|---|---|---|
| Navigation | App header links | overview, works, reviews, data | build |
| Select/Listbox | components/ui/select | authored | typecheck |
| Table | components/ui/table | read-only paginated | unit data checks |
| Form | shared review form | save in place, inline errors | API tests |
| Scrollbar | app/globals.css | global | static audit |
| Toast | shared live status | info, error, success | static audit |
| CRUD | authenticated review API | append history, no delete | integration |

Search/filter/page state is in URL. Selection is ephemeral; changing screens closes detail only after notes have saved or been explicitly discarded. Detail is a persistent nonmodal section, not a modal. No full-screen keyboard trap. Reviews are private per authenticated user and dataset. Import creates a new dataset; original is retained. No destructive actions. Uploads accept exactly the five source schemas, max 12MB combined, max 15000 rows. Missing values are unknown, not zero. Successful payments remain separate from pending payments.

## Public demonstration extension
The public `/` route renders the complete synthetic dataset immediately, independent of private research API availability. `/research` preserves authenticated CSV analysis. Demo sessions use an opaque HttpOnly Secure SameSite cookie; server records are isolated by a hashed session identity. Switching demo personas does not change permissions. No real authority receives notifications.

Navigation view/work and public filters use URL state. Work selection carries through map, peer group, investigation, timeline, duplicate pair and capture. Shared Choice remains the canonical authored select, shadcn Table the canonical table, Textarea the canonical note input. New shared scene, project card, buttons and status styles live under `components/demo` and `app/demo.css`. Custom side navigation remains nonmodal document navigation. Tables show ten rows with previous/next; peer list is bounded by the 36-work demo.

Evidence and action creation: user trigger → inline pending → server save → status plus persistent investigation history. IDs are idempotent per demo owner. Errors retain the draft and permit retry. Photos are converted locally to JPEG, capped at 480 KB server-side, and never used to automatically certify completion. Optional coordinates and exact image reuse checks are disclosed. Explicit offline queue uses browser-session storage; keep the browser session until sync. Demo saves have a 100-record workspace cap and sessions expire after 30 days. No destructive operations or external messages.
