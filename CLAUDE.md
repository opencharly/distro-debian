# box/debian — signpost (not the rule-set)

This submodule is the **Debian** base image family: a self-contained `charly.yml`
that pulls main's shared candy layers via `@github` refs — no namespace import
(`import: []`); the distro/builder/init build vocabulary is embedded in the `charly`
binary (`charly/charly.yml`).

**Load these skills FIRST (R0):**

- `/charly-distros:debian` — the Debian 13 base image (uid-1000 create mode).
- `/charly-distros:debian-builder` — the builder image.
- `/charly-distros:debian-debootstrap`, `/charly-distros:debian-debootstrap-builder` —
  the bootstrap path.
- `/charly-coder:debian-coder` — the dev image; `/charly-vm:debian-debootstrap-vm` — the bootstrap VM.

**Authoritative rules live in the `opencharly` superproject's root `CLAUDE.md`**
(R0–R10, hard-cutover, AI attribution, git-workflow). This file only signposts
and restates no rule. The multi-agent workflow is in `/charly-internals:agents`.
History lives in this repo's `CHANGELOG/` (one file per month).
