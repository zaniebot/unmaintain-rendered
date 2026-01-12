```yaml
number: 10114
title: "[E301] Failing to catch blank lines above classmethods when compared to pycodestyle"
type: issue
state: closed
author: dfrtz
labels:
  - bug
  - help wanted
  - preview
assignees: []
created_at: 2024-02-24T20:19:48Z
updated_at: 2024-03-01T09:30:55Z
url: https://github.com/astral-sh/ruff/issues/10114
synced_at: 2026-01-12T15:54:49Z
```

# [E301] Failing to catch blank lines above classmethods when compared to pycodestyle

---

_@dfrtz_

The closest I could find to this is #10039, but it appears to be a bit different, as it is talking about conflicts with format on pyi files, but this is impacting regular files and specifically classmethods. I also saw E301 mentioned in #10032 as potentially superfluous with formatter, but I don't see it listed in documentation as having conflicts. Blank line spacing around the E rules is also discussed in some PRs like https://github.com/astral-sh/ruff/pull/8720, but they do not appear to cover this specific scenario. I believe it should still be able to be caught when format is not used for parity with pycodestyle, which does catch these.

**pyproject.toml**
```
[tool.ruff.lint]
select = [
  "E",
  "W",
]
```

**Python file**
```
"""Minimal repo."""


class MinRepoClassA:
    """Class for minimal repo."""

    columns = []
    @classmethod
    def cls_method_a(cls) -> None:
        pass
    @classmethod
    def cls_method_b(cls) -> None:
        pass
    def method_a(self) -> None:
        pass
    def method_b(self) -> None:
        pass

```

**pycodestyle 2.11.1** catching all 4
```
$ pycodestyle --count min_repo.py
min_repo.py:8:5: E301 expected 1 blank line, found 0
min_repo.py:11:5: E301 expected 1 blank line, found 0
min_repo.py:14:5: E301 expected 1 blank line, found 0
min_repo.py:16:5: E301 expected 1 blank line, found 0
4
```

**ruff 0.2.2** catching 2/4
```
$ ruff --version
ruff 0.2.2
$ ruff check --preview min_repo.py
min_repo.py:14:5: E301 [*] Expected 1 blank line, found 0
   |
12 |     def cls_method_b(cls) -> None:
13 |         pass
14 |     def method_a(self) -> None:
   |     ^^^ E301
15 |         pass
16 |     def method_b(self) -> None:
   |
   = help: Add missing blank line

min_repo.py:16:5: E301 [*] Expected 1 blank line, found 0
   |
14 |     def method_a(self) -> None:
15 |         pass
16 |     def method_b(self) -> None:
   |     ^^^ E301
17 |         pass
   |
   = help: Add missing blank line

Found 2 errors.
[*] 2 fixable with the `--fix` option.
```

To note, if using ruff format, it would fix all 4. If you are only using ruff as a replacement for pycodestyle without format however, then you would hit this miss without pycodestyle.
```
$ ruff format --check min_repo.py --diff
--- min_repo.py
+++ min_repo.py
@@ -5,13 +5,17 @@
     """Class for minimal repo."""
 
     columns = []
+
     @classmethod
     def cls_method_a(cls) -> None:
         pass
+
     @classmethod
     def cls_method_b(cls) -> None:
         pass
+
     def method_a(self) -> None:
         pass
+
     def method_b(self) -> None:
         pass

1 file would be reformatted
```

I see E301 is in preview, but this does not appear to be intentional based on pycodestyle behavior, and it working with regular methods. I also did not see any configuration options to change this behavior, but please let me know if I am overlooking something. Thanks!


---

_Comment by @MichaReiser on 2024-02-25 09:21_

Thanks. Yeah I agree this is a bug. @hoel-bagard, would you be interested in looking into this issue? Just be aware, that there's one open PR that restructures the code. So maybe it would be best to start of that branch. https://github.com/astral-sh/ruff/pull/10098

---

_Label `bug` added by @MichaReiser on 2024-02-25 09:21_

---

_Label `help wanted` added by @MichaReiser on 2024-02-25 09:21_

---

_Label `preview` added by @MichaReiser on 2024-02-25 09:21_

---

_Comment by @hoel-bagard on 2024-02-25 09:34_

@MichaReiser I'll look into fixing this, thanks for the notice!

---

_Closed by @MichaReiser on 2024-03-01 09:30_

---
