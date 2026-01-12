```yaml
number: 10113
title: "[E231] Inconsistent catch compared to pycodestyle, such as when dict nested in list"
type: issue
state: closed
author: dfrtz
labels:
  - bug
assignees: []
created_at: 2024-02-24T19:11:01Z
updated_at: 2024-03-21T08:13:39Z
url: https://github.com/astral-sh/ruff/issues/10113
synced_at: 2026-01-12T15:54:49Z
```

# [E231] Inconsistent catch compared to pycodestyle, such as when dict nested in list

---

_@dfrtz_

Searching through closed and opened issues, I did not see anywhere that this is intentional or known, apologies if I missed something. It appears that in certain scenarios E231 is not triggering as expected, whereas in pycodestyle is it being caught. It may impact other scenarios, but the primary one I could find is it not working on dicts inside a list. Strangely enough it does work in tuples, and outside lists, just not in lists. Even stranger, only if not the first key/value; it does work if it is the first key/value in the dict in the list.

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


def main() -> None:
    """Primary function."""
    results = {
        "k1": [1],
        "k2":[2],
    }
    results_in_tuple = (
        {
            "k1": [1],
            "k2":[2],
        },
    )
    results_in_list = [
        {
            "k1": [1],
            "k2":[2],
        }
    ]
    results_in_list_first = [
        {
            "k2":[2],
        }
    ]


main()
```

**pycodestyle 2.11.1** catching all 4
```
$ pycodestyle --count min_repo.py
min_repo.py:8:13: E231 missing whitespace after ':'
min_repo.py:13:17: E231 missing whitespace after ':'
min_repo.py:19:17: E231 missing whitespace after ':'
min_repo.py:24:17: E231 missing whitespace after ':'
4
```

**ruff 0.2.2** catching 3/4
```
$ ruff --version
ruff 0.2.2
$ ruff check --preview min_repo.py
min_repo.py:8:13: E231 [*] Missing whitespace after ':'
   |
 6 |     results = {
 7 |         "k1": [1],
 8 |         "k2":[2],
   |             ^ E231
 9 |     }
10 |     results_in_tuple = (
   |
   = help: Add missing whitespace

min_repo.py:13:17: E231 [*] Missing whitespace after ':'
   |
11 |         {
12 |             "k1": [1],
13 |             "k2":[2],
   |                 ^ E231
14 |         },
15 |     )
   |
   = help: Add missing whitespace

min_repo.py:24:17: E231 [*] Missing whitespace after ':'
   |
22 |     results_in_list_first = [
23 |         {
24 |             "k2":[2],
   |                 ^ E231
25 |         }
26 |     ]
   |
   = help: Add missing whitespace

Found 3 errors.
[*] 3 fixable with the `--fix` option.
```

To note, if using ruff format, it would fix this, so not a blocker if you are also using format along with check. If you are only using ruff to replace pycodestyle without format (for whatever reason, such as your project not being ready/compatible with auto format), then you would hit this miss if only using ruff and not pycodestyle.
```
$ ruff format --check min_repo.py --diff
--- min_repo.py
+++ min_repo.py
@@ -5,23 +5,23 @@
     """Primary function."""
     results = {
         "k1": [1],
-        "k2":[2],
+        "k2": [2],
     }
     results_in_tuple = (
         {
             "k1": [1],
-            "k2":[2],
+            "k2": [2],
         },
     )
     results_in_list = [
         {
             "k1": [1],
-            "k2":[2],
+            "k2": [2],
         }
     ]
     results_in_list_first = [
         {
-            "k2":[2],
+            "k2": [2],
         }
     ]


1 file would be reformatted
```

I know E231 is still preview, and this is a pretty minor miss, but I assume the goal is for parity with pycodestyle (and consistency across nested objects).


---

_Comment by @charliermarsh on 2024-02-24 19:12_

Nice, thanks!

---

_Label `bug` added by @charliermarsh on 2024-02-24 19:12_

---

_Comment by @charliermarsh on 2024-02-24 19:12_

Great write-up.

---

_Closed by @MichaReiser on 2024-03-21 08:13_

---
