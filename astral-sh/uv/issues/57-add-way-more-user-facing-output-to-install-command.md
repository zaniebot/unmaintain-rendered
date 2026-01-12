```yaml
number: 57
title: Add way more user-facing output to install command
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-08T13:55:48Z
updated_at: 2023-10-08T19:46:08Z
url: https://github.com/astral-sh/uv/issues/57
synced_at: 2026-01-12T15:58:21Z
```

# Add way more user-facing output to install command

---

_@charliermarsh_

We could either do something npm- and yarn-like (show a progress bar), or something pip-like (show output for every package, for everything we do).


---

_Comment by @charliermarsh on 2023-10-08 14:03_

Another datapoint -- Orogene looks like this:

```shell
❯ oro apply
 INFO Performing first-time setup...
 INFO Orogene is able to collect anonymous usage statistics and
 INFO crash reports to help the team improve the tool.
 INFO Anonymous, aggregate metrics are publicly available (see `oro telemetry`),
 INFO and no personally identifiable information is collected.
 INFO This is entirely opt-in, but we would appreciate it if you considered it!
✔ Do you wish to enable anonymous telemetry? · no

 INFO 🔍 Resolved 371 packages in 0.02s.
 INFO 🧹 Pruned 0 packages in 0.66s.
 INFO 📦 Extracted 370 packages in 5.401s.
 INFO 🏃 Ran lifecycle scripts in 0.168s.
 INFO 📝 Wrote lockfile to package-lock.kdl.
 INFO 🎉 Applied node_modules/ in 6.254s. Let the hacking commence!
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-08 14:51_

---

_Comment by @charliermarsh on 2023-10-08 14:57_

We could consider using tracing for user-facing and debug output, and allowing a flag like `--verbose` which toggles the output _format_ of tracing messages (to include timestamps in the `--verbose` case, etc.).

---

_Closed by @charliermarsh on 2023-10-08 19:46_

---
