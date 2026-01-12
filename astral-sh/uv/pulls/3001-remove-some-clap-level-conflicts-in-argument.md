```yaml
number: 3001
title: Remove some Clap-level conflicts in argument groups
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/arg
created_at: 2024-04-11T22:01:55Z
updated_at: 2024-04-12T19:51:18Z
url: https://github.com/astral-sh/uv/pull/3001
synced_at: 2026-01-12T16:05:21Z
```

# Remove some Clap-level conflicts in argument groups

---

_@charliermarsh_

## Summary

It turns out that if you have an environment variable set, Clap will consider that equivalent to passing the flag, even if it's set to (e.g.) something falsy or the default value.

So, e.g., this fails:

```shell
UV_SYSTEM=false uv pip install --python ./.venv/bin/python flask
```

Worse, this fails, because it thinks `--no-index` and `--index-url` are conflicting:

```shell
export UV_INDEX_URL=https://google.com
uv pip install flask --no-index
```

This PR removes some of the conflicts, namely those related to environment variables, such that:

- You _can_ pass mixes of `--no-index`, `--index-url`, etc. If `--no-index` is provided, all the index URLs will be ignored (but we won't error).
- Passing `--pre` will always enable prereleases, even if `--prerelease` is also provided. (We could warn here, although honestly it's not trivial because we'd need to make `--prerelease` take an optional, then we'd lose the default argument from the `--help`.)
- You _can_ pass `--system` and `--python`. If `--python` is provided, we use that, and ignore `--system`. (We could warn here.)

I guess the underlying problem here is that we can't differentiate between arguments passed on the CLI and those set as environment variables. But making bigger changes here seems out-of-scope.

Closes https://github.com/astral-sh/uv/issues/3000.


---

_Label `bug` added by @charliermarsh on 2024-04-11 22:02_

---

_Label `cli` added by @charliermarsh on 2024-04-11 22:02_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-11 22:02_

---

_Marked ready for review by @charliermarsh on 2024-04-11 22:02_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-11 22:09_

---

_@konstin approved on 2024-04-12 06:45_

---

_@BurntSushi approved on 2024-04-12 13:40_

This seems like an improvement to me.

Does this take the order of flags into account? e.g., Are `--no-index --index-url ...` and `--index-url ... --no-index` the same? I think arguably the order should determine which one "wins." IIRC, this was difficult to do in Clap unfortunately. (Or at least, difficult to do in Clap 2. I'm unsure about Clap 4.)

---

_Comment by @charliermarsh on 2024-04-12 13:42_

It doesn't, though in that specific case I actually think `--no-index` should still win...

---

_Comment by @charliermarsh on 2024-04-12 13:43_

In general though, I agree that taking order into account would help here.

---

_Merged by @charliermarsh on 2024-04-12 19:51_

---

_Closed by @charliermarsh on 2024-04-12 19:51_

---

_Branch deleted on 2024-04-12 19:51_

---
