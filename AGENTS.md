# AGENTS.md — layer-omarchy-network

Standalone candy repo for the Omarchy network/bluetooth/print/firewall layer —
the daemons that own a machine's network state, composed by the machine images
only. The repo is multi-candy-shaped: the root `charly.yml` carries only the repo
shape (`discover:`), and the member candy lives in
`candy/omarchy-network/charly.yml`.

This repo has **no `skill:` entity** in its candy manifest, so there is no
dedicated owning skill projected into the marketplace corpus. The gap is recorded
against `opencharly/opencharly#291` (the batch that authors missing `skill:`
entities).

Canonical files:

- `charly.yml` — the repo shape (`repo:` + `discover:`).
- `candy/omarchy-network/charly.yml` — the candy entity: the `require:` on the
  foundation layer, the `distro:` package arm, and the `plan:` `check:`
  assertions.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:omarchy-base` — the family foundation skill: the package
  sources, the pinned mirror snapshot, and the runtime this layer builds on.
  Load before editing or troubleshooting.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, and service declarations). Load before editing any entity field or
  plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence — they assert
  the packages are present. No unit is enabled here.

## Modify this repo

- There is no `skill:` entity to keep in sync; if one is added (per #291), it
  must be edited together with the candy entity in the same change.
- Keep this candy **machine-only**. NetworkManager, bluez, cups and avahi claim
  host devices or the system D-Bus, and `ufw` needs `NET_ADMIN`; none belong in a
  pod. Enabling the units is the machine image's job, not this candy's.
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo — read it
  before landing.
- Release history lives in `CHANGELOG/`.
