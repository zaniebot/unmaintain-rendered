```yaml
number: 1311
title: Adds support for --no-deps to pip compile
type: pull_request
state: merged
author: mitsuhiko
labels:
  - enhancement
assignees: []
merged: true
base: main
head: feature/compile-no-deps
created_at: 2024-02-15T08:39:35Z
updated_at: 2024-02-15T14:56:54Z
url: https://github.com/astral-sh/uv/pull/1311
synced_at: 2026-01-12T16:04:35Z
```

# Adds support for --no-deps to pip compile

---

_@mitsuhiko_

Mostly throwing this up here as a discussion topic. Having something like this is primarily useful for enabling use cases similar to `rye add` where I want to use this currently. One can accomplish something similar with `unearth` today or by abusing regular `pip install`:

```
$ ~/.rye/self/bin/pip install --no-deps --dry-run flask --report - -q | jq '.install[0].metadata | {name, version}'
{
  "name": "Flask",
  "version": "3.0.2"
}
```

Another option would be to have a `puffin resolve` command or similar that works like `pip compile` without dependencies, takes the requirements as arguments and returns a line for each resolution. That would be a larger change.

---

_Converted to draft by @mitsuhiko on 2024-02-15 08:39_

---

_@BurntSushi approved on 2024-02-15 13:08_

LGTM!

---

_Marked ready for review by @mitsuhiko on 2024-02-15 13:38_

---

_Merged by @BurntSushi on 2024-02-15 14:01_

---

_Closed by @BurntSushi on 2024-02-15 14:01_

---

_Branch deleted on 2024-02-15 14:01_

---

_Label `enhancement` added by @BurntSushi on 2024-02-15 14:01_

---

_Comment by @charliermarsh on 2024-02-15 14:56_

👍 👍 

---
