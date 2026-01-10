```yaml
number: 14022
title: "[red-knot] Follow up: Type::Unbound removal"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2024-10-31T18:51:47Z
updated_at: 2024-11-12T14:30:23Z
url: https://github.com/astral-sh/ruff/issues/14022
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] Follow up: Type::Unbound removal

---

_Issue opened by @sharkdp on 2024-10-31 18:51_

This is a follow-up from astral-sh/ruff#13671 that lists some tasks that were intentionally left out from astral-sh/ruff#13980:

- [x] Work on the `Symbol` API: Functions like `as_type`, `unwrap_or`, `unwrap_or_never` ignore possibly-unboundness. Look into call sites of those functions and see if that's really what we require in all cases. Possibly remove some of these functions. 
- [x] `.unwrap_or` is currently only used with `Type::Never`. We did this to keep existing behavior for some cases where we then proceed to call `.call(…)`, where `Type::Never` would lead to a "not callable" error, just like `Type::Unbound` before. This can be handled more explicitly, potentially with better diagnostics for the user.
- [x] Understand if we need to handle the possibly-unboundness of the `replacement` argument in `Symbol::replace_unbound_with`. Write tests.
- [x] Investigate and understand the performance gains of astral-sh/ruff#13671.
- [x] Possibly look into undeclared-ness issues brought up [here](https://github.com/astral-sh/ruff/pull/13980#discussion_r1824747585). See also the corresponding TODO comment in `types.rs`. => moved to astral-sh/ty#229
- [x] Clarify if we want a diagnostic for in the situation described [here](https://github.com/astral-sh/ruff/pull/13980#discussion_r1824405789) => moved to astral-sh/ty#229 
- [x] Review the `Type::Union` case in the `Type::member()` function, potentially write some more tests

---

_Label `red-knot` added by @sharkdp on 2024-10-31 18:51_

---

_Assigned to @sharkdp by @sharkdp on 2024-10-31 18:51_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:23_

---

_Comment by @sharkdp on 2024-11-12 14:14_

## Performance investigation

I benchmarked two states of the repository: just before and right after the merge of the `Type::Unbound`-removal PR (before: d1189c20d vs. after: 53fa32a38).

The performance gain is small, but real (statistically significant; smaller mean and min times):

```
hyperfine -w50 -r500 -N -i \
  'red_knot_before --current-directory path/to/tomllib' -n before \
  'red_knot_after --current-directory path/to/tomllib' -n after \
  --export-json results.json
uv run path/to/hyperfine/scripts/plot_whisker.py results.json
```

![image](https://github.com/user-attachments/assets/793c8e86-e3e5-4b04-bd30-04bf940b9283)

I get similar results (~ -5%) on the criterion benchmark.

I then looked into the profiling results, and long story short (hint: use inverted call stacks), I believe the initial guess by @MichaReiser is correct: it looks like we simply build fewer union types (previously, we had lots of `… | Type::Unbound` unions).

before: ~4% of the total time is spent on `UnionType::from_elements`

![image](https://github.com/user-attachments/assets/0ba1a1eb-fff1-4ab1-9820-f7fb2a38fe29)

after: only 2% of the total time is spent building union types

![image](https://github.com/user-attachments/assets/a6e7a06b-3344-4b1d-88db-42708fe9a935)

There might be other small gains somewhere else, but at this point, it does not seem worth spending more time on it.

---

_Closed by @sharkdp on 2024-11-12 14:14_

---

_Comment by @sharkdp on 2024-11-12 14:30_

Closing this with one remaining follow up (#14297) which is unrelated to the changes done astral-sh/ruff#13980.

---
