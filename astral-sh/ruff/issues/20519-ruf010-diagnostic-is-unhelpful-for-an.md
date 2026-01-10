```yaml
number: 20519
title: RUF010 diagnostic is unhelpful for an interpolation with debug text
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-09-22T20:06:55Z
updated_at: 2025-10-07T20:58:00Z
url: https://github.com/astral-sh/ruff/issues/20519
synced_at: 2026-01-10T11:09:59Z
```

# RUF010 diagnostic is unhelpful for an interpolation with debug text

---

_Issue opened by @dscorbett on 2025-09-22 20:06_

### Summary

[`explicit-f-string-type-conversion` (RUF010)](https://docs.astral.sh/ruff/rules/explicit-f-string-type-conversion/) should be suppressed for interpolations that use `=` because there is no way to use a conversion flag to get the same output, which the user presumably intended, so the diagnostic is not helpful.
```console
$ cat >ruf010.py <<'# EOF'
print(f"{ascii(1)=}")
# EOF

$ python ruf010.py
ascii(1)='1'

$ ruff --isolated check ruf010.py --select RUF010 --output-format concise -q
ruf010.py:1:10: RUF010 Use explicit conversion flag
```
Alternatively, if the diagnostic _is_ deemed helpful, I suggest reverting that piece of #18690 and marking the fix unsafe in this case. Either works; it’s just this middle ground of hinting at a change but not showing it that seems unhelpful.

### Version

ruff 0.13.1 (706be0a6e 2025-09-18)

---

_Comment by @ntBre on 2025-09-22 21:49_

Thank you! I agree that we should suppress the diagnostic in this case. We'll just need to check for debug text before creating the diagnostic instead of after.

---

_Label `bug` added by @ntBre on 2025-09-22 21:49_

---

_Label `good first issue` added by @ntBre on 2025-09-22 21:49_

---

_Closed by @ntBre on 2025-10-07 20:58_

---
