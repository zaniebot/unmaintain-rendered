```yaml
number: 10241
title: Add a configuration option for default init options
type: issue
state: open
author: m-legrand
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-12-30T14:58:37Z
updated_at: 2024-12-30T17:37:14Z
url: https://github.com/astral-sh/uv/issues/10241
synced_at: 2026-01-12T16:00:09Z
```

# Add a configuration option for default init options

---

_@m-legrand_

_This is a feature request_

I like `uv init` to create simple ad-hoc projects and often use it with the exact same options.
I would love to be able to drive this via a configuration, such as a `uv.toml` in a "one-off apps" folder.

For consistency with other subcommands it could look like this:

```uv.toml
# uv.toml

[init]
app = true
vcs = "none"
```

---

_Label `configuration` added by @charliermarsh on 2024-12-30 17:37_

---

_Label `needs-decision` added by @charliermarsh on 2024-12-30 17:37_

---
