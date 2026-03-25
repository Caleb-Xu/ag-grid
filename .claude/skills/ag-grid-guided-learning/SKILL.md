---
name: ag-grid-guided-learning
description: Use when starting or continuing topic-by-topic AG Grid learning in this repository, especially when the session should follow the in-repo roadmap, keep the main assistant user-facing, use one-teaching-block-at-a-time pacing, and optionally use ag-grid-teaching-aide only as a backstage teaching aide.
---

# AG Grid Guided Learning

## Overview
Use this skill to start or continue AG Grid learning sessions in this repository.

Core principle: the main assistant teaches the user in small visible steps, while `ag-grid-teaching-aide` is used only as a backstage aide for one teaching block at a time.

## When to Use
Use when:
- the user says “开始学习 AG Grid” or “继续学习 AG Grid”
- the session should follow the in-repo learning roadmap
- the teaching should be interactive rather than dump-style
- the user wants one-teaching-block-at-a-time pacing

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
- identify the current topic and current teaching block
- keep the main assistant user-facing
- teach one block, ask one question, then wait
- use `ag-grid-teaching-aide` only to prepare a backstage teaching brief if needed
- update final notes only when the topic or stage is actually ready

## Quick Reference
### Startup files to read
- `research-notes/00-overview.md`
- `research-notes/plans/02-learning-roadmap-v1.md`
- `research-notes/meta/10-learning-guide-agent.md`

### Working layers
- `research-notes/meta/` = rules, method, follow-ups
- `research-notes/plans/` = roadmap, lesson plans, block planning
- `research-notes/notes/` = finalized topic notes

### Default teaching rhythm
1. confirm current topic
2. reduce to one current teaching block
3. teach one chunk
4. ask one question
5. wait for the user

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
4. Reduce the topic to **one current teaching block**.
5. Decide whether you need backstage help from `ag-grid-teaching-aide`.
   - Use it only to prepare a brief for the current block.
   - Do not let it become the visible teacher.
6. Deliver exactly one user-facing teaching block.
7. Ask exactly one question.
8. Stop and wait.

### User-facing output contract
Your first visible teaching turn should contain only:
- current topic
- current block
- one concise explanation
- 1-3 source references if needed
- one question

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

## Common Mistakes
### Mistake: Teaching the whole lesson at once
Fix: shrink to one block and stop after one question.

### Mistake: Letting `ag-grid-teaching-aide` talk to the user as the teacher
Fix: keep it backstage and ask it only for a teaching brief.

### Mistake: Skipping the roadmap files
Fix: always read the in-repo overview, roadmap, and learning-agent notes first.

### Mistake: Writing final notes too early
Fix: keep in-progress structure in `plans/`; write `notes/` only when the topic is actually ready.

## Red Flags
If you catch yourself doing any of these, stop and restart the turn correctly:
- “I’ll just give the whole first lesson now”
- “The subagent can explain this directly”
- “I already know the roadmap, no need to read it”
- “I’ll write the final note while we’re still exploring”
- “I’ll ask several questions so the user can choose one”

All of these mean: reduce to one block, one question, one visible turn.
