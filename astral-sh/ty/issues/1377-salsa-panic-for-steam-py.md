```yaml
number: 1377
title: "Salsa panic for `steam.py`"
type: issue
state: closed
author: MichaReiser
labels:
  - fatal
assignees: []
created_at: 2025-10-17T08:35:33Z
updated_at: 2025-10-24T16:31:36Z
url: https://github.com/astral-sh/ty/issues/1377
synced_at: 2026-01-12T15:54:25Z
```

# Salsa panic for `steam.py`

---

_@MichaReiser_

With the new salsa version merged in https://github.com/astral-sh/ruff/pull/20645 and after adding cycle handing to `lazy_default`, ty panics in 1/3 of the runs with `Provisional memo retry limit exceeded for ... this usually indicates a bug in salsa's cycle caching/locking.`


The panic seems to be related to multithreading, as I can't reproduce it when running in a single thread (`TY_MAX_PARALLELISM=1`). Two threads tend to be sufficient to trigger it.

Here a specifc instance of the panic, enriched with the current cycle heads of the query panicking:

```
Panicked at /Users/micha/astral/salsa/src/function/memo.rs:166:17 when checking `/private/tmp/mypy_primer/projects/steam.py/steam/message.py`: `Provisional retry for memo infer_deferred_types(Id(609b)) exceeded.
Cycle heads: CycleHeads([
    CycleHead { database_key_index: infer_deferred_types(Id(978a)), iteration_count: AtomicIterationCount(2), removed: false }, 
    CycleHead { database_key_index: infer_deferred_types(Id(9782)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: TypeVarInstance < 'db >::lazy_default_(Id(5a403)), iteration_count: AtomicIterationCount(0), removed: false },
    CycleHead { database_key_index: TypeVarInstance < 'db >::lazy_bound_(Id(5a404)), iteration_count: AtomicIterationCount(2), removed: false },
    CycleHead { database_key_index: infer_deferred_types(Id(9783)), iteration_count: AtomicIterationCount(2), removed: false }, 
    CycleHead { database_key_index: infer_definition_types(Id(605f)), iteration_count: AtomicIterationCount(2), removed: false }, 
    CycleHead { database_key_index: infer_definition_types(Id(9406)), iteration_count: AtomicIterationCount(1), removed: false }, 
    CycleHead { database_key_index: infer_definition_types(Id(616b)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: Type < 'db >::member_lookup_with_policy_(Id(1cc0b)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: infer_definition_types(Id(5e4f2)), iteration_count: AtomicIterationCount(1), removed: false }, 
    CycleHead { database_key_index: ClassLiteral < 'db >::explicit_bases_(Id(22406)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: Type < 'db >::is_redundant_with_(Id(2bc0d)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: infer_definition_types(Id(9787)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: TypeVarInstance < 'db >::lazy_bound_(Id(5a400)), iteration_count: AtomicIterationCount(1), removed: false }, 
    CycleHead { database_key_index: ClassLiteral < 'db >::is_typed_dict_(Id(2240d)), iteration_count: AtomicIterationCount(1), removed: false }, 
    CycleHead { database_key_index: ClassLiteral < 'db >::explicit_bases_(Id(2240d)), iteration_count: AtomicIterationCount(1), removed: false }, 
    CycleHead { database_key_index: infer_deferred_types(Id(9419)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: TypeVarInstance < 'db >::lazy_bound_(Id(5a402)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: infer_deferred_types(Id(9626)), iteration_count: AtomicIterationCount(0), removed: false }, 
    CycleHead { database_key_index: ClassLiteral < 'db >::explicit_bases_(Id(3a007)), iteration_count: AtomicIterationCount(1), removed: false }
])

Cycle { head_iteration_count: IterationCount(1), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: ClassLiteral < 'db >::explicit_bases_(Id(3a007)), inner: true },  <-- updated
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: infer_deferred_types(Id(9626)), inner: true },  <-- updated
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: TypeVarInstance < 'db >::lazy_bound_(Id(5a402)), inner: true },  <-- updated
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: infer_deferred_types(Id(9419)), inner: true },  <-- updated
Cycle { head_iteration_count: IterationCount(1), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: ClassLiteral < 'db >::explicit_bases_(Id(2240d)), inner: true },  <-- updated
Cycle { head_iteration_count: IterationCount(1), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: ClassLiteral < 'db >::is_typed_dict_(Id(2240d)), inner: true },  <-- updated
Cycle { head_iteration_count: IterationCount(1), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: TypeVarInstance < 'db >::lazy_bound_(Id(5a400)), inner: false },  <-- updated
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: infer_definition_types(Id(9787)), inner: true },  <-- updated
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: Type < 'db >::is_redundant_with_(Id(2bc0d)), inner: true }, <-- updated
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: ClassLiteral < 'db >::explicit_bases_(Id(22406)), inner: true }, <-- updated
Cycle { head_iteration_count: IterationCount(1), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: infer_definition_types(Id(5e4f2)), inner: false },  <-- updated
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(0), verified_at: R2, database_key_index: Type < 'db >::member_lookup_with_policy_(Id(1cc0b)), inner: false }, 
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(0), verified_at: R2, database_key_index: infer_definition_types(Id(616b)), inner: false }, 
Cycle { head_iteration_count: IterationCount(1), memo_iteration_count: IterationCount(1), verified_at: R2, database_key_index: infer_definition_types(Id(9406)), inner: false }, 
Cycle { head_iteration_count: IterationCount(2), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: infer_definition_types(Id(605f)), inner: false }, 
Cycle { head_iteration_count: IterationCount(2), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: infer_deferred_types(Id(9783)), inner: false }, 
Cycle { head_iteration_count: IterationCount(2), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: TypeVarInstance < 'db >::lazy_bound_(Id(5a404)), inner: false }, 
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(0), verified_at: R2, database_key_index: TypeVarInstance < 'db >::lazy_default_(Id(5a403)), inner: false }, 
Cycle { head_iteration_count: IterationCount(0), memo_iteration_count: IterationCount(0), verified_at: R2, database_key_index: infer_deferred_types(Id(9782)), inner: false }, 
Cycle { head_iteration_count: IterationCount(2), memo_iteration_count: IterationCount(2), verified_at: R2, database_key_index: infer_deferred_types(Id(978a)), inner: false }, 

```

I don't fully understand what's happening yet but:

* The query is blocked in `blocks_on`
* The query is retried because there are newer memos for some of its cycle heads (they have a different iteration count)
* The query re-executes but it ends up with the same outdated cycle heads after every retry. This suggests that salsa returns cached results for its dependencies when it shouldn't.
* The query itself isn't a cycle head (but it has cycle handling enabled)
* Changing `validate_maybe_iterate` to recursively fetch the cycle heads seems to fix the issue (why?)


... A fixpoint iteration with that many nested cycle heads would never have completed with the old salsa version :)


---

_Assigned to @MichaReiser by @MichaReiser on 2025-10-17 08:35_

---

_Label `fatal` added by @MichaReiser on 2025-10-17 08:35_

---

_Added to milestone `Beta` by @MichaReiser on 2025-10-17 08:39_

---

_Comment by @MichaReiser on 2025-10-23 07:47_

This was fixed in https://github.com/astral-sh/ruff/pull/21038

---

_Closed by @MichaReiser on 2025-10-23 07:47_

---

_Reopened by @MichaReiser on 2025-10-23 08:13_

---

_Comment by @MichaReiser on 2025-10-24 16:31_

This should, hopefully, now be fixed for good, see https://github.com/astral-sh/ruff/pull/21059

---

_Closed by @MichaReiser on 2025-10-24 16:31_

---
