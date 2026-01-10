```yaml
number: 9560
title: "Use `.yml` suffix consistently"
type: pull_request
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/yml
created_at: 2024-01-17T05:56:40Z
updated_at: 2024-01-17T17:45:28Z
url: https://github.com/astral-sh/ruff/pull/9560
synced_at: 2026-01-10T22:57:09Z
```

# Use `.yml` suffix consistently

---

_Pull request opened by @charliermarsh on 2024-01-17 05:56_

## Summary

We use `.yaml` for ~half of our YAML files, and `.yml` for the other half. This PR standardizes on `.yml`. I don't have a strong preference between the two, but `cargo-dist` prefers `.yml`, so seems easier to fall on that side of the argument.


---

_Label `internal` added by @charliermarsh on 2024-01-17 05:57_

---

_Comment by @MichaReiser on 2024-01-17 06:09_

Pre commit doesn't agree 🤣

---

_Comment by @charliermarsh on 2024-01-17 06:13_

Heavy sigh

---

_Comment by @johnslavik on 2024-01-17 06:19_

😭

---

_Comment by @zanieb on 2024-01-17 14:02_

fwiw I have the smallest of preferences for `yaml`.

---

_Comment by @charliermarsh on 2024-01-17 17:37_

Yeah me too. I'm probably going to change it anyway though, since cargo-dist requires it and it's not a strong enough preference for me to change my mind on a bigger tooling decision based on the suffix alone.

---

_Comment by @MichaReiser on 2024-01-17 17:41_

> Yeah me too. I'm probably going to change it anyway though, since cargo-dist requires it and it's not a strong enough preference for me to change my mind on a bigger tooling decision based on the suffix alone.

For which files does `cargo-dist` require `.yml`? Could we only change these files (yml is impossible to pronounce)

---

_Comment by @charliermarsh on 2024-01-17 17:44_

`release.yml` (which is auto-generated), and then the files that are automatically referenced as sub-steps (e.g., `publish-to-pypi.yml`).

---

_Comment by @charliermarsh on 2024-01-17 17:45_

I'll just close this then and bundle it with the `cargo-dist` PR. I figured it'd be easier to stage it separately, but seems not.

---

_Closed by @charliermarsh on 2024-01-17 17:45_

---
