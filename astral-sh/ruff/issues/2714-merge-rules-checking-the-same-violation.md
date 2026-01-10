```yaml
number: 2714
title: Merge rules checking the same violation
type: issue
state: open
author: not-my-profile
labels:
  - tracking
assignees: []
created_at: 2023-02-10T06:34:11Z
updated_at: 2024-07-26T01:57:47Z
url: https://github.com/astral-sh/ruff/issues/2714
synced_at: 2026-01-10T11:09:45Z
```

# Merge rules checking the same violation

---

_Issue opened by @not-my-profile on 2023-02-10 06:34_

As per the rule naming convention established in #2712 the rule name should be the bad thing that's being checked for and not contain the solution, so we should merge the following rules accordingly:

* [ ] Merge use-contextlib-suppress (SIM105) and try-except-pass (S110) to try-except-pass
* [ ] Merge blind-except (BLE001) with bare-except (E722)
* [ ] Merge if-expr-with-true-false (SIM210) and if-expr-with-false-true (SIM211) into needless-bool (SIM103)
* [ ] Merge EM101, EM102 and EM103
* [x] #2903
* [x] https://github.com/astral-sh/ruff/issues/7283
* [ ] https://github.com/astral-sh/ruff/issues/11403

In the long run it would be nice if ruff supported different autofixes for one violation, see #2713.

---

_Comment by @charliermarsh on 2023-02-10 14:10_

Agreed. Not sure which to "remove" (i.e., treat as the canonical rule, as opposed to the redirect), but I guess that gets back to the discussion in #2517.

---

_Comment by @not-my-profile on 2023-02-10 14:16_

I don't think it matters much which code we map and which one we redirect for now, since #2517 should land soon(tm).

---

_Renamed from "Merge use-contextlib-suppress and try-except-pass" to "Merge rules checking the same violation" by @not-my-profile on 2023-02-11 05:25_

---

_Comment by @charliermarsh on 2023-02-12 17:56_

To unify `blind-` and `bare-except`, we need to change `bare-except` to flag `except Exception` and `except BaseException`. I assume that's fine.

---

_Comment by @not-my-profile on 2023-02-13 04:45_

Ah right ... good catch! I think `except:` and `except BaseException:` are equivalent since every exception must derive `BaseException`. `except Exception` is different though since some exceptions e.g. `GeneratorExit`, `KeyboardInterrupt` and `SystemExit` don't derive from it. So we may want to split `blind-except` into `except-any` and `except-exception`.

---

_Comment by @charliermarsh on 2023-02-13 04:52_

We could also make it a setting on the rule, rather than creating separate rules. But I don’t think we’ve articulated a strict philosophy either way there.

---

_Comment by @andersk on 2023-02-14 06:05_

use-contextlib-suppress (SIM105) is intentionally much weaker than try-except-pass (S110), and it’s not unreasonable to enable the former without the latter. See #1948.

---

_Label `tracking` added by @dhruvmanila on 2024-05-13 11:21_

---
