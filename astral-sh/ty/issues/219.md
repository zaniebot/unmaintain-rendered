```yaml
number: 219
title: Configuration
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2025-01-15T09:59:50Z
updated_at: 2025-06-18T14:16:29Z
url: https://github.com/astral-sh/ty/issues/219
synced_at: 2026-01-10T02:08:20Z
```

# Configuration

---

_Issue opened by @MichaReiser on 2025-01-15 09:59_

Support configuring Red Knot in a persistent configuration:

* [x] Configuration in the project's `pyproject.toml` or `knot.toml`. 
* [ ] Configuration sections:
  * [x] `rules`: enable and disable rules
  * [x] `environment`: (python version, platform, search paths)
  * [x] `files`: Override a sub-set of settings on a path level
  * [x] `src`: Which files should be considered first-party and what's the src root
  * [x] `terminal`: Configure color output, etc. 
  * [x] root options (for now, only `respect-ignore-files`)
  * [x] `requires-python` (I'm leaning towards just making it a field on `ProjectMetadata` because we only respect the `requires-python` constraint in the project's configuration file)
* [x] Path anchoring in configuration files (relative paths are relative to the current working directory, the configuration file, or the project root)
* [x] Track the source of a configuration value for better error reporting: E.g. clippy tells you why a rule is enabled. Is it because the rule is enabled by default or because you enabled it in the configuration or in the CLI? Ideally, we'd have this for all options so that we can refer users to the right configuration file if we detect an invalid setting. 
* [x] User-level configuration 
* [x] Support overriding configuration options from the CLI
* [ ] Hierarchical configuration (`knot.toml` in any ancestor directory)
* [x] Generate a JSON schema

---

_Assigned to @MichaReiser by @MichaReiser on 2025-01-15 10:00_

---

_Comment by @hauntsaninja on 2025-01-21 08:17_

Note mypy has gotten some use of being able to configure things on a module level, not path level. In particular, this works well for settings related to specific dependencies (e.g. ignoring them or ignoring their possible non-existence or ignoring PEP 561 warnings or setting implicit export behaviour etc etc)

---

_Comment by @MichaReiser on 2025-01-21 08:25_

Interesting. Overall, we don't plan on emitting warnings for third-party code (we don't check third party code). Allowing non-existent modules is its own setting that only takes module names and the same is probably true for PEP 561. I'd have to look into implicit export behavior more closely. 



---

_Renamed from "[red-knot] Persistent configuration" to "[red-knot] Configuration" by @MichaReiser on 2025-03-14 09:58_

---

_Comment by @thejchap on 2025-04-22 12:31_

@carljm @MichaReiser are any of these decent candidates for an outside contributor? I'd be interested in picking one or two up - perhaps file inclusion/exclusion?

---

_Comment by @MichaReiser on 2025-04-22 12:33_

There's some design work left around file inclusion/exclusion, which makes it a not so good external contributor task. The same is true for `files`. The only one left is `respect-ignore-files` but that's sort of trivial and probably not very interesting :(

---

_Comment by @thejchap on 2025-04-22 13:34_

@MichaReiser trivial and uninteresting sounds like a good first issue :) don't think I see a subtask for that one - if you want to provide some quick context here or in a subtask I'll get a PR up

Also in general this is a project I'm interested in - if other things are ready for execution feel free to tag me

---

_Comment by @MichaReiser on 2025-04-23 06:52_

The basic idea is to add a `--respect-ignore-files` CLI and `knot.respect-ignore-files` setting which controls whether `walker.standard_filters` is set to true or false in

https://github.com/astral-sh/ruff/blob/c4578162d555a1555d4c7719272dbac5ebaaef17/crates/red_knot_project/src/walk.rs#L137

The behavior should be similar to https://github.com/astral-sh/ruff/blob/a192d96880399413432a0316e24ce7982cbc3cb2/crates/ruff_workspace/src/resolver.rs#L486-L487


A slightly more involved feature would be https://github.com/astral-sh/ty/issues/218

---

_Comment by @carljm on 2025-04-24 00:45_

@thejchap you can also look for issues tagged `help-wanted` -- looks like currently we are short of those (thanks to a lot of helpful contributors!) but I'll try to keep an eye out for new issues we could tag that way.

---

_Renamed from "[red-knot] Configuration" to "Configuration" by @MichaReiser on 2025-05-07 15:27_

---

_Label `configuration` added by @AlexWaygood on 2025-05-11 07:49_

---

_Comment by @MichaReiser on 2025-06-18 14:16_

I'll close this as completed. We can track the other changes in the corresponding issues.

---

_Closed by @MichaReiser on 2025-06-18 14:16_

---
