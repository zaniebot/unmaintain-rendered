```yaml
number: 20768
title: Remove parentheses around multiple exception types on Python 3.14+
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - preview
  - great writeup
assignees: []
merged: true
base: main
head: brent/fmt-remove-exception-parens
created_at: 2025-10-08T15:22:47Z
updated_at: 2025-10-14T15:17:47Z
url: https://github.com/astral-sh/ruff/pull/20768
synced_at: 2026-01-10T17:34:34Z
```

# Remove parentheses around multiple exception types on Python 3.14+

---

_Pull request opened by @ntBre on 2025-10-08 15:22_

Summary
--

This PR implements the black preview style from https://github.com/psf/black/pull/4720. As of Python 3.14, you're allowed to omit the parentheses around groups of exceptions, as long as there's no `as` binding:

**3.13**

```pycon
Python 3.13.4 (main, Jun  4 2025, 17:37:06) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> try: ...
... except (Exception, BaseException): ...
...
Ellipsis
>>> try: ...
... except Exception, BaseException: ...
...
  File "<python-input-1>", line 2
    except Exception, BaseException: ...
           ^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: multiple exception types must be parenthesized
```

**3.14**

```pycon
Python 3.14.0rc2 (main, Sep  2 2025, 14:20:56) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> try: ...
... except Exception, BaseException: ...
...
Ellipsis
>>> try: ...
... except (Exception, BaseException): ...
...
Ellipsis
>>> try: ...
... except Exception, BaseException as e: ...
...
  File "<python-input-2>", line 2
    except Exception, BaseException as e: ...
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: multiple exception types must be parenthesized when using 'as'
```

I think this ended up being pretty straightforward, at least once Micha showed me where to start :)

Test Plan
--

New tests

At first I thought we were deviating from black in how we handle comments within the exception type tuple, but I think this applies to how we format all tuples, not specifically with the new preview style.


---

_Label `formatter` added by @ntBre on 2025-10-08 15:22_

---

_Label `preview` added by @ntBre on 2025-10-08 15:22_

---

_Comment by @github-actions[bot] on 2025-10-08 15:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-10-08 15:39_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-08 15:39_

---

_@MichaReiser approved on 2025-10-08 15:55_

Looks great. 

I just realized that we haven't updated our black tests in a while. It's something I normally do before working on preview styles because we then "inherit" the black tests

Would you mind updating the black tests in a separate PR that we can merge first to get more test coverage?

Updating is normally very smooth but you might run into problems if Black changed their test structure. All that's needed is to run 

https://github.com/astral-sh/ruff/blob/a9efdea113a4f676ef85d89ed2dc285f7087478d/crates/ruff_python_formatter/resources/test/fixtures/import_black_tests.py

I believe it takes a path to black's checkout as an argument

---

_Label `great writeup` added by @MichaReiser on 2025-10-08 16:33_

---

_@MichaReiser reviewed on 2025-10-14 14:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__remove_except_types_parens.py.snap`:1 on 2025-10-14 14:50_

Nice. The most satisfying feeling ever when working on the formatter :)

---

_Merged by @ntBre on 2025-10-14 15:17_

---

_Closed by @ntBre on 2025-10-14 15:17_

---

_Branch deleted on 2025-10-14 15:17_

---
