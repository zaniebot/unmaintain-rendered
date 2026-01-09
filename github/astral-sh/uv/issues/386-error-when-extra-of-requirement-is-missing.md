---
number: 386
title: Error when extra of requirement is missing
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-10T13:29:26Z
updated_at: 2023-12-13T17:36:29Z
url: https://github.com/astral-sh/uv/issues/386
synced_at: 2026-01-07T13:12:16-06:00
---

# Error when extra of requirement is missing

---

_Issue opened by @konstin on 2023-11-10 13:29_

I tried to resolve `torch[transformers]` (it's the other way round) and got a very confusing error:

```
cargo run --bin puffin-dev -- resolve-cli "torch[tranformers]"
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/puffin-dev resolve-cli 'torch[tranformers]'`
puffin-dev failed
  Caused by: No solution found when resolving: torch[tranformers]
  Caused by: Because there is no version of torch[tranformers] available matching <1.7.1, >1.7.1, <1.8.0, >1.8.0, <1.8.1, >1.8.1, <1.9.0, >1.9.0, <1.9.1, >1.9.1, <1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 and dependencies of torch[tranformers] at version ==1.7.1 are unavailable, torch[tranformers]<1.8.0, >1.8.0, <1.8.1, >1.8.1, <1.9.0, >1.9.0, <1.9.1, >1.9.1, <1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==1.8.0 are unavailable and dependencies of torch[tranformers] at version ==1.8.1 are unavailable, torch[tranformers]<1.9.0, >1.9.0, <1.9.1, >1.9.1, <1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==1.9.0 are unavailable and dependencies of torch[tranformers] at version ==1.9.1 are unavailable, torch[tranformers]<1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==1.10.0 are unavailable and dependencies of torch[tranformers] at version ==1.10.1 are unavailable, torch[tranformers]<1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==1.10.2 are unavailable and dependencies of torch[tranformers] at version ==1.11.0 are unavailable, torch[tranformers]<1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==1.12.0 are unavailable and dependencies of torch[tranformers] at version ==1.12.1 are unavailable, torch[tranformers]<1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==1.13.0 are unavailable and dependencies of torch[tranformers] at version ==1.13.1 are unavailable, torch[tranformers]<2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==2.0.0 are unavailable and dependencies of torch[tranformers] at version ==2.0.1 are unavailable, torch[tranformers]<2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[tranformers] at version ==2.1.0 are unavailable and root ==0a0.dev0 depends on torch[tranformers], version solving failed.
```

Compared to pip-compile:
```
echo "torch[transformers]" | pip-compile --output-file - -
  WARNING: torch 2.1.0 does not provide the extra 'transformers'
[...]
```


---

_Referenced in [astral-sh/uv#309](../../astral-sh/uv/issues/309.md) on 2023-11-10 13:30_

---

_Comment by @charliermarsh on 2023-11-10 14:34_

We should add a test for this.

---

_Label `bug` added by @charliermarsh on 2023-11-13 14:40_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-13 14:41_

---

_Comment by @charliermarsh on 2023-12-04 04:37_

@zanieb - This is another concrete example where it'd be nice to have a custom error message on failure.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-12 22:20_

---

_Referenced in [astral-sh/uv#627](../../astral-sh/uv/pulls/627.md) on 2023-12-13 16:37_

---

_Closed by @charliermarsh on 2023-12-13 17:36_

---
