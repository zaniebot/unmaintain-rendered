```yaml
number: 1342
title: Respect --force-exclude for files passed via stdin
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/stdin-force-exclude
created_at: 2022-12-22T21:29:01Z
updated_at: 2022-12-22T21:40:16Z
url: https://github.com/astral-sh/ruff/pull/1342
synced_at: 2026-01-12T05:36:31Z
```

# Respect --force-exclude for files passed via stdin

---

_Pull request opened by @charliermarsh on 2022-12-22 21:29_

Resolves #1339.

---

_Comment by @charliermarsh on 2022-12-22 21:30_

It turns out that it's very, very hard to use the `ignore` crate to answer the question, "Is a given path ignored?", because it doesn't _seem_ to expose the underlying matchers, only the directory walkers. This creates a few problems. I'll just leave this comment here for posterity as I roll back some of the code:

```
// Step 2: Is the file ignored via a gitignore?
// Note that we can't enforce this behavior for non-existent files, which _could_ come up when
// you pass a file via `--stdin-filename`, and _could_ be considered incorrect. For example,
// if `subdir` is listed in the `gitignore`, and you pass `subdir/non_existent_file.py`, then
// right now, we _wouldn't_ mark that theoretical file as ignored. If the `ignore` crate had
// a public matcher API, we could support that, but right now, we _have_ to look at the
// filesystem to reverse-engineer the gitignore match.
```

```
// TODO(charlie): This isn't right either, because the _parent_ could be the thing that's
// ignored via the `.gitignore`.
```

---

_Comment by @jhossbach on 2022-12-22 21:38_

Seems like I was too slow. Also closes #1343 

---

_Comment by @charliermarsh on 2022-12-22 21:40_

Sadly this doesn't respect gitignore rules in those cases (I tried to get this to work for a while, but I don't think it's reliable without changes to the `ignore` crate), but otherwise seems to work.

---

_Merged by @charliermarsh on 2022-12-22 21:40_

---

_Closed by @charliermarsh on 2022-12-22 21:40_

---

_Branch deleted on 2022-12-22 21:40_

---
