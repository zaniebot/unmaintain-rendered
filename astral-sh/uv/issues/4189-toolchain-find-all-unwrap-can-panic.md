```yaml
number: 4189
title: "`Toolchain::find_all` unwrap can panic"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-10T08:04:09Z
updated_at: 2024-06-10T14:22:01Z
url: https://github.com/astral-sh/uv/issues/4189
synced_at: 2026-01-12T15:58:48Z
```

# `Toolchain::find_all` unwrap can panic

---

_@konstin_

The following unwrap in toolchain discovery can panic:

https://github.com/astral-sh/uv/blob/53035d65a14503769c9749e9ce60e4c22a72af1c/crates/uv-toolchain/src/managed.rs#L104

```
thread 'seed' panicked at crates/uv-toolchain/src/managed.rs:104:55:
called `Result::unwrap()` on an `Err` value: NameError("not enough `-`-separated values")
stack backtrace:
   0: rust_begin_unwind
   1: core::panicking::panic_fmt
   2: core::result::unwrap_failed
   3: uv_toolchain::managed::InstalledToolchains::find_all::{{closure}}
   4: core::iter::adapters::map::map_try_fold::{{closure}}
   5: core::iter::traits::double_ended::DoubleEndedIterator::try_rfold
   6: <core::iter::adapters::map::Map<I,F> as core::iter::traits::double_ended::DoubleEndedIterator>::try_rfold
   7: <core::iter::adapters::rev::Rev<I> as core::iter::traits::iterator::Iterator>::try_fold
   8: <core::iter::adapters::filter::Filter<I,P> as core::iter::traits::iterator::Iterator>::try_fold
   9: core::iter::traits::iterator::Iterator::find
  10: <core::iter::adapters::filter::Filter<I,P> as core::iter::traits::iterator::Iterator>::next
  11: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::next
  12: alloc::vec::Vec<T,A>::extend_desugared
  13: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
  14: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  15: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  16: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  17: core::iter::traits::iterator::Iterator::collect
  18: venv::common::python_path_with_versions::{{closure}}::{{closure}}
  19: core::result::Result<T,E>::map
  20: venv::common::python_path_with_versions::{{closure}}
  21: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &mut F>::call_once
  22: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::next
  23: <core::iter::adapters::fuse::Fuse<I> as core::iter::adapters::fuse::FuseImpl<I>>::next
  24: <core::iter::adapters::flatten::FlattenCompat<I,U> as core::iter::traits::iterator::Iterator>::next
  25: <core::iter::adapters::flatten::FlatMap<I,U,F> as core::iter::traits::iterator::Iterator>::next
  26: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
  27: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
  28: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
  29: core::iter::traits::iterator::Iterator::collect
  30: venv::common::python_path_with_versions
  31: venv::VenvTestContext::new
  32: venv::seed
  33: venv::seed::{{closure}}
  34: core::ops::function::FnOnce::call_once
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

---

_Label `bug` added by @konstin on 2024-06-10 08:04_

---

_Label `preview` added by @konstin on 2024-06-10 08:04_

---

_Comment by @zanieb on 2024-06-10 13:46_

I've fixed this already in one of the upcoming pull requests to productionize toolchain management.

---

_Comment by @charliermarsh on 2024-06-10 13:46_

Is this panic user-accessible on `main`?

---

_Comment by @charliermarsh on 2024-06-10 13:47_

(Do we need to fix it prior to today's release?)

---

_Comment by @konstin on 2024-06-10 13:49_

I think it's from the downloaded toolchains, so a preview feature

---

_Comment by @zanieb on 2024-06-10 14:03_

This has been accessible on main for a while now. It's only relevant if you are using managed toolchains and pollute the toolchain directory. I could fix it separately from my stack if you want though?

---

_Closed by @zanieb on 2024-06-10 14:22_

---
