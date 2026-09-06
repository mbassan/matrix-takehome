# MassiveUpload — Home Assignment

A system design for uploading and browsing terabyte-scale, diverse file data (docs, images, video, radar, etc.), with user-entered metadata and automatic technical enrichment.

## Repo Overview

Start with [`docs/ASSIGNMENT.md`](docs/ASSIGNMENT.md) for the brief, then read in this order:

1. **[`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)** — the core deliverable. End-to-end flow (upload → handoff → processing pipeline → browse), data models, chosen tools and *why*, upload reliability/resume design, and security/privacy.
2. **[`docs/INFRASTRUCTURE.md`](docs/INFRASTRUCTURE.md)** — what is actually deployed: every component, what it owns, who it talks to, and how the system fails and recovers.
3. **[`docs/API.md`](docs/API.md)** — the Upload API request/response contracts referenced by the architecture doc.
4. **[`diagrams/`](diagrams)** — supporting diagrams (`infra-diagram.png`, `upload-stage.png`, `file-lifecycle.mmd`) referenced from the docs above.

## UI design

Upload + browse page mockups are in Figma (no functional implementation, per the assignment):

**[MassiveUpload Web App — Figma](https://www.figma.com/make/MFAy1KuYLgrPPz006Nip5w/MassiveUpload-Web-App?t=IlGOXMgq9cD5aHC5-20&fullscreen=1)**
