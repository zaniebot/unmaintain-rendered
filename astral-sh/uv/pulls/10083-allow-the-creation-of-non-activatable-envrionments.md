```yaml
number: 10083
title: Allow the creation of non-activatable envrionments
type: pull_request
state: open
author: nathanscain
labels: []
assignees: []
draft: true
base: main
head: feature/no-activate-scripts
created_at: 2024-12-21T18:08:53Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/10083
synced_at: 2026-01-10T11:10:34Z
```

# Allow the creation of non-activatable envrionments

---

_Pull request opened by @nathanscain on 2024-12-21 18:08_

## Summary

To resolve #10070 by allow environments to be created without activation scripts in them.

Saves a bit of time when only uv will be used to interact with the environment meant. This is off by default for user-facing environments (`uv venv` and anything calling `get_or_init_environment`) and on by default for internal environments so no time is wasted in environments that would never be directly interacted with (tool installs, isolated calls to uv run, package builds with build isolation, etc). Saves a bit of time writing files for everyone in those isolated envrionments, while letting those who want it to go more aggressive.

## Test Plan

All current tests pass. Will probably need to work with you on how you'd like this tested.

Locally, I have verified that:

- `uv venv` creates an environment with activation scripts
- `uv venv --not-activatable` does not create an environment with activation scripts
- `uv run` when there isn't an existing environment, creates one with activation scripts
- `uv run` when there is an existing environment without activation scripts, leaves the environment without activation scripts
- when the default behavior is to not have activation scripts, `uv venv --activatable` creates a virtual environment with activation scripts
- selecting the environment in vscode appears unaffected by the missing scripts

## TODO

Still need to make this configurable as a `uv.toml` / `pyproject.toml` option, but the core behavior is working so I figured I'd throw in draft to make sure I'm on the right path.


---
