```yaml
number: 15915
title: open-alias (UP020) has unexpected auto-fix behaviour
type: issue
state: open
author: RasmusNygren
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-02-03T18:29:44Z
updated_at: 2025-02-11T20:01:56Z
url: https://github.com/astral-sh/ruff/issues/15915
synced_at: 2026-01-10T11:09:57Z
```

# open-alias (UP020) has unexpected auto-fix behaviour

---

_Issue opened by @RasmusNygren on 2025-02-03 18:29_

### Description

Running the UP020 rule with --fix produces weird and unexpected results.


**Example 1 (ImportStyle::ImportFrom)**
When running `ruff check --fix --select --isolated UP020 /path/to/file.py` on the particular file below:
```python
from io import open
open("Cargo.toml")
```
you end up with this result:
```python
from io import open
import builtins
builtins.open("Cargo.toml"): ...
```
Not only is the `from io import open` statement still there, but it also imports builtins which is not needed in this minimal example.

**Example 2 (ImportStyle::Import)**
When running `ruff check --fix --select --isolated UP020 /path/to/file.py` on the particular file below:
```python
import io
io.open("Cargo.toml")
```
you end up with the following:
```python
import io
open("Cargo.toml")
```
where the `import io` statement is still there despite being unused.

`ruff --version`: `ruff 0.9.4`


---

_Comment by @ntBre on 2025-02-03 18:46_

I think both of these examples boil down to the rule not removing the `io` import. Also enabling [F401 - unused-import](https://docs.astral.sh/ruff/rules/unused-import/) should take care of the second case:

```shell
$ cat <<EOF | ruff check --select UP020,F401 --diff -
import io
io.open("Cargo.toml")
EOF
```
Output
```diff
@@ -1,2 +1 @@
-import io
-io.open("Cargo.toml")
+open("Cargo.toml")

Would fix 2 errors.
```

But I agree that the first case is not really ideal. Even with F401, `builtins` still ends up imported:

```shell
$ cat <<EOF | ruff check --select UP020,F401 --diff --no-cache -
from io import open
open("Cargo.toml")   
EOF
```

Output
```diff
@@ -1,2 +1,2 @@
-from io import open
-open("Cargo.toml")
+import builtins
+builtins.open("Cargo.toml")

Would fix 2 errors.
```

---

_Label `bug` added by @MichaReiser on 2025-02-03 19:20_

---

_Label `fixes` added by @MichaReiser on 2025-02-03 19:20_

---

_Comment by @ntBre on 2025-02-03 19:21_

There is another rule that looks promising for cleaning up the `builtins` import: [UP029 - unnecessary-builtin-import](https://docs.astral.sh/ruff/rules/unnecessary-builtin-import/), but it doesn't actually help here, unfortunately, as it looks for imports like `from builtins import open`. That might be the best rule to update  to clean this up? What do you think @MichaReiser or @AlexWaygood ?

---

_Comment by @MichaReiser on 2025-02-03 19:34_

Adjusting UP029 seems reasonable to me. I'm a bit surprised that it wouldn't catch `from io import open` because there's an `io`, `open` in the match statement here

https://github.com/astral-sh/ruff/blob/14ba469fc099e0678859bba3ae6f67477d8934ec/crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_builtin_import.rs#L102

---

_Comment by @ntBre on 2025-02-03 19:39_

Ahhh, with `--unsafe-fixes` UP029 *does* cause the desired behavior:

```shell
cat <<EOF | ruff check --select UP020,UP029,F401 --diff --no-cache --unsafe-fixes -
from io import open
open("Cargo.toml")
EOF
```

```diff
@@ -1,2 +1 @@
-from io import open
 open("Cargo.toml")

Would fix 1 error.
```

But without `--unsafe-fixes` and with UP020 also selected, UP020 runs and doesn't mention the unsafe fix. I only got the `(1 fix available with --unsafe-fixes)` when I didn't select UP020 too.



---

_Comment by @MichaReiser on 2025-02-03 19:42_

Ah nice find. I wonder why the fix is marked as unsafe.

---

_Comment by @ntBre on 2025-02-03 19:58_

Not sure either, but it's being tracked for improving that in the docs in #15584 at least.

---

_Comment by @RasmusNygren on 2025-02-11 19:56_

Ah, It's interesting why auto-fixing (removing) `from builtins import open` is deemed unsafe to begin with. The documentation for [UP029](https://docs.astral.sh/ruff/rules/unnecessary-builtin-import/) mentions nothing about unsafe fixes so there's a mismatch there somewhere. If the case was `import builtins; builtins.open("Cargo.toml")` (and the equivalent `import io; io.open("Cargo.toml")`) it makes sense for that variant of the problem to be unsafe to auto-fix as potential shadowing means that the same behaviour can not be guaranteed, but I can't see how this applies to the variant with `from ... import ...`.

However, with that logic, what UP029 does in Example 2 is probably unsafe (despite being classified as safe). As otherwise removing the builtins prefix in `import builtins; builtins.open` must be safe as well, but is not classified as such.

To connect this back to the original issue - I can't see why the fix for Example 1 would involve the explicit builtins import at all. It can probably be auto-fixed without it as involving the builtins import (using the `import builtins; builtins.open` syntax) essentially converts what could be a safe auto-fix into a different format of the problem where auto-fixing is most certainly unsafe. And would now require UP029 as well to get the expected behaviour.

Unless I'm missing something essential here, my opinion is that the Example 1 version should probably not rely on UP029 (or F401 for that matter) at all for the fix, and Example 2 should at least turn
```python
import io
io.open("Cargo.toml")
```
into
```python
import builtins
builtins.open("Cargo.toml")
```
if it can't be safely auto-fixed by dropping the io import

---
