---
number: 22343
title: "Consider marking `UP026`'s fix unsafe"
type: issue
state: open
author: robsdedude
labels:
  - rule
assignees: []
created_at: 2026-01-02T18:31:42Z
updated_at: 2026-01-05T21:19:36Z
url: https://github.com/astral-sh/ruff/issues/22343
synced_at: 2026-01-07T13:12:16-06:00
---

# Consider marking `UP026`'s fix unsafe

---

_Issue opened by @robsdedude on 2026-01-02 18:31_

### Summary

I'll quote the readme of the [`mock` package](https://pypi.org/project/mock/):

> mock is now part of the Python standard library, available as [unittest.mock](https://docs.python.org/dev/library/unittest.mock.html) in Python 3.3 onwards.
> 
> This package contains a rolling backport of the standard library mock code compatible with Python 3.6 and up.

So replacing `import mock` with `from unittest import mock` is not equivalent given a code-base that's on an old-ish Python version but want to use new features of `mock` and therefore decides to use the stand-alone package. Arguably, such code-base should be disabling the lint, nonetheless does the fix cause a little mayhem first should the rule be enabled.

---

_Comment by @ntBre on 2026-01-02 23:53_

Thanks! This makes some sense to me. Out of curiosity, did you run into any specific APIs that didn't work in `unittest.mock`? This is probably not too reliable, but I quickly grepped through the `unittest.mock` docs and most of the version-related changes were for 3.8 or earlier ([`DEFAULT_TIMEOUT`](https://docs.python.org/dev/library/unittest.mock.html#unittest.mock.ThreadingMock.DEFAULT_TIMEOUT) was the one exception I noticed). We could potentially make only fixes touching known-problematic APIs unsafe instead of the whole fix, if it's easy to enumerate them.

---

_Label `fixes` added by @ntBre on 2026-01-02 23:53_

---

_Label `needs-decision` added by @ntBre on 2026-01-02 23:53_

---

_Comment by @MichaReiser on 2026-01-05 09:55_

I'd prefer if we disable the rule for APIs that we know are unavailable on the target version rather than making the fix unsafe. 

---

_Label `fixes` removed by @MichaReiser on 2026-01-05 09:56_

---

_Label `rule` added by @MichaReiser on 2026-01-05 09:56_

---

_Label `needs-decision` removed by @MichaReiser on 2026-01-05 09:56_

---

_Comment by @robsdedude on 2026-01-05 21:19_

Iirc, ruff only computes lints on a per-file basis. If that's the case, it's not possible to know where a certain mock object is going to be used. Usually the mock specific methods are being called in the same test function where the mock is being created, but it's entirely possible that a helper method in another file is being used to set up the mock or to perform common assertions. In such cases the suggested approach might still break.

Further, this approach easily ends up with a test suite using different mock versions. Besides differences in the API the versions might also differ in behaviour (think fixes). I personally think making ruff introduce a mix of dependency versions where there (hopefully) was none before is a trip hazard. 

---
