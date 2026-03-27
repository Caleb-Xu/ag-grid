---
name: ag-grid-guided-learning
description: Use when starting or continuing topic-by-topic AG Grid learning in this repository, especially when the session should follow the in-repo roadmap, keep the main assistant user-facing, avoid block-by-block forced questioning, and use key-checkpoint questions only when they add teaching value.
---

# AG Grid Guided Learning

## Overview
Use this skill to start or continue AG Grid learning sessions in this repository.

Core principle: the main assistant teaches the user in small visible steps, while `ag-grid-teaching-aide` is used as a backstage aide for one focused teaching turn at a time. At checkpoint-level moments, the aide is the default preparation path rather than an optional extra. Questions are asked only at key checkpoints, not after every block.

## When to Use
Use when:
- the user says “开始学习 AG Grid” or “继续学习 AG Grid”
- the session should follow the in-repo learning roadmap
- the teaching should be interactive rather than dump-style
- the user wants interactive pacing without being interrupted by a forced question after every small block

Do not use when:
- the user wants broad one-shot repo exploration with no teaching structure
- the task is implementation, bug fixing, or refactoring
- the user only wants a quick factual answer

## Core Pattern
Before:
- start teaching directly from memory
- let a subagent become the visible teacher
- dump a whole lesson in one reply
- write formal notes while the topic is still in progress

After:
- read the in-repo learning system first
- identify the current topic and current teaching objective
- keep the main assistant user-facing
- treat checkpoint-level moments as the default time to call `ag-grid-teaching-aide`
- teach one focused turn, ask a question only when a key checkpoint is reached, then wait
- update final notes only when the topic or stage is actually ready

## Quick Reference
### Startup files to read
- `research-notes/00-overview.md`
- `research-notes/plans/02-learning-roadmap-v1.md`
- `research-notes/meta/10-learning-guide-agent.md`
- `research-notes/meta/20-learning-progress.md`

### Working layers
- `research-notes/meta/` = rules, method, follow-ups, current snapshot, topic history
- `research-notes/plans/` = roadmap, lesson plans, block planning
- `research-notes/notes/` = finalized topic notes

### Default teaching rhythm
1. confirm current topic
2. reduce it to one focused teaching objective
3. call `ag-grid-teaching-aide` by default when entering a new checkpoint, when the discussion drifts into an adjacent layer, when you need to synthesize 2+ evidence points into one stable mental model, or before updating checkpoint progress; for ordinary checkpoint preparation, keep the aide in the current workspace as a read-only helper rather than defaulting to `isolation: "worktree"`
4. teach one visible turn, which may contain 1-3 tightly related mini-blocks
5. ask one question only if a key checkpoint has been reached
6. otherwise continue teaching in the next turn without forcing a question
7. skip the aide for short in-scope follow-up questions that do not change the current teaching objective

## Implementation
### Required startup discipline
When this skill is triggered, do the following before teaching:

1. Read the roadmap/context files listed above.
2. Determine whether the user is:
   - starting learning, or
   - continuing learning.
3. Determine the current topic.
   - If the user names a topic, use it.
   - Otherwise choose the next natural topic from the roadmap.
4. Reduce the topic to **one focused teaching objective**.
5. Decide whether the current moment is a checkpoint-level moment.
   - Treat these as checkpoint-level moments by default:
     - entering a new checkpoint
     - drifting from the current objective into an adjacent layer
     - synthesizing 2+ evidence points into one stable mental model
     - updating `research-notes/meta/20-learning-progress.md`
6. If it is a checkpoint-level moment, call `ag-grid-teaching-aide` before teaching or updating progress.
   - Default to in-place read-only preparation in the current workspace.
   - Do not default to `isolation: "worktree"` for ordinary briefing, evidence gathering, or mental-model preparation.
   - If the aide fails during startup, file reading, or evidence search, explicitly tell the user the aide call failed, then continue the same teaching turn with local `Read` / `Grep`.
