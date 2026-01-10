```yaml
number: 19970
title: "F821 false positives for `__annotate__` and `__warningregistry__`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-08-18T14:50:36Z
updated_at: 2025-09-23T12:16:02Z
url: https://github.com/astral-sh/ruff/issues/19970
synced_at: 2026-01-10T11:09:59Z
```

# F821 false positives for `__annotate__` and `__warningregistry__`

---

_Issue opened by @dscorbett on 2025-08-18 14:50_

### Summary

There are two documented module-level dunder attributes that [`undefined-name` (F821)](https://docs.astral.sh/ruff/rules/undefined-name/) doesn’t recognize: [`__annotate__`](https://docs.python.org/3.14/reference/datamodel.html#other-writable-attributes-on-module-objects) and [`__warningregistry__`](https://docs.python.org/3.14/library/warnings.html#warnings.warn_explicit). They are like `__file__` and `__cached__` in that not every module has them and they are not commonly used, but if someone uses them it was probably intentional, so F821 shouldn’t flag them. [Example](https://play.ruff.rs/1ba60c54-4666-4cd7-abfb-e02fc526c6ff):
```console
$ cat >f821.py <<'# EOF'
x: str = str(b"!")
print(__warningregistry__)
print(__annotate__(1))
# EOF

$ python3.14 -b f821.py 2>/dev/null
{'version': 1, ('str() on a bytes instance', <class 'BytesWarning'>, 1): True}
{'x': <class 'str'>}

$ ruff --isolated check f821.py --select F821 --target-version py314 --preview --output-format concise -q
f821.py:2:7: F821 Undefined name `__warningregistry__`
f821.py:3:7: F821 Undefined name `__annotate__`
```

### Version

ruff 0.12.9 (ef422460d 2025-08-14)

---

_Label `rule` added by @ntBre on 2025-08-18 17:59_

---

_Label `bug` added by @ntBre on 2025-08-18 17:59_

---

_Closed by @ntBre on 2025-09-23 12:16_

---
