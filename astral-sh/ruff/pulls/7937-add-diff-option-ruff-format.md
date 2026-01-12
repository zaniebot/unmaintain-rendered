```yaml
number: 7937
title: "Add `--diff` option `ruff format`"
type: pull_request
state: merged
author: konstin
labels:
  - cli
  - formatter
assignees: []
merged: true
base: main
head: format-diff
created_at: 2023-10-13T09:31:30Z
updated_at: 2023-10-18T12:01:55Z
url: https://github.com/astral-sh/ruff/pull/7937
synced_at: 2026-01-12T02:32:41Z
```

# Add `--diff` option `ruff format`

---

_Pull request opened by @konstin on 2023-10-13 09:31_

**Summary** `ruff format --diff` is similar to `ruff format --check`, but we don't only error with the list of file that would be formatted, but also show a diff between the unformatted input and the formatted output.

```console
$ ruff format --diff scratch.py scratch.pyi scratch.ipynb
warning: `ruff format` is not yet stable, and subject to change in future versions.
--- scratch.ipynb
+++ scratch.ipynb
@@ -1,3 +1,4 @@
 import numpy
-maths = (numpy.arange(100)**2).sum()
-stats= numpy.asarray([1,2,3,4]).median()
+
+maths = (numpy.arange(100) ** 2).sum()
+stats = numpy.asarray([1, 2, 3, 4]).median()
--- scratch.py
+++ scratch.py
@@ -1,3 +1,3 @@
 x = 1
-y=2
+y = 2
 z = 3
2 files would be reformatted, 1 file left unchanged
```

With `--diff`, the summary message gets printed to stderr to allow e.g. `ruff format --diff . > format.patch`.

At the moment, jupyter notebooks are formatted as code diffs, while everything else is a real diff that could be applied. This means that the diffs containing jupyter notebooks are not real diffs and can't be applied. We could change this to json diffs, but they are hard to read. We could also split the diff option into a human diff option, where we deviate from the machine readable diff constraints, and a proper machine readable, appliable diff output that you can pipe into other tools.

To make the tests work, the results (and errors, if any) are sorted before printing them. Previously, the print order was random, i.e. two identical runs could have different output.

Open question: Should this go into the markdown docs? Or will this be subsumed by the integration of the formatter into `ruff check`?

**Test plan** Fixtures for the change and no change cases, including a jupyter notebook and for file input and stdin.

Fixes #7231


---

_Label `formatter` added by @konstin on 2023-10-13 09:31_

---

_Comment by @github-actions[bot] on 2023-10-13 10:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-10-13 19:54_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:211 on 2023-10-13 19:54_

Can we make this unowned, by storing `&Path`? The lifetimes should work out, as far as I can tell.

---

_@charliermarsh reviewed on 2023-10-13 19:56_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:407 on 2023-10-13 19:56_

Nit: can probably remove this comment, it doesn't add much on top of the match guard itself (`FormatResult::Diff`).

---

_@charliermarsh reviewed on 2023-10-13 19:56_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:405 on 2023-10-13 19:56_

Why rename here, but leave `unformatted` as-is? Can we either leave them both, or rename them both, for consistency?

---

_@charliermarsh reviewed on 2023-10-13 19:59_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:226 on 2023-10-13 19:59_

This is making me wonder if `format_source` should take `&SourceKind`, and we should change `Unchanged(SourceKind)` to just `Unchanged`, and remove `unformatted` from the `Formatted` variant. It strikes me as inflexible that the method takes ownership of `SourceKind` but only acts on references and then returns the owned `SourceKind` in all cases. But, this is minor -- if it doesn't work for whatever reason, or you disagree, it's fine.

---

_@charliermarsh approved on 2023-10-13 19:59_

---

_Comment by @charliermarsh on 2023-10-13 20:00_

> Should this go into the markdown docs?

I think it's fine to leave this as-is for now -- I will fold into the documentation when I revisit it next week.

---

_Label `cli` added by @MichaReiser on 2023-10-16 01:54_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:109 on 2023-10-16 01:57_

```suggestion
    results.sort_unstable_by(|x, y| x.path.cmp(&y.path));
```

Using `unstable` should be fine because `results` should never contain two entries with the same path. Using unstable tends to be faster than stable sorting.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:147 on 2023-10-16 01:59_

Nit: To make it clear what is written to stderr. I first assumed it writes the diff to stderr.

```suggestion
                // Allow piping the diff to e.g. a file by writing the summary to stderr
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:226 on 2023-10-16 02:01_

I guess the challenge with this is that we need to move `SourceKind` out of the linter, maybe into `ruff_source_file`? I recommend investigating this as a separate PR.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:408 on 2023-10-16 02:03_

Should `stdout` be locked outside of the loop?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:426 on 2023-10-16 02:04_

Nit: It feels a bit strange that the diff is written as part of the `FormatSummary`'s `Display` implementation (the diff isn't part of the summary). 

I would prefer moving the diff printing out of the summary implementation and handle it explicitly when the `FormatMode` is `Diff` (also allows to locking `stdout` exactly once)

---

_@MichaReiser approved on 2023-10-16 02:05_

---

_@konstin reviewed on 2023-10-16 13:22_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:226 on 2023-10-16 13:22_

Thought so too but didn't want to create so much churn, changed it now

---

_@konstin reviewed on 2023-10-16 13:23_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:211 on 2023-10-16 13:23_

I missed that we already have the path on the outer struct that we can use

---

_Comment by @konstin on 2023-10-16 13:25_

> I guess the challenge with this is that we need to move SourceKind out of the linter, maybe into ruff_source_file? I recommend investigating this as a separate PR.

Did work without it, but i'm fine with moving it anyway

---

_@konstin reviewed on 2023-10-16 14:08_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:426 on 2023-10-16 14:08_

I went with renaming `FormatSummary` to `FormatResults` and giving it two different write methods. We unfortunately lose the fmt impl but we get proper separation

---

_Merged by @konstin on 2023-10-18 11:55_

---

_Closed by @konstin on 2023-10-18 11:55_

---

_Branch deleted on 2023-10-18 11:55_

---
