```yaml
number: 15863
title: "`UP015`'s range and message are slightly incorrect"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - breaking
  - diagnostics
assignees: []
created_at: 2025-02-01T00:03:14Z
updated_at: 2025-02-05T08:44:27Z
url: https://github.com/astral-sh/ruff/issues/15863
synced_at: 2026-01-10T11:09:57Z
```

# `UP015`'s range and message are slightly incorrect

---

_Issue opened by @InSyncWithFoo on 2025-02-01 00:03_

Minimal reproducible example ([playground](https://play.ruff.rs/b66038dc-2266-4210-b181-6487d53a44f2)):

```python
open(
    '.txt',
    'r'
)
```

<pre><code>$ ruff check --isolated --select <a href="https://docs.astral.sh/ruff/rules/redundant-open-modes/">UP015</a>
.py:1:1: UP015 [*] Unnecessary open mode parameters
  |
1 | / open(
2 | |     '.txt',
3 | |     'r'
4 | | )
  | |_^ UP015
  |
  = help: Remove open mode parameters

Found 1 error.
[*] 1 fixable with the `--fix` option.
</code></pre>

There is only one `mode` parameter, and the value passed to that parameter is called an <em>argument</em>. Additionally, the range is much wider than is necessary; it should cover that argument but nothing else.

---

_Comment by @InSyncWithFoo on 2025-02-01 00:04_

Changing the diagnostic range is, very unfortunately, a breaking change and thus must be preview-aware. I intended for this to be a good first issue, but I can submit a fix myself if that proves to be a blocker.

---

_Label `diagnostics` added by @dhruvmanila on 2025-02-01 05:32_

---

_Label `breaking` added by @MichaReiser on 2025-02-01 20:20_

---

_Closed by @MichaReiser on 2025-02-05 08:44_

---
