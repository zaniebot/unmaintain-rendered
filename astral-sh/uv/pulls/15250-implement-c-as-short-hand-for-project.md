```yaml
number: 15250
title: Implement -C as short hand for --project
type: pull_request
state: open
author: nils-werner
labels: []
assignees: []
base: main
head: project-short-arg
created_at: 2025-08-13T09:24:05Z
updated_at: 2025-08-13T09:45:26Z
url: https://github.com/astral-sh/uv/pull/15250
synced_at: 2026-01-10T06:44:33Z
```

# Implement -C as short hand for --project

---

_Pull request opened by @nils-werner on 2025-08-13 09:24_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This MR implements the shorthand option `-C` for `--project`, so you can call

```
uv -C my/project/ run python
```

Many other prominent tools use this, like `make`, `git`, `cargo`, `poetry`, `meson`, `ninja` etc., and it would be great to see `uv` follow that convention, as proposed in #12040.

However, there is one small-ish technical hurdle: `-C` was recently also picked up as a shorthand for `uv pip install --config-settings` (#1460). These two options clash, because `clap` cannot disambiguate between global and subcommand options.

I could not find a way to deprecate `clap` options, so I simply removed the other occurrences for now, just to get started. Please understand that this was only done to get this MR out, and I see this MR as a conversation starter, not a s a finished proposal.

If you would like to adopt `-C/--project` (and I 100% think you should), what would be the best way forward to integrate this feature? I'd be happy to amend this MR to implement them.

## Test Plan

The corresponding tests strings were adjusted, no additional tests were added.


---
