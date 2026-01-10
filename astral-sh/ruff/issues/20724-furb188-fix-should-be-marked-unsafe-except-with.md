```yaml
number: 20724
title: "FURB188 fix should be marked unsafe except with known `str`s"
type: issue
state: open
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-10-06T16:07:08Z
updated_at: 2025-10-06T18:06:33Z
url: https://github.com/astral-sh/ruff/issues/20724
synced_at: 2026-01-10T11:09:59Z
```

# FURB188 fix should be marked unsafe except with known `str`s

---

_Issue opened by @dscorbett on 2025-10-06 16:07_

### Summary

The fix for [`slice-to-remove-prefix-or-suffix` (FURB188)](https://docs.astral.sh/ruff/rules/slice-to-remove-prefix-or-suffix/) should be marked unsafe unless it is statically known that the full string and its affix are both `str`s. If it is statically known that they are instances of unrelated classes, the rule should be suppressed. Otherwise, the fix can change the program’s behavior. They have to be exactly `str` for the fix to be safe, not subclasses of `str`. [Example](https://play.ruff.rs/e827d415-18c2-424a-86e4-a49c3c731b10):
```console
$ cat >furb188.py <<'# EOF'
class Seq:
    def __init__(self, inner):
        self.inner = inner
    def startswith(self, prefix):
        return tuple(self.inner[:len(prefix)]) == tuple(prefix)
    def __getitem__(self, item):
        return type(self)(self.inner[item])
seq = Seq(("1", "2", "3", "4", "5"))
try:
    if seq.startswith("123"):
        seq = seq[3:]
    print(seq.inner)
except AttributeError as e:
    print(e)

text = "12345"
prefix = ("123",)
try:
    if text.startswith(prefix):
        text = text[len(prefix):]
    print(text)
except TypeError as e:
    print(e)
# EOF

$ python furb188.py
('4', '5')
2345

$ ruff --isolated check furb188.py --select FURB188 --fix
Found 2 errors (2 fixed, 0 remaining).

$ python furb188.py
'Seq' object has no attribute 'removeprefix'
removeprefix() argument must be str, not tuple
```

### Version

ruff 0.13.3 (188c0dce2 2025-10-02)

---

_Label `fixes` added by @ntBre on 2025-10-06 18:06_

---
