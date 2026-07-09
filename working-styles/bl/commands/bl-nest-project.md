Move a local project folder into the `ooc-team` repo so the team can use it.

The `ooc-team` repo at `~/Projects/ooc-team/` is the team's shared working space. This skill takes a local project folder (typically under `~/Projects/`) and nests it into the right place in `ooc-team/`, generates a README that fits the repo's conventions, commits direct to `main` (per repo convention), and optionally tears down the original.

**Do not run this proactively.** Only when Brian explicitly asks to nest a project, share something with the team, or move work into `ooc-team`.

## Guardrails

- **The repo's `CLAUDE.md` is law.** Read `~/Projects/ooc-team/CLAUDE.md` once at the start of every run. Its rules (especially git safety) override anything in this skill that conflicts.
- **Naming is snake_case** for any new directory or file you create inside `ooc-team/`.
- **Commits go direct to `main`** — that's the established repo convention. No PR, no feature branch, unless Brian asks.
- **`git add` specific files by name.** Never `git add -A` or `git add .`.
- **Push only after Brian confirms.** Default workflow: commit, summarize, ask before pushing.
- **Never delete the source folder until Brian explicitly approves teardown** at the end.

## Workflow

Follow these phases in order. Confirm with Brian at each `→ ASK` step before continuing.

### Phase 1 — Discover

1. Read `~/Projects/ooc-team/CLAUDE.md` and `~/Projects/ooc-team/README.md`. Note the index markers (`<!-- index:start -->` / `<!-- index:end -->`).
2. Run `git fetch && git status` inside `~/Projects/ooc-team/`. If `main` is behind `origin/main`, pull (fast-forward) before doing anything else. If it has diverged, stop and surface the situation to Brian.
3. List top-level subdirs of `~/Projects/ooc-team/`. For each topical parent (e.g. `launch/`), peek at its `CLAUDE.md` if one exists — its naming conventions take precedence over this skill's defaults for anything nested under it.

→ ASK Brian for the **source folder path** if not already given. Confirm with `ls -la` that it exists and inspect its top-level contents.

### Phase 2 — Decide placement

Look at the source folder and the target repo's structure together. Decide whether the new project belongs:
- **Nested under an existing topical parent** (e.g. a launch-related project belongs under `launch/`), or
- **At the top level** of `ooc-team/` (new topical area, new README index entry)

Bias toward nesting when there's a clear topical parent — that's how `intake_tracking_process` ended up under `launch/`.

→ ASK Brian:
- Confirm the placement (parent dir or top-level).
- Confirm the **snake_case directory name** for the project in its new home.

### Phase 3 — Plan the layout

Inspect the source folder. Classify its contents:
- **Markdown / text docs** → will go under `docs/` in the new location
- **Source binaries** (PDFs, DOCX, PPTX, XLSX, images) → will go under `sources/`
- **Scripts / code** → keep their existing structure unless Brian says otherwise
- **Anything that's already obsolete or duplicates parent material** → flag and propose to leave behind

→ ASK Brian:
- **Canonical source of truth**: is there a live system (Notion page, Google Sheet, Jira project) where the project's state actually lives? The repo is for offline reference and source binaries. Get the URL.
- **Sibling material to NOT duplicate**: anything in the parent topical dir (or a sibling project) that already covers part of this material? If yes, the new README will link to it instead of copying it.
- **Governance snapshot**: who owns this? Owner, sponsor, key roles. (Optional — skip if not applicable.)
- **Teardown intent**: does Brian want the original source folder deleted at the end, or kept in place?

### Phase 4 — Propose and confirm

Print a tight plan covering:
- **Source** → **Destination** path
- **Directory tree** that will be created (`docs/`, `sources/`, etc.)
- **Files** to copy and where each one lands
- **README.md** outline (sections and key links — not the full text yet)
- **Commit message** (one-line subject; body explains why)
- **Push or hold** (default: hold and ask after commit)
- **Teardown** of the original (yes/no, deferred to end)

→ ASK Brian to approve the plan before executing anything.

### Phase 5 — Execute

In this order:
1. `mkdir -p` the destination structure.
2. Copy (don't move yet — preserve the source until teardown) markdown into `docs/`, binaries into `sources/`. Use `cp` for files, `cp -R` for nested dirs.
3. Generate the `README.md` at the new project root using the template below.
4. Run `git status` to see what's new. Sanity-check nothing unexpected got pulled in.
5. `git add` each new path by name (no `-A`, no `.`).
6. `git commit` with the agreed message. Use the SSH-agent inline workaround if the commit fails with a signing error (see [`memory: Commit signing + Claude Code shell snapshot trap`] in Brian's notes):

   ```bash
   SSH_AUTH_SOCK="$HOME/Library/Group Containers/2BUA8C4S2C.com.1password/t/agent.sock" git commit -m "..."
   ```

7. If the new project lands at top level (not nested), update `~/Projects/ooc-team/README.md`: insert a one-line entry into the `<!-- index:start -->` / `<!-- index:end -->` block, pointing to the new dir.

→ ASK Brian whether to push to `origin/main`. Don't push without an explicit yes.

### Phase 6 — Teardown

Only if Brian approved teardown in Phase 3:
- Confirm the destination has the files (a quick `ls -R` of both source and destination, side-by-side).
- → ASK one more time: "Confirm delete `<source path>`?" Get an explicit yes.
- `rm -rf` the source. Never use destructive deletes without that second confirmation.

If Brian declined teardown, just leave the source alone and note where it is.

### Phase 7 — Summarize

End with a tight recap:
- New location
- Commit SHA + message
- Push status (pushed / held)
- Teardown status (deleted / kept)
- Next steps if any (e.g. "update sibling CLAUDE.md to mention the new project")

Also update Brian's memory (`~/.donna/memory.md`) with a one-line pointer to the new project under the **Projects** section — but only for substantive projects, not trivial moves.

## README template

Generate the new project's `README.md` from this skeleton. Fill in the bracketed fields from what Brian provided in Phase 3. Sections marked OPTIONAL get dropped if not applicable.

```markdown
# [Project Title]

> [One-sentence description of what this folder contains — pulled from Brian's intent.]

**Canonical [source of truth] ([system name]):** [URL]   <!-- OPTIONAL if no live system -->

Snapshot date: **YYYY-MM-DD**

## Scope

This folder holds:
- [Bullet list of what's in scope]

[For framework / shared context, see ...]   <!-- OPTIONAL: link siblings to avoid duplication -->
- [`../<sibling>.md`](../<sibling>.md) — [what's in it]

## Contents

- [docs/<file>.md](docs/<file>.md) — [one-line description]
- [sources/](sources/) — [list of source binaries, one line]

## Governance snapshot   <!-- OPTIONAL -->

| Role | Owner |
|---|---|
| [Role name] | [Owner name] |
```

The template is a starting point. Adapt it to fit what Brian actually said — don't force unused sections.

## What "good" looks like

A successful run leaves:
- A new directory in `ooc-team/` with a clean `docs/` + `sources/` split and a README that points to the canonical live system (so future readers don't mistake the repo snapshot for the source of truth).
- A single direct-to-`main` commit with a clear subject line.
- No duplicated framework material — if a sibling already covers it, the README links to the sibling.
- Either a torn-down original (when Brian approved) or an untouched original (when he didn't).
- A pushed `main` (when Brian approved push) or a committed-but-held `main` (when he didn't).
