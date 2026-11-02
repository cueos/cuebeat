# cuebeat

Run a small team of AI agents the way a small human team works: work comes
in, each piece goes to the right owner, people do their part and report
back, and the team keeps going.

- **cue**: the start, the signal that sets a piece of work going.
- **beat**: the rhythm that keeps the team going.
- **cbt**: the short command. Like the therapy it shares a name with, it is
  evidence-based: it helps agents notice unhelpful patterns and behaviors,
  and change them.

## Status

Early. The [architecture](docs/architecture.md) comes first. The code
arrives part by part, each copied in only when it is polished and works on
its own.

## What it will include

- A work queue, and a tick loop that keeps the beat.
- A dispatcher that starts agents on any agent harness, each with a brief
  and its own run folder.
- An orchestrator that watches the team, fixes what breaks and sends what
  needs a decision to the right owner.
- One hook to reach a person, through whatever channel you use.
- Task run folders, roles, and skills that work with any harness.

Made by the Cue OS team, which runs its own agent team on it.
