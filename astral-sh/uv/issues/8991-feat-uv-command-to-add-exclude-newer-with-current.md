```yaml
number: 8991
title: "feat: uv command to add `exclude-newer` with current date"
type: issue
state: open
author: tlambert03
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-11-10T16:45:19Z
updated_at: 2024-11-26T16:24:45Z
url: https://github.com/astral-sh/uv/issues/8991
synced_at: 2026-01-12T15:59:40Z
```

# feat: uv command to add `exclude-newer` with current date

---

_@tlambert03_

Apologies if I missed an existing feature:
I love the `exclude-newer` feature and wonder where there is a convenience to programmatically populate the exclude-newer field with the "current" timestamp:
```
# [tool.uv]
# exclude-newer = "<RFC 3339 NOW>"
```

(something to save looking up the current timestamp and manually editing the file).
Looks like using something like `uv add --script myscript.py --exclude-newer "2023-10-16T00:00:00Z" some-package` doesn't add the tool.uv section

*edit*: 
to clarify, I'm looking for some command:
```sh
uv <command> <target.py | project>
```

that would add `exclude-newer = <NOW>` to the target

---

_Comment by @tlambert03 on 2024-11-10 17:13_

probably related to the hypothetical `uv config set` mentioned in https://github.com/astral-sh/uv/pull/8553 

---

_Comment by @zanieb on 2024-11-11 15:06_

Hm. We could write `exclude-newer` during `uv add` without a `uv config set` feature.

---

_Label `enhancement` added by @zanieb on 2024-11-11 15:07_

---

_Label `cli` added by @zanieb on 2024-11-11 15:07_

---

_Comment by @tlambert03 on 2024-11-11 15:34_

that would be cool.  I guess the partial "mismatch" there is that the `exclude-newer = ...` line that would be added to the config section really doesn't have any association with the specific package that was added with `uv add`.  And if someone wanted to "repin" the `exclude-newer` date to the current date, they would need to pick an existing package to re-add it just so that they could add the `--exclude-newer` flag, right?   Do you agree that it's a bit of a conceptual mismatch?

---

_Comment by @zanieb on 2024-11-11 15:51_

I don't think `uv add --exclude-newer` applies the exclusion to a single package — I think that option is global to the resolution. It's possible the behavior is different when we're reading a previous resolution from the lockfile, but I'm not sure.

---

_Comment by @zanieb on 2024-11-11 15:52_

cc @charliermarsh who is likely to know how that interacts

---

_Comment by @tlambert03 on 2024-11-11 15:53_

> I don't think uv add --exclude-newer applies the exclusion to a single package — I think that option is global to the resolution.

that's exactly my point.  and that's why i'm suggesting that it would be a little bit of an awkward/misleading solution

---

_Comment by @kolibril13 on 2024-11-26 09:15_

there is [juv](https://github.com/manzt/juv) (A toolkit for reproducible Jupyter notebooks, powered by uv) by @manzt   
which has a `stamp` command to pin dependencies.
```
# Pin a timestamp to constrain dependency resolution to a specific date
juv stamp notebook.ipynb # now
```
https://github.com/manzt/juv?tab=readme-ov-file#usage

maybe this can be an inspiration for a potential `--exclude-newer` flag.

---

_Comment by @manzt on 2024-11-26 12:44_

> maybe this can be an inspiration for a potential --exclude-newer flag.

There is discussion about this in [the PR](https://github.com/astral-sh/uv/pull/8553) linked above.

fwiw, for now `juv stamp` also works for scripts (not only notebooks), and you can invoke it with `uvx`.

```sh
# Stamp “now”
uv init —script foo.py
uvx juv stamp foo.py

# a specific (resolves to system tz)
uvx juv stamp foo.py --date 2022-01-03

# a git commit
uvx juv stamp foo.py --rev e20c99
uvx juv stamp foo.py --latest

# Clear the pinned timestamp
uvx juv stamp foo.py --clear
```

---
