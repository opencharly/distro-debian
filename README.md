# distro-debian

The **Debian image family** for [OpenCharly](https://github.com/opencharly/charly) —
the root of the deb-based hierarchy.

This repo is mounted as a git submodule at `box/debian` of the main repo. It
contains **no candies of its own** and carries no build-config file: every candy
is an `@github.com/opencharly/<layer-*|pod-*|plugin-*>[:subdir]:<tag>` ref into
its standalone candy repo, and the distro/builder/init build vocabulary is
embedded in the `charly` binary — `import:` is empty (`import: []`). The Debian
bases root at the upstream `docker.io/debian:13` image directly.

## What's here

| Kind | Entries |
|---|---|
| Base / builder | `debian` (base), `debian-builder` (pixi/npm/cargo multi-stage builder) |
| Images | `debian-coder` (kitchen-sink dev box), `debian-debootstrap-builder` (privileged), `debian-debootstrap` (`from: builder:debootstrap`) |
| VM | `debian-debootstrap` (bootstrap-from-scratch via `debootstrap`) |
| Check bed | `check-debian-debootstrap-vm` (disposable bootstrap-VM bed) |

The `debian` base runs as uid-1000 `user` in **create mode** — Debian 13 ships
no pre-existing uid-1000 account.

## No coupling with main

Nothing in the main `opencharly` repo consumes any Debian image (no
`base: debian` image stays in main), so there is no main ↔ debian coupling: the
only edge is `debian → main` (this repo pulls candies via `@github` refs). The
image DAG is acyclic (`debian-coder → debian → docker.io/debian:13`;
`debian-debootstrap → debian-debootstrap-builder → docker.io/debian:13`).

## Build

```bash
# Inside the submodule (the build verb defaults to charly.yml):
charly box build debian

# From the parent opencharly repo:
charly -C box/debian box build debian

# Standalone, against the published repo:
charly --repo opencharly/distro-debian box build debian
```

The first build resolves the upstream github references into
`~/.cache/charly/repos/` and materializes the referenced layers under
`.build/_layers/`.

## debootstrap-from-scratch (`debian-debootstrap` / `check-debian-debootstrap-vm`)

`debian-debootstrap` builds a Debian rootfs from scratch via `debootstrap` inside
the privileged `debian-debootstrap-builder` container (`from:
builder:debootstrap`). `check-debian-debootstrap-vm` boots that rootfs under
libvirt/QEMU and carries `disposable: true`, so it rebuilds unattended. From
this repo's root:

```bash
charly check run check-debian-debootstrap-vm
```

## Requirements

A build of any image here fetches from the upstream repo, so it needs network
access and a `charly` recent enough to understand the config's schema version
(`charly` hard-fails with a "newer than this charly supports" message if the
config schema is newer than the binary supports).

## Layout

The canonical-file inventory (the root `charly.yml`, the per-box
`box/<name>/charly.yml` manifests, and the workflow) lives in
[`AGENTS.md`](AGENTS.md).

## Related

- Owning skills: `/charly-distros:debian`, `/charly-distros:debian-builder`,
  `/charly-distros:debian-debootstrap`,
  `/charly-distros:debian-debootstrap-builder`, `/charly-coder:debian-coder`
- Bootstrap VM: `/charly-vm:debian-debootstrap-vm`
- Sibling: `/charly-distros:ubuntu` (deb-family, adopt mode)
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
