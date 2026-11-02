# How-to

The specific things the [architecture](architecture.md) names. Read a
section when you are doing that thing. The role and run-folder patterns
can be used directly, with any tools. The connector contract and the
context generation describe our internal implementation; its executable
parts are not in this repository yet.

## Write a role file

A role tells whoever hands out work (a person, a project manager agent or
the orchestrator) who should take a piece of work and what that worker
should load. The tool defines no roles. Each organization writes its own,
if it wants them, from skills it already has: one Markdown file per role,
kept wherever the organization keeps its guidance. Every field is
optional.

```markdown
# Backend

Owns: server APIs, stored data and background jobs.
Repositories: api, workers.
Skills: backend-dev, verify-flows.
Resources: the API docs, the production runbook, the error dashboard,
  the wiki pages on data and background jobs.
Model: the default; a stronger one for reviews.
Hands off to: Web, for screens that use a new API.
```

- Skills are names of skills the organization already has; the skill
  index lists them.
- Resources are where the role looks first: the docs, wiki pages,
  runbooks and dashboards for its area. A worker in the role reads them
  before it investigates, the way a person in that job would.
- A role can be the lead for its area: work for that area goes to it, and
  it hands out the pieces within its scope.
- A brief names the role and its skills, so the worker loads them. The
  same worker, started with a different brief, is a different role;
  nothing is installed per role.
- A role grants nothing: what an agent may do still comes from the
  workspace's rules.
- Roles are files, not configuration, because a role is guidance, and
  guidance changes as a team learns. A file next to the organization's
  other guidance can be read by a person, searched by an agent and
  reviewed like any other change.

## The connector contract

A connector carries a message to a person over one channel: a chat
channel, email, a phone call, a push notification, anything that can be
reached. The organization wires each one; the hook hands it the message
as data, never as code to run.

- The message carries: what happened and how urgent it is; the text, with
  links to the evidence; a key for the problem, so repeats are
  recognized; whether an answer is wanted, and where to give it.
- Each connector runs with its own time limit and its own recorded
  result. A hanging or failing connector does not block the others, and a
  connector that succeeded is not sent the same message again because
  another failed.
- Success means the connector accepted the message, not that a person
  read it. A lost acknowledgement is recorded as unknown.
- The same problem within its quiet period (an hour to start) is sent
  once and counted, unless its urgency rises. The record is kept on disk,
  so quieting survives restarts and an unsent message stays visible.
- A known failure may be retried (up to five attempts to start) without
  resending what succeeded. A delivery with no acknowledgement is kept
  visible rather than resent, because resending could duplicate it.
- With no connector wired, the message is kept where the owner can read
  it, shown in status, and the result says nobody was reached. A
  workspace can start with nothing wired, see what it would have been
  told, then add channels one at a time.

## How context is generated (our implementation)

1. `docs/context.json` names the source pages for its scope, grouped
   into sections, plus optional checks to run before anything is
   generated. Source paths can point into sibling scopes, so the
   organization's shared rules are named, not copied.
2. `context generate` writes `AGENTS.md` from those pages and `CLAUDE.md`
   as a link to it. With `--all` it also builds the skill indexes and
   each folder that has its own context manifest.
3. A local overlay file for this machine and the skill indexes are added
   automatically and never committed.
4. The generated files are ignored by git on purpose. Edit the sources,
   then regenerate.
5. A check verifies each scope: its sources exist and pass their declared
   checks, the generated output is current, the generated and local files
   are ignored, and the scope is registered. It fails loudly when any is
   off.
6. Workspace sync regenerates enabled scopes when it succeeds and reports
   skips and failures. Automatic discovery today covers the root and its
   immediate child scopes; deeper scopes need explicit handling. The
   generator can report each section's size and warn when one page
   dominates.

Skills: a skill is a folder with a `SKILL.md`, a short description of when
to use it and what it does, plus references. A skill marked as not
loadable at startup stays discoverable but is not put in front of every
agent. An index that selects nothing is left out, so no scope carries an
empty heading.

## Keep a run folder

```text
runs/<task>/
  brief.md       what to do and why, written before it started
  status.md      where it stands, kept current by the worker
  progress.log   one line per step, with times
  summary.md     what changed, what was checked, what is left
  notes.md       decisions and their reasons
  evidence/      the logs and receipts behind each claim
```

- **The brief** is written before the worker starts, by whoever hands out
  the work. It carries the goal as the requester meant it, what done looks
  like, the constraints with their reasons, where the result goes, and
  links to the sources. It does not prescribe steps a capable worker
  should choose for itself: the more a brief says, the more its guesses
  can pull the worker away from what was asked.
- **Status, not chat.** A long conversation gets summarized and loses
  detail. The status file is where the work stands now, rewritten as it
  changes: the next step, open questions with options and a
  recommendation, decisions with their reasons, blockers, follow-ups.
  Someone picking the task up reads the brief and the status together,
  then follows the evidence for any claim they need to check; they should
  not need the chat transcript.
- **Evidence next to every claim.** The summary says what changed, what
  was checked and what is left; the evidence folder holds the logs and
  receipts that back each sentence. A reviewer reads the evidence. A
  confident summary without evidence is a claim, not a result.
- **Decisions with reasons.** Notes record what was chosen, why, and what
  would justify revisiting it. The reason is what lets a later reader
  check the call or challenge it. A proposal is marked as a proposal; a
  worker's call is never written as the owner's decision.
- **Corrections.** When a task finds that a recorded fact was wrong, it
  records the correction with its evidence and points at the copies that
  need fixing, so the wrong fact does not survive in another document.
