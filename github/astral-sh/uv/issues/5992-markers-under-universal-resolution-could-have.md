---
number: 5992
title: Markers under universal resolution could have more concise representation 
type: issue
state: closed
author: notatallshaw
labels:
  - wish
assignees: []
created_at: 2024-08-10T03:20:49Z
updated_at: 2025-09-25T13:32:01Z
url: https://github.com/astral-sh/uv/issues/5992
synced_at: 2026-01-07T13:12:17-06:00
---

# Markers under universal resolution could have more concise representation 

---

_Issue opened by @notatallshaw on 2024-08-10 03:20_

uv 0.2.35

Markers are looking much better in general, but here's an example that popped up for me that felt a little too verbose:

```
$ echo -e "sqlalchemy>=2" | uv pip compile --universal --annotation-style line - 2>/dev/null | grep "greenlet=="
greenlet==3.0.3 ; (python_version < '3.13' and platform_machine == 'AMD64') or (python_version < '3.13' and platform_machine == 'WIN32') or (python_version < '3.13' and platform_machine == 'aarch64') or (python_version < '3.13' and platform_machine == 'amd64') or (python_version < '3.13' and platform_machine == 'ppc64le') or (python_version < '3.13' and platform_machine == 'win32') or (python_version < '3.13' and platform_machine == 'x86_64')  # via sqlalchemy
```

This could have been more neatly represented as:

```
greenlet==3.0.3 ; python_version < '3.13'  and (platform_machine == 'AMD64' or platform_machine == 'WIN32' or platform_machine == 'aarch64' or platform_machine == 'amd64' or platform_machine == 'ppc64le' or platform_machine == 'win32' or platform_machine == 'x86_64')  # via sqlalchemy
```

This is fairly small and not that big of a deal.

---

_Comment by @ibraheemdev on 2024-08-10 03:29_

Hmm yeah this is a consequence of us strictly writing in DNF form. Your example does seem like it would be much simpler in CNF.. it's possible that we want more aggressive simplification. However, having a stable representation of markers is also important (see https://github.com/astral-sh/uv/issues/5179), so if we do decide to simplify more we should use an established algorithm instead of continuously adding heuristics (as we were before https://github.com/astral-sh/uv/pull/5898). 

---

_Label `wish` added by @charliermarsh on 2024-08-10 03:31_

---

_Referenced in [astral-sh/uv#5179](../../astral-sh/uv/issues/5179.md) on 2024-08-10 03:46_

---

_Referenced in [astral-sh/uv#6295](../../astral-sh/uv/issues/6295.md) on 2024-08-21 02:39_

---

_Comment by @charliermarsh on 2024-12-27 14:51_

I did look at using DNF when it's more concise, and interestingly it turned out that there were only one or two markers in our entire test suite that fell back to DNF.

---

_Comment by @notatallshaw on 2025-09-25 13:32_

I think this is unlikely to change, and if it does it will likely have a motivating issue other than this one, I'm going to close this.

---

_Closed by @notatallshaw on 2025-09-25 13:32_

---
