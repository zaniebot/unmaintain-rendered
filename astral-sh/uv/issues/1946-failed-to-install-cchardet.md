---
number: 1946
title: Failed to install cchardet
type: issue
state: closed
author: chrisgoddard
labels:
  - needs-mre
assignees: []
created_at: 2024-02-24T00:25:14Z
updated_at: 2024-02-26T23:11:40Z
url: https://github.com/astral-sh/uv/issues/1946
synced_at: 2026-01-10T01:23:09Z
---

# Failed to install cchardet

---

_Issue opened by @chrisgoddard on 2024-02-24 00:25_

Error install cchardet==2.1.7

```--- stderr:
src/cchardet/_cchardet.cpp:196:12: fatal error: 'longintrepr.h' file not found
  #include "longintrepr.h"
           ^~~~~~~~~~~~~~~
1 error generated.
error: command '/usr/bin/clang' failed with exit code 1```

Mac (intel) OSX 14.2.1 - though I had the same issue on my M1.

---

_Comment by @charliermarsh on 2024-02-24 00:32_

Can you please include your uv version, the output of running `uv pip install` with `--verbose`, and confirmation that you're able to install this package with `pip` and `--no-cache`?

---

_Label `needs-mre` added by @charliermarsh on 2024-02-24 00:32_

---

_Comment by @hwong557 on 2024-02-24 02:46_

Are you on 3.11? It seems `cchardet` is unsupported on 3.11 and the package seems to be abandoned.

https://github.com/PyYoshi/cChardet/issues/81

But there seems to be a fork: https://github.com/faust-streaming/cChardet

---

_Comment by @chrisgoddard on 2024-02-24 03:27_

Oh weird - 3.12 actually. Normal pip works but I'll check out the fork 

---

_Comment by @hauntsaninja on 2024-02-24 08:11_

Yeah, cchardet is dead. Fwiw, seems like a lot of people are switching to charset_normalizer anyway

---

_Comment by @chrisgoddard on 2024-02-24 18:12_

I'll check that out - quite a few scraping libs have it as an optional dependency for performance.

---

_Comment by @notatallshaw-gts on 2024-02-26 20:21_

> I'll check that out - quite a few scraping libs have it as an optional dependency for performance.

FYI there is a mypyc mode for charset-normalizer for some performance improvement: https://charset-normalizer.readthedocs.io/en/latest/community/speedup.html

---

_Comment by @zanieb on 2024-02-26 23:11_

It looks like there's nothing for us to address here. (please let us know if that's not the case)

---

_Closed by @zanieb on 2024-02-26 23:11_

---

_Referenced in [astral-sh/uv#2646](../../astral-sh/uv/issues/2646.md) on 2024-03-25 11:35_

---
