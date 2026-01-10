```yaml
number: 108
title: "salsa panic \"attempt to subtract with overflow\" in table/memo.rs"
type: issue
state: closed
author: carljm
labels:
  - bug
assignees: []
created_at: 2025-04-30T23:26:01Z
updated_at: 2025-05-08T10:02:55Z
url: https://github.com/astral-sh/ty/issues/108
synced_at: 2026-01-10T02:34:09Z
```

# salsa panic "attempt to subtract with overflow" in table/memo.rs

---

_Issue opened by @carljm on 2025-04-30 23:26_

```
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/table/memo.rs:225:30 when checking `/tmp/mypy_primer/projects/optuna/optuna/visualization/_rank.py`: `attempt to subtract with overflow`
```

Reproduces (but not consistently) on ruff commit `547f159e3f5e35023c62b8962a3d26e0bc14450d` (PR https://github.com/astral-sh/ruff/pull/17589) checking the mypy-primer project `optuna`: https://github.com/astral-sh/ruff/actions/runs/14765384601/job/41455638363?pr=17589

---

_Label `bug` added by @carljm on 2025-04-30 23:26_

---

_Comment by @MichaReiser on 2025-05-01 05:58_

Probably related to https://github.com/astral-sh/ty/issues/115

I don't think we need to fix those two for the alpha. They only reproduce once every 1000th run or something like that (I haven't had any look to repro even when running red knot for minutes)

---

_Comment by @AlexWaygood on 2025-05-03 14:49_

Another occurence of this, this time while checking `starlette`: https://github.com/astral-sh/ruff/actions/runs/14811731265/job/41587228964 (CI run for https://github.com/astral-sh/ruff/pull/17813)

---

_Renamed from "[red-knot] salsa panic "attempt to subtract with overflow" in table/memo.rs" to "salsa panic "attempt to subtract with overflow" in table/memo.rs" by @MichaReiser on 2025-05-07 15:24_

---

_Closed by @sharkdp on 2025-05-08 10:02_

---

_Closed by @sharkdp on 2025-05-08 10:02_

---
