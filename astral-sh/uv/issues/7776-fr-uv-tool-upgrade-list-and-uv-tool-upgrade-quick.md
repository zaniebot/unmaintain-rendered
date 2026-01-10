```yaml
number: 7776
title: "FR: `uv tool upgrade --list` and `uv tool upgrade --quick` (?)"
type: issue
state: open
author: ilyagr
labels: []
assignees: []
created_at: 2024-09-29T05:39:48Z
updated_at: 2024-09-29T05:39:48Z
url: https://github.com/astral-sh/uv/issues/7776
synced_at: 2026-01-10T04:45:10Z
```

# FR: `uv tool upgrade --list` and `uv tool upgrade --quick` (?)

---

_Issue opened by @ilyagr on 2024-09-29 05:39_

It would be nice if I could find out whether any of my installed tools have upgrades available.

Relatedly, currently `uv tool upgrade` upgrades every tool's dependencies even if the tools themselves cannot be upgraded. I would prefer an option (e.g. `--quick`) where the command does nothing unless the tool itself can be upgraded to a new version. Less importantly, there could also be more advanced options that update the minimum number of packages to upgrade to the new version of the tool.

More fancifully, even with `--quick`, `uv tool upgrade` could upgrade dependencies for which there is a security advisory. (And it could even downgrade if there is a security advisory about the newest version). But that is probably a separate feature.

---
