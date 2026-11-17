# CLAUDE.md

**Goal and first step** (AGENTS.md, "Goal: what to work on next" and "First
step of every task"): the SessionStart hook prints `tools/goal_brief.py` (the
pinned goal and items in `docs/ACTIVE.md`, then the newest session-log open
threads); with no other instruction, take the first unchecked item and start.
`/goal` re-reads the brief; `/goal <text>` pins a new goal. Before any other
work on a task, add its line to `docs/ACTIVE.md`, commit that alone, push and
open a **draft** PR; when the work is finished and checked, tick the line and
mark the PR ready for review.


Read [AGENTS.md](AGENTS.md) for this repository's agent instructions.

## Persistent scientific research loop

Read and follow [AGENTS.md](AGENTS.md#persistent-scientific-research-loop).
That section governs hypothesis testing, recovery from obstacles, continued
iteration, checkpoints and evidence required before stopping in this runtime.
