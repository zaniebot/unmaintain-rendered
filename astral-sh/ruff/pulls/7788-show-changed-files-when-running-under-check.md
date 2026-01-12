```yaml
number: 7788
title: "Show changed files when running under `--check`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/check
created_at: 2023-10-03T18:19:49Z
updated_at: 2023-10-03T18:57:51Z
url: https://github.com/astral-sh/ruff/pull/7788
synced_at: 2026-01-12T15:55:24Z
```

# Show changed files when running under `--check`

---

_@charliermarsh_

## Summary

We now list each changed file when running with `--check`.

Closes https://github.com/astral-sh/ruff/issues/7782.

## Test Plan

```
❯ cargo run -p ruff_cli -- format foo.py --check
   Compiling ruff_cli v0.0.292 (/Users/crmarsh/workspace/ruff/crates/ruff_cli)
rgo +    Finished dev [unoptimized + debuginfo] target(s) in 1.41s
     Running `target/debug/ruff format foo.py --check`
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
Would reformat: foo.py
1 file would be reformatted
```


---

_Review requested from @zanieb by @charliermarsh on 2023-10-03 18:19_

---

_Review requested from @konstin by @charliermarsh on 2023-10-03 18:19_

---

_Comment by @zanieb on 2023-10-03 18:25_

What's this look like for multiple files?

---

_Comment by @charliermarsh on 2023-10-03 18:26_

It shows one line (like the above) per file:

```
File is unformatted: bar.py
File is unformatted: foo.py
2 files would be reformatted
```

(The filenames are bolded.)

---

_Comment by @zanieb on 2023-10-03 18:35_

And we don't mention the files that are correctly formatted? Seems reasonable to me. Should we say "Would reformat: foo.py" instead?

---

_Comment by @github-actions[bot] on 2023-10-03 18:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-03 18:40_

Changed!

---

_@zanieb approved on 2023-10-03 18:42_

LGTM; why the refactor?

---

_Comment by @charliermarsh on 2023-10-03 18:45_

Which part? Overall, I needed to store the results with paths so that we could report on individual paths.

The `impl<P: AsRef<Path>> From<P>` change was necessary to allow passing `&path` to the `SourceKind::from` calls rather than `path.as_path()` -- the `.as_path()` shouldn't be necessary.

---

_Merged by @charliermarsh on 2023-10-03 18:50_

---

_Closed by @charliermarsh on 2023-10-03 18:50_

---

_Branch deleted on 2023-10-03 18:50_

---
