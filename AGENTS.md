# Agent instructions

## Start here

1. For a fresh session, read [the handoff](docs/HANDOFF.md).
2. Use [the design](docs/template-design.md), [work organization](docs/work-organization.md),
   and [glossary](GLOSSARY.md) as the current references. The dated opening draft
   is historical.
3. Continue from the open decision in the handoff. Work down the design tree
   one layer at a time and record decisions after the maintainer agrees.

Use `grill-with-docs` for design sessions. It combines `grilling` and
`domain-modeling`. If those skills are unavailable, read the local evidence,
ask one round of independent design questions with a recommendation for each,
wait for the maintainer's answers, and record agreed terms and decisions as
you go. See the handoff for skills used during implementation and verification.

## Git and verification

- Work autonomously for routine reads, edits, non-destructive Git commands,
  tests, and local builds.
- Use `main` as the trunk. Deliver implementation through short-lived PR
  branches into `main` and squash merge. The initial documentation commit
  establishes the trunk.
- Use the same pinned toolchain and frozen Bun lockfile locally and in CI once
  the starter is scaffolded. Run the relevant checks and review their output
  before claiming completion.

## Safety checkpoints

- Before deleting or overwriting files, removing directories, clearing
  generated or cache content, or using any command that may discard local
  work, ask for explicit approval. Give the exact command, affected paths
  or refs, and consequence. Wait for approval of that specific action.
- Apply the same checkpoint to destructive Git operations such as resets,
  cleans, restores, branch or tag deletion, rebases, amendments, force pushes,
  changes to configured remotes, and other history rewrites.
- Use local or approved non-production targets for routine work. Get explicit
  approval for each action that uses production credentials, contacts a
  production host, or builds, installs, or runs a production build. Apply any
  additional production safeguards in the active workspace instructions.

For this planning checkout, continue design discussion until the maintainer
agrees to scaffold or enable GitHub's template setting.
