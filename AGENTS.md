# Agent instructions

## Persistent scientific research loop

Owner instruction, 2026-11-08. Pursue the user's research objective through
successive evidence-driven attempts. A failed command, rejected candidate,
negative result, completed subtask, or written plan is not by itself a reason
to end the effort.

### Hypotheses and experiments

- State the question and retain its acceptance criteria. Separate observations,
  assumptions, hypotheses, theories and conclusions. For explanatory work,
  propose competing falsifiable hypotheses and say what would change your mind.
- Choose the smallest informative experiment or proof obligation. Before running,
  state predictions, controls or baseline, observables, success/falsification
  criteria, resource limits and artifact locations. Descriptive studies use a
  sampling and observation plan without inventing a predicted outcome.
- Inspect actual outputs and check correctness before interpreting them. Keep
  methods, inputs, versions, seeds, raw evidence, uncertainty and failed attempts
  reproducible under the repository's existing storage and privacy rules.
- Update or reject hypotheses when evidence disagrees. Broader theories must
  explain existing observations and yield further testable predictions. Preserve
  null and negative results; never invent evidence, cherry-pick outcomes, relax
  frozen criteria, or turn a timeout into a proof of impossibility.

### Recover, adapt and continue

1. After each attempt, record what happened, what was learned, the remaining gap,
   and the next discriminating action. Execute that action when it is within the
   authorized scope and available resources; do not stop at proposing it.
2. When blocked, diagnose the concrete cause. Distinguish infrastructure, access,
   implementation, measurement and mathematical issues. Test a focused remedy
   and then a materially different approach when the evidence warrants it.
   Consider smaller reproductions, alternative tools or methods, independent
   derivations and other tractable parts of the same objective.
3. Before declaring the objective blocked, investigate the reasonable available
   alternatives and record why each cannot currently proceed. One failed path
   blocks that path, not the entire goal. Recording a problem, backlog card or
   handoff does not itself satisfy the original task.
4. Continue across intermediate stage boundaries for a full-cycle assignment.
   A worker finishing, a review rejecting a candidate, or a component passing
   is input to the next step; it is not automatic goal completion. Honor an
   explicitly stage-limited task's stopping point.
5. Use evidence to choose retries. Do not repeat an unchanged failed command
   without a reason to expect a different outcome, rerun checks merely to get
   green, or consume resources indefinitely. Respect existing admission,
   ownership, role, privacy, review and budget limits. Persistence grants no
   additional execution or spending authority.
6. Before interruption or an actual resource/access limit, checkpoint through
   the repository's established mechanism: objective, current hypothesis,
   commands/configuration, evidence pointers, failed approaches, remaining
   budget and exact next action. Resume from that state and check whether a
   recorded blocker has changed instead of restarting the investigation.

### Evidence required to stop

Stop only when the requested acceptance criteria are met and checked, the user
redirects or stops the work, an explicit task boundary is reached, or a concrete
authorization/resource/dependency blocker prevents further useful authorized
progress after reasonable alternatives have been investigated. A disproved
candidate may close that candidate; pursue the remaining objective through an
informed successor when one is available.

If work must stop incomplete, report the observed blocker, alternatives actually
tried, evidence and unresolved uncertainty, saved checkpoint, exact next
experiment or action, and the specific input or condition needed to resume.
Never claim success from effort alone or claim that every avenue is exhausted
without evidence. Apply this loop proportionately: routine edits need direct
checks; substantive research needs an explicit experimental record.

## Goal: what to work on next

Sessions should never need a prompt to find their work. `docs/ACTIVE.md`
holds the owner's pinned goal and ordered active items; the newest session
logs in `docs/session-logs/` (where they exist) hold the open threads.
`python3 tools/goal_brief.py` prints both, pinned items first, capped at 120
lines.

- **Claude** gets the brief from the SessionStart hook in
  `.claude/settings.json`. `/goal` (`.claude/commands/goal.md`) re-reads it
  and starts the next item; `/goal <text>` pins a new goal.
- **Codex and other runtimes** run `python3 tools/goal_brief.py` at session
  start (Codex gets it from `.codex/hooks.json` where that file exists) and
  treat a bare `goal` or `/goal` message like Claude's `/goal`.
- With no other instruction, take the first unchecked item in
  `docs/ACTIVE.md`; if there is none, the most recent open thread that no
  open draft PR already covers. State the choice in one line and start; ask
  only for decisions the brief marks as the owner's.

### First step of every task: record it in `docs/ACTIVE.md` and open a draft PR

Owner instruction, 2026-11-16. Before any other work on a task, every
session, skill and agent, in every runtime, does these steps first:

1. Fetch the default branch and create the task branch.
2. Add one line for the task under "Active items" in `docs/ACTIVE.md`:
   `- [ ] <task> (<runtime>, started <YYYY-MM-DD>, branch <branch>)`. If the
   task is already listed, mark that line in progress instead.
3. Commit that change alone, push, and open a **draft** PR named after the
   task. Its body says what will be done and how it will be checked. Add
   `PR #<n>` to the line in the next commit.

The draft PR is the tracking record: push to it as the work goes and keep
the line current. When the work is finished and checked, tick the line
(`- [x]`) or remove it, then **mark the PR ready for review** and enable
auto-merge (`gh pr merge <n> --auto --merge`) where the repository allows it.
A task that stops unfinished keeps its draft and its unticked line, with the
blocker under "Blocked / questions". When `docs/ACTIVE.md` conflicts, merge
the default branch and keep every other session's line. A subagent working
inside the parent's task uses the parent's line and PR.
