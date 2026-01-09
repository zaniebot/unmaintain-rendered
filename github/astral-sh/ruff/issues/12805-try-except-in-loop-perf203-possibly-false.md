---
number: 12805
title: "`try-except-in-loop (PERF203)` possibly false-positives with `continue`/`pass` in `except` block"
type: issue
state: closed
author: Avasam
labels:
  - rule
  - preview
assignees: []
created_at: 2024-08-11T21:03:05Z
updated_at: 2024-08-17T15:00:16Z
url: https://github.com/astral-sh/ruff/issues/12805
synced_at: 2026-01-07T13:12:15-06:00
---

# `try-except-in-loop (PERF203)` possibly false-positives with `continue`/`pass` in `except` block

---

_Issue opened by @Avasam on 2024-08-11 21:03_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
try-except, continue, pass, PERF203, try-except-in-loop

* A minimal code snippet that reproduces the bug.
Here's a few examples taken directly from setuptools. I don't see a better way of writing them, nor does the doc at make any mention of such cases. Where the iteration must complete and ignore certain errors https://docs.astral.sh/ruff/rules/try-except-in-loop/
```py
def test_process_template_line_invalid(self):
    # invalid lines
    file_list = FileList()
    for action in (
        'include',
        'exclude',
        'global-include',
        'global-exclude',
        'recursive-include',
        'recursive-exclude',
        'graft',
        'prune',
        'blarg',
    ):
        try:
            file_list.process_template_line(action)
        except DistutilsTemplateError:  # raises PERF203
            pass
        except Exception:
            assert False, "Incorrect error thrown"
        else:
            assert False, "Should have thrown an error"

###

def filesys_decode(path):
    """
    Ensure that the given path is decoded,
    ``None`` when no expected encoding works
    """

    if isinstance(path, str):
        return path

    fs_enc = sys.getfilesystemencoding() or 'utf-8'
    candidates = fs_enc, 'utf-8'

    for enc in candidates:
        try:
            return path.decode(enc)
        except UnicodeDecodeError:  # raises PERF203
            continue

    return None

###

def process_index(self, url, page):
    """Process the contents of a PyPI page"""

    # process an index page into the package-page index
    for match in HREF.finditer(page):
        try:
            self._scan(urllib.parse.urljoin(url, htmldecode(match.group(1))))
        except ValueError:  # raises PERF203
            pass
    ...
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
ruff check . --isolated --preview --select=PERF203

* The current Ruff settings (any relevant sections from your `pyproject.toml`).
N/A

* The current Ruff version (`ruff --version`).
ruff 0.5.7



---

_Referenced in [pypa/setuptools#4556](../../pypa/setuptools/pulls/4556.md) on 2024-08-11 21:24_

---

_Comment by @MichaReiser on 2024-08-14 08:41_

[Playground](https://play.ruff.rs/ef9da7e2-20b4-4f6d-b1c9-33ceac3db00d)

The first example with `pass` other exceptions that get captured seems difficult to detect because it also requires evaluating that the `except` branches never return. 

It does seem reasonable to not flag the rule for `try` statements where all `except` clauses unconditionally use `continue`.

---

_Label `rule` added by @MichaReiser on 2024-08-14 08:41_

---

_Label `preview` added by @MichaReiser on 2024-08-14 08:42_

---

_Label `help wanted` added by @MichaReiser on 2024-08-14 08:42_

---

_Comment by @AlexWaygood on 2024-08-14 10:45_

I'm not sure I understand why this is a false positive. The premise of this rule is that raising and catching an exception is often surprisingly expensive in Python. If you have a `try`/`except` clause in a tight loop, you can often speedup your code by refactoring your code to use "Look Before You Leap" (LBYL) idioms inside the loop instead of `try`/`except` clauses. For example, a change like this _can_ speedup your code a lot (depending on the precise circumstances), which is often surprising to Python developers because it's often seen as less idiomatic:

```diff
for x in whatever:
-    try:
-        val = mapping[x]
-    except KeyError:
-        continue
+    if x in mapping:
+        val = mapping[x]
+    else:
+        continue
```

While I think the rule is useful in some situations, I personally would never enable this rule in CI, since this kind of micro-optimisation will only make a difference in very hot loops, and refactoring code to use LBYL idioms instead of exception-catching idioms is often easier said than done. I would almost definitely not enable it on my tests, since micro-optimising performance in tests is a much lower priority than having tests that are easy to read and provide good coverage for my code.

But I think the rule is working as intended.

---

_Label `help wanted` removed by @AlexWaygood on 2024-08-14 10:45_

---

_Comment by @Avasam on 2024-08-16 15:51_

@AlexWaygood It's a false-positive when there's no better way than using try-catch.

The example you gave (a classic check for KeyError, AttributeError, etc.) is a great example I didn't immediatly think of. And a reason why my false-positives probably can't be avoided with heuristics.
I think such an example should probably be added to the doc as another way to avoid try-except in loops where possible. (Doc only references the LBYL idiom)

Sidenote: my first example isn't great as it's better served by `pytest.raises` anyway. Per-file-ignore could also be used to ignore `PERF203` in tests if need be.

---

The reasons you mentioned that you wouldn't turn this rule on in CI is also why I use warning/info levels in other linters (pyright, ESLint): bring the dev's attention to a code smell/potential issue, but don't fail tests for it.
And one of the reasons I'm in support of configuring a "warning" level for rules in Ruff https://github.com/astral-sh/ruff/issues/1256



---

_Comment by @AlexWaygood on 2024-08-16 16:14_

> @AlexWaygood It's a false-positive when there's no better way than using try-catch.

Right, sometimes it's impractical or silly to refactor the code to use LBYL idioms. But the rule is still working as designed, in that it's accurately identifying a place where an exception might be repeatedly raised and caught inside a loop, which _could_ lead to a performance bottleneck. That's the purpose of the rule, so from that perspective it's not really a false positive.

> think such an example should probably be added to the doc as another way to avoid try-except in loops where possible. (Doc only references the LBYL idiom)

Oh, good point -- the docs do indeed seem to be quite lacking here. Fancy making a PR? :)

---

_Referenced in [astral-sh/ruff#12947](../../astral-sh/ruff/pulls/12947.md) on 2024-08-17 12:24_

---

_Closed by @AlexWaygood on 2024-08-17 15:00_

---

_Referenced in [python/typeshed#13312](../../python/typeshed/pulls/13312.md) on 2024-12-26 23:26_

---
