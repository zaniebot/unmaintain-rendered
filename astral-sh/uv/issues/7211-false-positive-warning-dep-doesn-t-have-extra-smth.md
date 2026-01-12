```yaml
number: 7211
title: "False positive warning: <dep> doesn't have extra <smth>"
type: issue
state: open
author: dimaqq
labels:
  - error messages
assignees: []
created_at: 2024-09-09T07:41:21Z
updated_at: 2024-09-16T01:51:11Z
url: https://github.com/astral-sh/uv/issues/7211
synced_at: 2026-01-12T15:59:11Z
```

# False positive warning: <dep> doesn't have extra <smth>

---

_@dimaqq_

Check out https://github.com/canonical/charmcraft

```
🦐/c/charmcraft (main)> uv venv .host-venv
Using Python 3.11.9
Creating virtualenv at: .host-venv
Activate with: source .host-venv/bin/activate.fish
🦐/c/charmcraft (main)> . .host-venv/bin/activate.fish
(.host-venv) 🦐/c/charmcraft (main)> uv pip install -e .
Resolved 67 packages in 627ms
Prepared 15 packages in 1.35s
░░░░░░░░░░░░░░░░░░░░ [0/67] Installing wheels...                                                                                                                                                                                                             warning: Failed to clone files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, reflinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 67 packages in 463ms
 + annotated-types==0.7.0
...
 + pydantic==2.9.0
 + pydantic-core==2.23.2
 + pydantic-yaml==1.3.0
...
 + pyyaml==6.0.2
...
 + zipp==3.20.1
warning: The package `pydantic-yaml==1.3.0` does not have an extra named `pyyaml`
```

Where did that warning come from?

The project specifies `pydantic-yaml` and `pyyaml` dependencies, but completely independently.

Very view extras are used in general and no extras are used in dependencies.

I'm happy to debug this further, though I'm not sure how.

---

_Comment by @smheidrich on 2024-09-09 11:14_

```
$ cd .venv
$ rg --no-ignore-parent '\[pyyaml'
craft_parts-2.0.0.dist-info/METADATA
22:Requires-Dist: pydantic-yaml[pyyaml]>=1.0.0
```

^ That package (craft-parts) is where it comes from in your specific case, but I agree that it would make sense for uv to report the package in whose dependencies the wrong extra was requested (perhaps even the whole chain of dependencies leading up the one with the wrong extra).

---

_Comment by @dimaqq on 2024-09-10 04:32_

Thank you @smheidrich I've added a bug report to that repo.

P.S. that's a handy ag/rg/grep to help diagnose problems like this.

---

_Label `error messages` added by @charliermarsh on 2024-09-16 01:51_

---
