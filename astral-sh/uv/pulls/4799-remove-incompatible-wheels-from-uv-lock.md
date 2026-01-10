```yaml
number: 4799
title: "Remove incompatible wheels from `uv.lock`"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/remove-incompatible-wheels
created_at: 2024-07-04T10:53:07Z
updated_at: 2024-07-08T17:40:56Z
url: https://github.com/astral-sh/uv/pull/4799
synced_at: 2026-01-10T13:42:52Z
```

# Remove incompatible wheels from `uv.lock`

---

_Pull request opened by @konstin on 2024-07-04 10:53_

Remove wheels from the lockfile that don't match the required python version. For example, we remove `charset_normalizer-3.3.2-cp311-cp311-win_amd64.whl` when we have `requires-python = ">=3.12"`.

Our snapshots barely show changes since we avoid the large binaries for which matters. Here are 3 real world `uv.lock` before/after comparisons to show the large difference:
* [warehouse](https://gist.github.com/konstin/9a1ed6a32b410e250fcf4c6ea8c536a5) (5677 -> 4214)
* [transformers](https://gist.github.com/konstin/5636281b5226f64aa44ce3244d5230cd) (6484 -> 5816)
* [github-wikidata-bot](https://gist.github.com/konstin/ebbd7b9474523aaa61d9a8945bc02071) (793 -> 454)

We only remove wheels we are certain don't match the python version and still keep those with unknown tags. We could remove even more wheels by also considering other markers, e.g. removing linux wheels for a windows-only dep, but we would trade complex, easy-to-get-wrong logic for diminishing returns.

---

_Label `enhancement` added by @konstin on 2024-07-04 10:53_

---

_Label `preview` added by @konstin on 2024-07-04 10:53_

---

_Review requested from @BurntSushi by @konstin on 2024-07-04 10:53_

---

_Review requested from @charliermarsh by @konstin on 2024-07-04 10:53_

---

_@charliermarsh approved on 2024-07-04 18:03_

Nice.

---

_Merged by @charliermarsh on 2024-07-04 18:03_

---

_Closed by @charliermarsh on 2024-07-04 18:03_

---

_Branch deleted on 2024-07-04 18:03_

---

_@BurntSushi reviewed on 2024-07-08 17:40_

I love this.

---
