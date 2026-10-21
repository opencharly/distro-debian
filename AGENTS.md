# AGENTS.md — distro-debian

The **Debian image family** — charly's `box/debian`. A self-contained
`charly.yml` that pulls main's shared candy layers via `@github` refs — no
namespace import (`import: []`); the distro/builder/init build vocabulary is
embedded in the `charly` binary (`charly/charly.yml`).

Canonical files:

- `charly.yml` — the root manifest: the `discover:` tree and the inline
  `check-debian-debootstrap-vm` bed.
- `candy/debian-skills/charly.yml` — the `debian-skills` bundle candy PLUS the
  five `skill:` routing nodes whose only other owner was an image (`debian`,
  `debian-builder`, `debian-coder`, `debian-debootstrap`,
  `debian-debootstrap-builder`), each carrying
  `name`/`family`/`owner`/`triggers` — with `owner: debian-skills`. A skill's
  `owner:` must be a CANDY: an image is a `candy:` node carrying
  `base:`/`from:`, so it has no source directory, its skill's prose can never be
  read and the node never reaches the `ai.opencharly.skill` label
  (opencharly/sdk#434). The candy is composed into every image that carries one
  of those skills.
- `.agents/skills/<name>/SKILL.md` — the five skill BODIES, one directory per
  node's `name:`, at THIS REPO'S ROOT — the one base both readers resolve after
  opencharly/sdk#445 (`deploykit.candyRepoRoot`). The files are the source of
  record for the prose.
- `box/<name>/charly.yml` — one manifest per image / builder / VM box.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:debian` — the Debian 13 base image (uid-1000 create mode).
- `/charly-distros:debian-builder` — the multi-stage builder image.
- `/charly-distros:debian-debootstrap`,
  `/charly-distros:debian-debootstrap-builder` — the bootstrap path.
- `/charly-coder:debian-coder` — the kitchen-sink dev image.
- `/charly-vm:debian-debootstrap-vm` — the bootstrap VM + its check bed.
- `/charly-image:image` + `/charly-image:layer` — composition and candy
  authoring (`charly.yml` schema, `plan:` step verbs, service declarations).
- `/charly-check:check` — the disposable check beds and `plan:` authoring.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The functional evidence is the disposable `check-debian-debootstrap-vm` bed
  and the `plan:` `check:` steps on every box. A docs-only change runs no runtime
  bed — the documentation-only change class runs the non-runtime standards only.

## Modify this repo

- Edit a skill's body at the repo root, `.agents/skills/<name>/`, and its routing
  node in `candy/debian-skills/charly.yml` together — never the generated
  `SKILL.md` in the marketplace corpus; a corpus regeneration
  (`charly marketplace generate`) projects both.
- This repo carries exactly ONE candy of its own — `debian-skills`, the
  skill-prose bundle that exists because a `skill:` owner must be a candy
  (opencharly/sdk#434). Any other new layer belongs in its own `layer-*` repo and
  is referenced here by a pinned `@github` ref.
- New behaviour claims belong in a `plan:` as an observable `check:` step, and
  in the owning skill.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
