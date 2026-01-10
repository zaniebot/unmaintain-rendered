```yaml
number: 10364
title: uv sync ignores pytest upper bound in GitHub actions
type: issue
state: closed
author: pchalasani
labels: []
assignees: []
created_at: 2025-01-07T14:42:36Z
updated_at: 2025-01-07T15:16:08Z
url: https://github.com/astral-sh/uv/issues/10364
synced_at: 2026-01-10T04:36:21Z
```

# uv sync ignores pytest upper bound in GitHub actions

---

_Issue opened by @pchalasani on 2025-01-07 14:42_

I'm trying to move [langroid](https://github.com/langroid/langroid) from poetry -> uv, and I noticed an issue -- even though the [`pyproject.toml`](https://github.com/langroid/langroid/blob/339df3fd1192004d9d0b32e2fbad56168162487b/pyproject.toml) specifies `"pytest<8.0.0,>=7.3.1"`, when the [pytest.yml](https://github.com/langroid/langroid/actions/runs/12641599213/workflow) is run in GitHub actions, it installs [`pytest==8.3.4`](https://github.com/langroid/langroid/actions/runs/12641599213/job/35224317628#step:8:234), which then causes test failure since the [`--cov` option is not recognized](https://github.com/langroid/langroid/actions/runs/12641599213/job/35224317628#step:14:43)

I'm curious if this is a known issue or I should be using a different mechanism to ensure version bounds are respected?

Thanks

---

_Comment by @pchalasani on 2025-01-07 15:16_

Please ignore, we were using an older version of `uv` which didn't recognize the `--dev` flag.

---

_Closed by @pchalasani on 2025-01-07 15:16_

---
