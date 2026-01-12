```yaml
number: 7853
title: Diff is reversed
type: issue
state: closed
author: bluthej
labels: []
assignees: []
created_at: 2023-10-08T21:07:42Z
updated_at: 2023-10-09T07:28:15Z
url: https://github.com/astral-sh/ruff/issues/7853
synced_at: 2026-01-12T15:54:47Z
```

# Diff is reversed

---

_@bluthej_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
# Issue

The diff output is reversed since #7813.

For example, if I give ruff the following input file:

```python
from .logging import config_logging
from .settings import ENV
from .settings import *
```

then I get the following correct output with `v0.0.292`:

```shell
--- test.py
+++ test.py
@@ -1,3 +1,3 @@
 from .logging import config_logging
+from .settings import *
 from .settings import ENV
-from .settings import *

Would fix 1 error.
```

But with the main branch, I get:

```shell
--- test.py
+++ test.py
@@ -1,3 +1,3 @@
 from .logging import config_logging
+from .settings import ENV
 from .settings import *
-from .settings import ENV

Would fix 1 error.
```

# Fix

I traced it back to [this line](https://github.com/astral-sh/ruff/blob/2d6557a51bfeb43ed1e922383ad066ce6626fec8/crates/ruff_linter/src/source_kind.rs#L94), where the old and new source files are reversed. It should read:

```rust
let text_diff = TextDiff::from_lines(src, dst);
```

If that's ok, I can submit a quick PR to fix it. There are 2 tests whose snapshot needs to be updated as a consequence, but apart from that the only change needed is the one I mentioned above.

---

_Comment by @zanieb on 2023-10-08 21:58_

Thanks! I wonder how that pull request didn't change any snapshots

---

_Comment by @charliermarsh on 2023-10-08 23:21_

Oops, PR welcome, thank you.

---

_Comment by @bluthej on 2023-10-09 05:33_

@zanieb it seems like there are only two integration tests that test the output of the CLI diff option (`diff_shows_safe_fixes_by_default` and `diff_shows_unsafe_fixes_with_opt_in`), and they were added in #7769, which was merged 2 days after #7813 ^^

---

_Closed by @dhruvmanila on 2023-10-09 07:28_

---
