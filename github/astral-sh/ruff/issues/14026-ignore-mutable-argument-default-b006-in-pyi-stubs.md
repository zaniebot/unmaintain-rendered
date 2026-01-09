---
number: 14026
title: "Ignore `mutable-argument-default (B006)` in `.pyi` stubs"
type: issue
state: closed
author: jorenham
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-10-31T21:31:36Z
updated_at: 2024-11-03T02:48:49Z
url: https://github.com/astral-sh/ruff/issues/14026
synced_at: 2026-01-07T13:12:16-06:00
---

# Ignore `mutable-argument-default (B006)` in `.pyi` stubs

---

_Issue opened by @jorenham on 2024-10-31 21:31_

[Mutable argument defaults](https://docs.astral.sh/ruff/rules/mutable-argument-default/) can only be a problem at runtime. But at the moment, `B006` will also be reported in `.pyi` stub files, even though it is certain that mutations can never occur there.

For example, consider the following function in [`scipy-stubs/stats/_mstats_basic.pyi:578`](https://github.com/jorenham/scipy-stubs/blob/5a2603e9fc3c37e9355773da31d294ac688d699d/scipy-stubs/stats/_mstats_basic.pyi#L578-L585):

```pyi
def mquantiles(
    a: _ArrayLikeFloat_co,
    prob: _ArrayLikeFloat_co = [0.25, 0.5, 0.75],
    alphap: AnyReal = 0.4,
    betap: AnyReal = 0.4,
    axis: CanIndex | None = None,
    limit: tuple[AnyReal, AnyReal] | tuple[()] = (),
) -> _MArrayND: ...
```

For the `prob` parameter the following error is reported:

```
scipy-stubs/stats/_mstats_basic.pyi:580:32: B006 Do not use mutable data structures for argument defaults
```

I think that "mutable" defaults should be allowed in cases like these. Having explicit parameter defaults that match the runtime can be seen as a form of documentation. This is especially useful when the runtime sources aren't available, which is quite common in this [`scipy-stubs`](https://github.com/jorenham/scipy-stubs) case.


---

_Comment by @AlexWaygood on 2024-10-31 22:02_

Yeah, your argument makes sense. You want to emulate the runtime in a stub file; whether or not the runtime _should_ be using mutable defaults or not, you'll want to accurately represent that in the stub.

---

_Label `help wanted` added by @AlexWaygood on 2024-10-31 22:03_

---

_Label `accepted` added by @AlexWaygood on 2024-10-31 22:03_

---

_Renamed from "`mutable-argument-default (B006)` in `.pyi` stubs" to "Ignore `mutable-argument-default (B006)` in `.pyi` stubs" by @AlexWaygood on 2024-10-31 22:03_

---

_Label `rule` added by @dhruvmanila on 2024-11-01 04:46_

---

_Referenced in [astral-sh/ruff#14058](../../astral-sh/ruff/pulls/14058.md) on 2024-11-02 22:09_

---

_Closed by @charliermarsh on 2024-11-03 02:48_

---

_Closed by @charliermarsh on 2024-11-03 02:48_

---
