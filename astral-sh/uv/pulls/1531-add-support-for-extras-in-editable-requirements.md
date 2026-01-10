```yaml
number: 1531
title: Add support for extras in editable requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/editable
created_at: 2024-02-16T19:26:27Z
updated_at: 2024-02-18T07:09:43Z
url: https://github.com/astral-sh/uv/pull/1531
synced_at: 2026-01-10T15:33:24Z
```

# Add support for extras in editable requirements

---

_Pull request opened by @charliermarsh on 2024-02-16 19:26_

## Summary

If you're developing on a package like `attrs` locally, and it has a recursive extra like `attrs[dev]`, it turns out that we then try to find the `attrs` in `attrs[dev]` from the registry, rather than recognizing that it's part of the editable.

This PR fixes the issue by making editables slightly more first-class throughout the resolver. Instead of mocking metadata, we explicitly check for extras in various places. Part of the problem here is that we treated editables as URL dependencies, but when we saw an _extra_ like `attrs[dev]`, we didn't map that back to the URL. So now, we treat them as registry dependencies, but with the appropriate guardrails throughout.

Closes https://github.com/astral-sh/uv/issues/1447.

## Test Plan

- Cloned `attrs`.
- Ran `cargo run venv && cargo run pip install -e ".[dev]" -v`.


---

_@charliermarsh reviewed on 2024-02-16 19:27_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:170 on 2024-02-16 19:27_

I think removing this is good.

---

_@charliermarsh reviewed on 2024-02-16 19:27_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:629 on 2024-02-16 19:27_

The only downside to all of this is that we now pay the cost of doing these lookups for all packages.

---

_Review requested from @zanieb by @charliermarsh on 2024-02-16 19:31_

---

_Label `bug` added by @charliermarsh on 2024-02-16 19:31_

---

_Comment by @matthewfeickert on 2024-02-16 21:13_

Ah, thank you very much for this! I was just about to report it given

```
uv pip install --upgrade ".[all,test]"
...
error: Failed to parse `.[all,test]`
  Caused by: Expected package name starting with an alphanumeric character, found '.'
.[all,test]
```

. :)

(Go Astral team go! :rocket: :star:)

---

_@zanieb approved on 2024-02-16 23:32_

---

_Merged by @charliermarsh on 2024-02-16 23:48_

---

_Closed by @charliermarsh on 2024-02-16 23:48_

---

_Branch deleted on 2024-02-16 23:48_

---

_Comment by @matthewfeickert on 2024-02-18 01:24_

@charliermarsh @zanieb we're still seeing this in https://github.com/scikit-hep/pyhf when we try to do

```console
$ uv --version
uv 0.1.4
$ python -m uv pip install --upgrade ".[all,test]"
error: Failed to parse `.[all,test]`
  Caused by: Expected package name starting with an alphanumeric character, found '.'
.[all,test]
^
$
```

---

_Comment by @charliermarsh on 2024-02-18 01:49_

That’s a slightly different issue — you can either use -e (like -e “.[all,test]”) or provide a package name like “pyhf @ .[all,test]”. We have a limitation that we require package names upfront for non-editable requirements, but we’ll be lifting that in the future.

---

_Comment by @matthewfeickert on 2024-02-18 07:09_

Ah okay great. I was alreadying watching Issue https://github.com/astral-sh/uv/issues/313 but for some reason hadn't made the connection. Thanks!

---
