# Turk-OS engineering journal

One entry per work session, newest at the bottom. The journal records design decisions, what was tried, what broke
and why. It is the evidence behind each phase's exit criteria and the starting point of every bug report.

## Conventions

- **Every session:** the date, the phase and task, what was done, the result, and the decision or lesson to keep.
- **Defects open longer than an hour:** write the hypothesis down *before* each attempt and change one thing at a
  time. Record the evidence (serial log lines, `-d int` output, register values), not a recollection of it.
- **Design decisions:** state the decision, the alternatives considered and the reason for the choice.
- **Assembly notes:** when reading compiler output (Phase 0, Task 0.6 and Phase 1), keep the instructive snippets and
  what each instruction does.
- **Phase completion:** when every exit criterion of a phase holds, add a phase-complete entry and update the status
  in the [README roadmap](../README.md#roadmap).
- **Escalation:** after two sessions without progress, follow the debugging protocol in the roadmap and attach this
  file's notes to the question.

## Session template

```markdown
### YYYY-MM-DD · Phase N, Task N.M: short title

**Goal:** what this session was meant to achieve.

**Done:**
- ...

**Result:** what worked, what didn't. Paste the key serial output or error message.

**Defect log** (only when needed):
| # | Hypothesis | Change | Evidence | Verdict |
|---|---|---|---|---|
| 1 | ... | ... | ... | confirmed / rejected |

**Decision / lesson:** the one thing to keep from this session.

**Next:** where work resumes.
```

## Phase-complete template

```markdown
### YYYY-MM-DD · Phase N complete: "exit-criteria title"

- [x] Each exit criterion from the phase document, with how it was verified.
- Main difficulty: ...
- Would change: ...
- Commit: `abc1234`
```

---

## Entries

<!-- New entries go below this line. -->
