```yaml
number: 20874
title: "False positve PD011: using `.values` on a NumPy NamedTuple, not a Pandas Series or Index object"
type: issue
state: closed
author: hunterhogan
labels:
  - bug
assignees: []
created_at: 2025-10-14T21:59:34Z
updated_at: 2025-10-17T03:09:21Z
url: https://github.com/astral-sh/ruff/issues/20874
synced_at: 2026-01-12T15:54:57Z
```

# False positve PD011: using `.values` on a NumPy NamedTuple, not a Pandas Series or Index object

---

_@hunterhogan_

### Summary

I suspect this situation is infrequent.

```python
...
import pandas

if TYPE_CHECKING:
    from numpy.lib._arraysetops_impl import UniqueInverseResult
...
        unique: UniqueInverseResult[numpy.uint64] = numpy.unique_inverse(arrayAnalyzed[slicerArcCode])

        state.arrayArcCodes = unique.values
```

[Snippet from this module](https://github.com/hunterhogan/mapFolding/blob/2ac46a4c15ee06b5a58ea674891ee747766e843d/mapFolding/algorithms/matrixMeandersNumPyndas.py), if you want it.

[`numpy.unique_inverse`](https://github.com/numpy/numpy/blob/0532af47d6a815298b7841de00bdbc547104b237/numpy/lib/_arraysetops_impl.py#L577-L584) returns a NamedTuple with a 'values' field.

`numpy.unique_all` and `numpy.unique_counts` [do the same](https://github.com/numpy/numpy/blob/0532af47d6a815298b7841de00bdbc547104b237/numpy/lib/_arraysetops_impl.py#L416-L430).

I'm not a NumPy expert, but I'm not aware of other functions that do the same thing, and I did a quick search of NumPy for 'from typing import NamedTuple' and that was the only module.

<details><summary>Diagnostic</summary>
<p>

```json
[{
	"resource": "/c:/apps/mapFolding/mapFolding/algorithms/matrixMeandersNumPyndas.py",
	"owner": "Ruff",
	"code": {
		"value": "PD011",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/pandas-use-of-dot-values",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 4,
	"message": "Use `.to_numpy()` instead of `.values`",
	"source": "Ruff",
	"startLineNumber": 44,
	"startColumn": 26,
	"endLineNumber": 44,
	"endColumn": 39,
	"origin": "extHost2"
}]
```

</p>
</details> 

<details><summary>Environment</summary>
<p>

>Version: 1.106.0-insider (user setup)
Commit: 359fa23b39bb37e5157e99c2bc5fc81088dd8431
Date: 2025-10-14T19:15:26.327Z
Electron: 37.6.0
ElectronBuildId: 12506819
Chromium: 138.0.7204.251
Node.js: 22.19.0
V8: 13.8.258.32-electron.0
OS: Windows_NT x64 10.0.26100

> charliermarsh.ruff
Version
2025.28.0
Last Updated
6 days ago

</p>
</details> 

### Version

2025.28.0

---

_Label `bug` added by @amyreese on 2025-10-14 23:52_

---

_Comment by @MichaReiser on 2025-10-16 08:07_

Thanks. This is related to https://github.com/astral-sh/ruff/issues/6432. We mitigated the cases where it's happening but completely avoiding false positives either requires multifile type inference or limiting the rule to cases where Ruff knows for certain that it's a pandas frame, very likely making the rule useless in most cases.

---

_Comment by @hunterhogan on 2025-10-17 03:09_

Ah, I didn't come across that issue when I searched. This seems like a difficult situation to address: good luck!

---

_Closed by @hunterhogan on 2025-10-17 03:09_

---
