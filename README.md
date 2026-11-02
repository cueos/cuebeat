# cuebeat

Run a small team of AI agents the way a small human team works: work comes
in, each piece goes to the right owner, agents do their part in their own
folder and report back, and the team keeps going when you stop watching.

This repository documents how we run that team: how work gets an owner,
how results return, and how stalled work is noticed. You can use the
design with your own tools; executable parts may follow here later.

- **cue**: the start, the signal that sets a piece of work going.
- **beat**: the rhythm that keeps the team going.
- **cbt**: the planned short command. Like the therapy it shares a name
  with, it is evidence-based: it helps agents notice unhelpful patterns
  and behaviors, and change them.

Made by the Cue OS team, which runs its own agent team on it.

## Every task keeps its record

Each task has a run folder, so a person or another agent can check its
work later:

```text
runs/<task>/
  brief.md       what to do and why, written before it started
  status.md      where it stands, kept current by the worker
  progress.log   one line per step, with times
  summary.md     what changed, what was checked, what is left
  notes.md       decisions and their reasons
  evidence/      the logs and receipts behind each claim
```

## How it works

- [Architecture](docs/architecture.md): the whole design on one page: the
  core loop, who plays each part step by step, the rules that keep work
  moving, the parts, the orchestrator, the limits, what comes next.
- [How-to](docs/how-to.md): writing a role file, the connector contract,
  how context is generated, keeping a run folder.

## Status

This repository holds the ideas and the design behind a system that runs
our team today. The code is being cleaned up in our internal tool and will
follow here part by part. Pull requests are off for now; issues are open.

## License

[MIT](LICENSE).
