```yaml
number: 7371
title: "Add `--no-editable` support to `uv sync` and `uv export`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/non-editable
created_at: 2024-09-13T18:06:11Z
updated_at: 2024-09-17T14:50:37Z
url: https://github.com/astral-sh/uv/pull/7371
synced_at: 2026-01-10T12:53:45Z
```

# Add `--no-editable` support to `uv sync` and `uv export`

---

_Pull request opened by @charliermarsh on 2024-09-13 18:06_

## Summary

Closes https://github.com/astral-sh/uv/issues/5792.


---

_Marked ready for review by @charliermarsh on 2024-09-13 18:12_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-13 18:12_

---

_Review requested from @konstin by @charliermarsh on 2024-09-13 18:12_

---

_Comment by @charliermarsh on 2024-09-13 18:12_

Avoiding docs until we have consensus on the CLI (flag name, etc.).

---

_Label `enhancement` added by @charliermarsh on 2024-09-13 18:12_

---

_Review comment by @konstin on `crates/uv/tests/export.rs`:911 on 2024-09-13 19:54_

Do we need to disambiguate this path, to make sure it doesn't install `child` from pypi?

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2411 on 2024-09-13 19:58_

It isn't clear here what a "local" dependency is, can we just say something like "Convert any editable dependencies, including the project and any workspace members, to non-editable dependencies"?

---

_@konstin approved on 2024-09-13 19:59_

---

_@charliermarsh reviewed on 2024-09-13 20:15_

---

_Review comment by @charliermarsh on `crates/uv/tests/export.rs`:911 on 2024-09-13 20:15_

Yes definitely, thank you.

---

_Review request for @zanieb removed by @charliermarsh on 2024-09-17 00:22_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-17 00:22_

---

_Comment by @zanieb on 2024-09-17 03:51_

Okay I know I said I was worried about this before, but why not `--no-editable`?

Our existing usage is `uv add --no-editable ./path/foo` which... adds the dependency and disables editable installs. I feel like `uv sync --no-editable` is fine? We have different semantics for excluding packages during sync and export, e.g., `--no-install-<thing>` and `--no-emit-<thing>` so I think there's already a nice separation of concepts there. 

I guess the complexity is overlap with like `--no-dev` which actually drops an entire group of packages but I can't think of why anyone would want to drop all the editable packages and their dependencies in that way? Is there a use-case where you'd want that that's not better addressed by `--no-install-project` etc.?

---

_Comment by @charliermarsh on 2024-09-17 12:31_

Yeah my main concern with `--no-editable` is that it sounds like it omits the editables, rather than including those dependencies but installing them as non-editable.

---

_Comment by @charliermarsh on 2024-09-17 13:08_

I'm fine with it though, I don't feel strongly.

---

_Comment by @zanieb on 2024-09-17 13:13_

I don't think "the editable packages" is a group of packages we would allow selecting, rather it's an installation mode you can toggle. Let's just run with it. If there was a better alternative I'd be all for it, but I can't think of anything that nearly as obvious as `--no-editable`.

---

_Comment by @charliermarsh on 2024-09-17 13:30_

Ok, I'll change it. Don't feel strongly, both seem ok to me.

---

_Comment by @charliermarsh on 2024-09-17 13:48_

Changed to `--no-editable`.

---

_Renamed from "Add `--non-editable` support to `uv sync` and `uv export`" to "Add `--no-editable` support to `uv sync` and `uv export`" by @zanieb on 2024-09-17 14:35_

---

_@zanieb approved on 2024-09-17 14:35_

---

_Merged by @charliermarsh on 2024-09-17 14:50_

---

_Closed by @charliermarsh on 2024-09-17 14:50_

---

_Branch deleted on 2024-09-17 14:50_

---
