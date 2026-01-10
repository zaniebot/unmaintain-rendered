```yaml
number: 18413
title: "Add newline after import only if blocks (`TYPE_CHECKING`)"
type: pull_request
state: closed
author: maxmynter
labels:
  - formatter
assignees: []
base: main
head: if-imports
created_at: 2025-06-01T16:01:21Z
updated_at: 2025-06-02T09:13:45Z
url: https://github.com/astral-sh/ruff/pull/18413
synced_at: 2026-01-10T18:45:04Z
```

# Add newline after import only if blocks (`TYPE_CHECKING`)

---

_Pull request opened by @maxmynter on 2025-06-01 16:01_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->
Closes: #18410 

## Summary

Formatter adds a newline after a standalone `if` block that only contains imports.

This adresses the concern from the linked issue that usually the Ruff formatter requires blank lines after imports.

## Note: 
I could not fully reproduce the formatting behaviour of the issue.

Running the example:
```bash
echo "from __future__ import annotations

from pathlib import Path

path: Path" > tmp.py && ruff check --select TC003 --fix --unsafe-fixes --diff tmp.py
```
gives
```bash
--- tmp.py
+++ tmp.py
@@ -1,5 +1,8 @@
 from __future__ import annotations

-from pathlib import Path
+from typing import TYPE_CHECKING
+
+if TYPE_CHECKING:
+    from pathlib import Path

 path: Path

Would fix 1 error.
```
(note the empty line that persists between the import and `path: Path`.) 

However, without the separating line, no empty line is added:
```bash
echo "from __future__ import annotations

from pathlib import Path
path: Path" > tmp.py && ruff check --select TC003 --fix --unsafe-fixes --diff tmp.py
```
gives
```bash
--- tmp.py
+++ tmp.py
@@ -1,4 +1,7 @@
 from __future__ import annotations

-from pathlib import Path
+from typing import TYPE_CHECKING
+
+if TYPE_CHECKING:
+    from pathlib import Path
 path: Path

Would fix 1 error.
```

And for consistency with empty lines after imports, agreeing with the issue, I feel like it should. 
This is addressed in this PR on format.



<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added a test case.

<!-- How was it tested? -->


---

_Review requested from @MichaReiser by @maxmynter on 2025-06-01 16:01_

---

_Comment by @github-actions[bot] on 2025-06-01 16:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+1 -0 lines in 1 file in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 lines across 1 file)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/e97367d555c4acd36d9ad597d76f3cfd93aff3da/rotkehlchen/tests/api/test_sushiswap.py#L23'>rotkehlchen/tests/api/test_sushiswap.py~L23</a>
```diff
 
 if TYPE_CHECKING:
     from rotkehlchen.api.server import APIServer
+
 SWAP_ADDRESS = string_to_evm_address("0x63BC843b9640c4D79d6aE0105bc39F773172d121")
 
 
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+5 -1 lines in 6 files in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/superset/blob/c09f8f6f7665e503a376926700fa815add6892ca/superset/db_engine_specs/bigquery.py#L74'>superset/db_engine_specs/bigquery.py~L74</a>
```diff
 if TYPE_CHECKING:
     from superset.models.core import Database  # pragma: no cover
 
-
 logger = logging.getLogger()
 
 CONNECTION_DATABASE_PERMISSIONS_REGEX = re.compile(
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/c708e152c42f81b17cf6e47f7939a86e4c3fc77f/pandas/core/indexes/interval.py#L100'>pandas/core/indexes/interval.py~L100</a>
```diff
         Self,
         npt,
     )
+
 _index_doc_kwargs = dict(ibase._index_doc_kwargs)
 
 _index_doc_kwargs.update({
```
<a href='https://github.com/pandas-dev/pandas/blob/c708e152c42f81b17cf6e47f7939a86e4c3fc77f/pandas/io/formats/style_render.py#L47'>pandas/io/formats/style_render.py~L47</a>
```diff
         Axis,
         Level,
     )
+
 jinja2 = import_optional_dependency("jinja2", extra="DataFrame.style requires jinja2.")
 from markupsafe import escape as escape_html  # markupsafe is jinja2 dependency
 
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/typeshed/blob/09b6802dc7a568309db46cae14c52e44787f8494/stdlib/ntpath.pyi#L47'>stdlib/ntpath.pyi~L47</a>
```diff
 
 if sys.version_info >= (3, 12):
     from posixpath import isjunction as isjunction, splitroot as splitroot
+
 if sys.version_info >= (3, 13):
     from genericpath import isdevdrive as isdevdrive
 
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/e97367d555c4acd36d9ad597d76f3cfd93aff3da/rotkehlchen/tests/api/test_sushiswap.py#L23'>rotkehlchen/tests/api/test_sushiswap.py~L23</a>
```diff
 
 if TYPE_CHECKING:
     from rotkehlchen.api.server import APIServer
+
 SWAP_ADDRESS = string_to_evm_address("0x63BC843b9640c4D79d6aE0105bc39F773172d121")
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/astropy/astropy/blob/c8ef6b1bd29ec2436ef6595e24c7988c467c0ac7/astropy/time/core.py#L58'>astropy/time/core.py~L58</a>
```diff
 
 if TYPE_CHECKING:
     from astropy.coordinates import EarthLocation
+
 __all__ = [
     "STANDARD_TIME_SCALES",
     "TIME_DELTA_SCALES",
```

</p>
</details>




---

_Comment by @maxmynter on 2025-06-01 16:29_

The apache superset ecosystem change is the deletion of a line (before there were 2 empty ones). The others are an added line. 

---

_Label `formatter` added by @AlexWaygood on 2025-06-01 16:30_

---

_Comment by @MichaReiser on 2025-06-01 16:40_

Is this change compatible with black? It also sets a new precedence to change formatting based on semantics

---

_Comment by @maxmynter on 2025-06-01 17:30_

I'm unsure on the exact definition of compatibility. 

It's compatible in the sense that Black doesn't complain on the changed file. However, it doesn't do the change either. 

I.e. 

```bash
black tmp.py --diff
All done! ✨ 🍰 ✨
1 file would be left unchanged.
```

for both 

```bash
cat tmp.py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from pathlib import Path

path: Path
```
and

```bash
cat tmp.py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from pathlib import Path
path: Path
```

---

_Comment by @MichaReiser on 2025-06-01 17:42_

Yeah. We definitely can't change this outside preview. But I'm not sure if we should change this because ruff/black are intentionally unopinionated about spacing around if statements. 

---

_Comment by @maxmynter on 2025-06-01 19:56_

Does that also mean we should not address the issue (#18410)?

In principle one could add this empty line formatting to the fix for TC003. But I fear this is going to be janky and breaks Ruff's usual lifecycle of fix -> format.

I personally like the empty line after the type check if clause but don't have arguments to supercede being unoponionated on spacing.
And in that case, the existing fix seems to preserve whatever spacing was there previously.

Let me know if you would like a different implementation. Otherwise feel free to close this PR. 


---

_Comment by @MichaReiser on 2025-06-02 09:13_

I think this requires some more design before we can even consider adding this to the formatter. E.g. what if the if statement has an else branch? What about other conditional imports (e.g. python version specific imports)? What about conditional imports using try-catch. 

Given the scope, I don't feel comfortable merging this as is.

---

_Closed by @MichaReiser on 2025-06-02 09:13_

---