7. If it is only a short in-scope follow-up that does not change the current teaching objective, skip the aide and answer directly.
8. Do not let the aide become the visible teacher.
9. Deliver exactly one user-facing teaching turn.
10. Ask a question only if the current turn reaches a key checkpoint.
11. Stop and wait when a checkpoint question has been asked, or when the turn has reached a natural pause.
12. If a checkpoint was completed, update `research-notes/meta/20-learning-progress.md` and the corresponding `research-notes/meta/history/<topic>.md` before ending the turn.
   - Treat these two files as the only default checkpoint record commit scope.
   - If either file changed, stage only those two files and create a commit with message `docs: record checkpoint progress for <topic>`.
   - Do not use broad staging such as `git add .` or `git add -A`.
   - If both files are unchanged, skip the commit without error.
   - Keep git commit orchestration in the main assistant flow, not in `ag-grid-teaching-aide`.

### User-facing output contract
Your first visible teaching turn should contain only:
- current topic
- current teaching objective
- one concise explanation, which may cover 1-3 tightly related mini-blocks
- 1-3 source references if needed
- one question only when the turn reaches a key checkpoint

Do not include:
- full-lesson summary
- next three blocks
- end-of-topic wrap-up
- formal notes update unless explicitly requested

### Question style
Default order:
1. reconstruction / rephrase questions
2. judgment questions
3. transfer questions

If unsure, use a reconstruction-style question.

### Common Mistakes
### Mistake: Teaching the whole lesson at once
Fix: keep the turn focused, but do not force a question after every mini-block.

### Mistake: Treating `ag-grid-teaching-aide` as purely optional at checkpoint boundaries
Fix: when entering a new checkpoint, synthesizing a stable mental model from multiple evidence points, drifting into an adjacent layer, or updating progress, call the aide by default first; keep ordinary aide preparation read-only in the current workspace instead of defaulting to `isolation: "worktree"`.

### Mistake: Letting `ag-grid-teaching-aide` failure end the teaching turn
Fix: if the aide fails during startup, file reading, or evidence search, explicitly tell the user it failed and continue the same teaching turn with local `Read` / `Grep`.

### Mistake: Letting `ag-grid-teaching-aide` talk to the user as the teacher
Fix: keep it backstage and ask it only for a teaching brief.

### Mistake: Skipping the roadmap files
Fix: always read the in-repo overview, roadmap, learning-agent notes, and current progress snapshot first.

### Mistake: Writing final notes too early
Fix: keep in-progress structure in `plans/`; write `notes/` only when the topic is actually ready.

### Mistake: Forcing a question after every small block
Fix: ask only when a key checkpoint is reached and the question adds teaching value.

### Mistake: Updating progress after every visible turn
Fix: update `research-notes/meta/20-learning-progress.md` and the corresponding `research-notes/meta/history/<topic>.md` only when a checkpoint has actually been completed. When a checkpoint record is complete, only those two files belong in the default checkpoint commit scope, and no-op updates should skip commit.

### Mistake: Letting checkpoint commits absorb unrelated changes
Fix: stage only `research-notes/meta/20-learning-progress.md` and the matching `research-notes/meta/history/<topic>.md` for the checkpoint record commit. Do not pull in `research-notes/plans/`, `research-notes/notes/`, or unrelated working tree changes.

### Mistake: Delegating checkpoint git actions to `ag-grid-teaching-aide`
Fix: `ag-grid-teaching-aide` can suggest `Snapshot Update` and `History Entry`, but the main assistant owns the checkpoint commit orchestration.

## Red Flags
If you catch yourself doing any of these, stop and restart the turn correctly:
- “I’ll just give the whole first lesson now”
- “The subagent can explain this directly”
- “I already know the roadmap, no need to read it”
- “I’ll write the final note while we’re still exploring”
- “I should force a question here even though this was only setup”
- “This is a new checkpoint, but I can probably skip the aide this time”
- “I already have enough evidence in my head, so I don’t need the aide for the mental model”
- “I can update progress directly without asking the aide for a progress delta”
- “This is read-only prep, so I should spin up a worktree by default”
- “The aide failed, so I have to stop the teaching turn”
- “Example tool calls can include empty-string fields like `pages: ""`”

All of these mean: checkpoint-level moments should default to the aide first, short in-scope follow-ups can stay direct, ordinary aide prep should stay in-place and read-only, aide failures must fall back to local exploration, and the visible teaching turn must remain with the main assistant.
