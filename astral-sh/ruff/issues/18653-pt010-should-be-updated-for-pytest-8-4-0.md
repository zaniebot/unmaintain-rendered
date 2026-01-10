```yaml
number: 18653
title: PT010 should be updated for pytest 8.4.0
type: issue
state: closed
author: dscorbett
labels:
  - documentation
  - rule
  - help wanted
assignees: []
created_at: 2025-06-12T20:53:35Z
updated_at: 2025-12-18T15:42:07Z
url: https://github.com/astral-sh/ruff/issues/18653
synced_at: 2026-01-10T11:09:58Z
```

# PT010 should be updated for pytest 8.4.0

---

_Issue opened by @dscorbett on 2025-06-12 20:53_

### Summary

The documentation for [`pytest-raises-without-exception` (PT010)](https://docs.astral.sh/ruff/rules/pytest-raises-without-exception/) says:
> `pytest.raises` expects to receive an expected exception as its first argument. If omitted, the `pytest.raises` call will fail at runtime.

As of pytest 8.4.0 (released June 2, 2025) it is also sufficient to pass the keyword argument `match` or `check`. The rule should recognize those arguments too.

```console
$ cat >pt010.py <<'# EOF'
import pytest
with pytest.raises(match=r"!+"):
    raise RuntimeError("!!!")
with pytest.raises(check=lambda _e: True):
    raise RuntimeError("...")
# EOF

$ python pt010.py; echo $?
0

$ ruff --isolated check pt010.py --select PT010 --output-format concise -q
pt010.py:2:6: PT010 Set the expected exception in `pytest.raises()`
pt010.py:4:6: PT010 Set the expected exception in `pytest.raises()`
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `documentation` added by @ntBre on 2025-06-12 22:34_

---

_Label `rule` added by @ntBre on 2025-06-12 22:34_

---

_Label `help wanted` added by @ntBre on 2025-06-12 22:34_

---

_Comment by @ntBre on 2025-06-12 22:43_

Nice, thanks! That's quite a recent version, so we should probably address both older and newer versions in the docs.

I'm not quite sure what to do about the rule itself since I don't think we can check the actual version of a package. I guess I would lean toward just always allowing calls with `match` or `check` arguments too. It looks like `check` is also new in 8.4.0, so I guess `match` is the only ambiguous one.

---

_Comment by @mahiro72 on 2025-12-12 11:53_

Hi @ntBre , I'd like to work on this issue.

---

_Assigned to @mahiro72 by @ntBre on 2025-12-12 14:26_

---

_Closed by @ntBre on 2025-12-18 15:42_

---
