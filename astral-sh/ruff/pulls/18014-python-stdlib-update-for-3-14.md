```yaml
number: 18014
title: "python_stdlib: update for 3.14"
type: pull_request
state: merged
author: Rogdham
labels:
  - python314
assignees: []
merged: true
base: main
head: stdlib3.14
created_at: 2025-05-11T07:58:20Z
updated_at: 2025-05-11T17:08:36Z
url: https://github.com/astral-sh/ruff/pull/18014
synced_at: 2026-01-10T18:57:03Z
```

# python_stdlib: update for 3.14

---

_Pull request opened by @Rogdham on 2025-05-11 07:58_

## Summary

Added version 3.14 to the script generating the `known_stdlib.rs` file.

Rebuilt the known stdlibs with latest version (2025.5.10) of [stdlibs Python lib](https://pypi.org/project/stdlibs/) (which added support for 3.14.0b1).

_Note: Python 3.14 is now in [feature freeze](https://peps.python.org/pep-0745/) so the modules in stdlib should be stable._

_See also: #15506_

## Test Plan

The following command has been run. Using for tests the `compression` module which been introduced with Python 3.14.
```sh
ruff check --no-cache --select I001 --target-version py314 --fix
```

With ruff 0.11.9:
```python
import base64
import datetime

import compression

print(base64, compression, datetime)
```

With this PR:
```python
import base64
import compression
import datetime   

print(base64, compression, datetime)
```


---

_Comment by @github-actions[bot] on 2025-05-11 08:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
```

</p>
</details>




---

_@dylwil3 approved on 2025-05-11 16:25_

Fantastic, thank you!

---

_Merged by @dylwil3 on 2025-05-11 16:25_

---

_Closed by @dylwil3 on 2025-05-11 16:25_

---

_Label `python314` added by @dylwil3 on 2025-05-11 16:26_

---

_Label `internal` added by @dylwil3 on 2025-05-11 16:26_

---

_Label `internal` removed by @dylwil3 on 2025-05-11 16:26_

---

_Branch deleted on 2025-05-11 17:08_

---
