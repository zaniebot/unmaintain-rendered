```yaml
number: 1315
title: ty never completes checking astropy (looks like a runaway memory leak)
type: issue
state: closed
author: neutrinoceros
labels:
  - hang
assignees: []
created_at: 2025-10-07T08:41:49Z
updated_at: 2025-10-16T21:36:07Z
url: https://github.com/astral-sh/ty/issues/1315
synced_at: 2026-01-10T02:06:25Z
```

# ty never completes checking astropy (looks like a runaway memory leak)

---

_Issue opened by @neutrinoceros on 2025-10-07 08:41_

### Summary

I've been trying to check the astropy source tree using `ty check`, however it never seems to complete, and instead hangs on a not-so-reproducible high fraction of the file count, e.g.

```
❯ git clone https://github.com/astropy/astropy
❯ cd astropy
❯ ty check astropy
Checking ------------------------------------------------------------ 926/958 files
```

meanwhile, the process steadily allocates GBs of memory and needs to be killed manually.
note: this code base is fairly large and poorly typed at the moment.
What can I do to help pin point the actual problem ?

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @AlexWaygood on 2025-10-07 09:20_

Thanks for the report. I can reproduce this on our current `main` branch as well, which is a shame (I was hoping it might have already been fixed by https://github.com/astral-sh/ruff/pull/20650 or https://github.com/astral-sh/ruff/pull/20602...).

---

_Label `hang` added by @AlexWaygood on 2025-10-07 09:20_

---

_Comment by @MichaReiser on 2025-10-07 09:22_

ty gets stuck somewhere in https://github.com/astropy/astropy/blob/main/astropy/nddata/tests/test_nddata.py

---

_Comment by @AlexWaygood on 2025-10-07 09:25_

The hang was introduced in ty 0.0.1a14 (it wasn't caused by a recent change)

---

_Comment by @MichaReiser on 2025-10-07 09:25_

This might be related to #1111 I'm seeign a lot of member_lookup_with_policy calls towards the end (not on my normal computer where I can easily debug salsa).

---

_Comment by @AlexWaygood on 2025-10-07 09:32_

```
f4bd74ab6af8981ccd384ad0c791ea6028e21a94 is the first bad commit
commit f4bd74ab6af8981ccd384ad0c791ea6028e21a94
Author: Abhijeet Prasad Bodas <55339528+abhijeetbodas2001@users.noreply.github.com>
Date:   Sat Jul 5 00:22:52 2025 +0530

    [ty] Correctly handle calls to functions marked as returning `Never` / `NoReturn` (#18333)
```

git bisect points to https://github.com/astral-sh/ruff/commit/f4bd74ab6af8981ccd384ad0c791ea6028e21a94 as having introduced the hang

---

_Comment by @MichaReiser on 2025-10-07 09:38_

I can confirm that my "new salsa" version fix this. So this is just another fixpoint runayway. (It completes in 300ms)

---

_Comment by @neutrinoceros on 2025-10-07 10:57_

thanks for engaging this quickly !

> I can confirm that my "new salsa" version fix this.

that's a bit hard to parse from an outsider's POV, but I'm assuming this means the first beta will have the fix ? :)

---

_Comment by @AlexWaygood on 2025-10-07 11:04_

The slightly less concise version of

> I can confirm that my "new salsa" version fix this

is: We have existing reports of runaway execution time on certain snippets of code (https://github.com/astral-sh/ty/issues/1111, etc.). The root cause of these is the mechanism that Salsa (a library that we depend on) uses to resolve cycles that crop up during type inference. This mechanism is called "fixpoint iteration". It turns out that Salsa's implementation of fixpoint iteration can get quite pathologically slow if you have nested cycles. Micha has a work-in-progress PR to fix that on the Salsa side: https://github.com/salsa-rs/salsa/pull/999. It appears to fix the pathological performance issues demonstrated in #1111, and also appears to fix the pathological performance issues here too.

So, the TL;DR is that this is a somewhat hard problem to fix (fixpoint iteration is complicated!), but it's something we're already working on, and we hope to have a fix up soon.

Hope that helps :-)

---

_Comment by @neutrinoceros on 2025-10-16 21:36_

This is solved in ty 0.0.1-alpha.23

Thank you very much !

---

_Closed by @neutrinoceros on 2025-10-16 21:36_

---
