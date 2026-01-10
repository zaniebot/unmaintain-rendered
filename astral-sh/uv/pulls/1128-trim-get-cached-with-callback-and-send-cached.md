```yaml
number: 1128
title: "Trim `get_cached_with_callback` and `send_cached` down some more."
type: pull_request
state: merged
author: konstin
labels:
  - performance
  - windows
assignees: []
merged: true
base: main
head: konsti/smaller-llvm-functions
created_at: 2024-01-26T16:56:13Z
updated_at: 2024-01-29T08:31:28Z
url: https://github.com/astral-sh/uv/pull/1128
synced_at: 2026-01-10T15:39:03Z
```

# Trim `get_cached_with_callback` and `send_cached` down some more.

---

_Pull request opened by @konstin on 2024-01-26 16:56_

I noticed that `get_cached_with_callback` and `send_cached` are large both in terms of llvm lines and in terms of types (and large types can cause buffer overflows on windows). `get_cached_with_callback`  specifically is large because it's monomorphized for each callback. I've split both functions into smaller units and boxed the callback.

llvm lines, before:

```
  Lines                 Copies               Function name
  -----                 ------               -------------
  909511                21625                (TOTAL)
   36026 (4.0%,  4.0%)     33 (0.2%,  0.2%)  <&mut rmp_serde::decode::Deserializer<R,C> as serde::de::Deserializer>::deserialize_any
   14688 (1.6%,  5.6%)      8 (0.0%,  0.2%)  puffin_client::cached_client::CachedClient::get_cached_with_callback::{{closure}}::{{closure}}
   13748 (1.5%,  7.1%)      5 (0.0%,  0.2%)  puffin_client::cached_client::CachedClient::send_cached::{{closure}}
   12460 (1.4%,  8.5%)     35 (0.2%,  0.4%)  alloc::raw_vec::RawVec<T,A>::grow_amortized
   10731 (1.2%,  9.6%)    122 (0.6%,  0.9%)  <alloc::boxed::Box<T,A> as core::ops::drop::Drop>::drop
    8952 (1.0%, 10.6%)      9 (0.0%,  1.0%)  core::slice::sort::partition_in_blocks
    8216 (0.9%, 11.5%)    323 (1.5%,  2.5%)  <core::result::Result<T,E> as core::ops::try_trait::Try>::branch
    7745 (0.9%, 12.4%)    205 (0.9%,  3.4%)  core::result::Result<T,E>::map_err
    6862 (0.8%, 13.1%)     54 (0.2%,  3.7%)  <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
    6720 (0.7%, 13.9%)    133 (0.6%,  4.3%)  std::panicking::try
    6600 (0.7%, 14.6%)     45 (0.2%,  4.5%)  <alloc::sync::Weak<T,A> as core::ops::drop::Drop>::drop
    5899 (0.6%, 15.2%)     33 (0.2%,  4.6%)  rmp_serde::decode::Deserializer<R,C>::read_str_data
    5610 (0.6%, 15.9%)     33 (0.2%,  4.8%)  alloc::raw_vec::RawVec<T,A>::allocate_in
    5187 (0.6%, 16.4%)    133 (0.6%,  5.4%)  std::panicking::try::do_catch
    4740 (0.5%, 17.0%)    268 (1.2%,  6.7%)  core::ops::function::FnOnce::call_once
    4670 (0.5%, 17.5%)     40 (0.2%,  6.8%)  puffin_client::cached_client::CachedClient::get_cached_with_callback::{{closure}}::{{closure}}::{{closure}}
    4527 (0.5%, 18.0%)     54 (0.2%,  7.1%)  core::iter::traits::iterator::Iterator::try_fold
```

after:

```
  Lines                 Copies               Function name
  -----                 ------               -------------
  910275                21712                (TOTAL)
   36026 (4.0%,  4.0%)     33 (0.2%,  0.2%)  <&mut rmp_serde::decode::Deserializer<R,C> as serde::de::Deserializer>::deserialize_any
   12460 (1.4%,  5.3%)     35 (0.2%,  0.3%)  alloc::raw_vec::RawVec<T,A>::grow_amortized
   10935 (1.2%,  6.5%)    124 (0.6%,  0.9%)  <alloc::boxed::Box<T,A> as core::ops::drop::Drop>::drop
    8952 (1.0%,  7.5%)      9 (0.0%,  0.9%)  core::slice::sort::partition_in_blocks
    8714 (1.0%,  8.5%)      5 (0.0%,  0.9%)  puffin_client::cached_client::CachedClient::send_cached_handle_stale::{{closure}}
    8216 (0.9%,  9.4%)    323 (1.5%,  2.4%)  <core::result::Result<T,E> as core::ops::try_trait::Try>::branch
    8192 (0.9%, 10.3%)      8 (0.0%,  2.5%)  puffin_client::cached_client::CachedClient::get_cached_with_callback::{{closure}}::{{closure}}
    7745 (0.9%, 11.1%)    205 (0.9%,  3.4%)  core::result::Result<T,E>::map_err
    6862 (0.8%, 11.9%)     54 (0.2%,  3.7%)  <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
    6778 (0.7%, 12.6%)      5 (0.0%,  3.7%)  puffin_client::cached_client::CachedClient::send_cached::{{closure}}
    6720 (0.7%, 13.4%)    133 (0.6%,  4.3%)  std::panicking::try
    6600 (0.7%, 14.1%)     45 (0.2%,  4.5%)  <alloc::sync::Weak<T,A> as core::ops::drop::Drop>::drop
    5899 (0.6%, 14.7%)     33 (0.2%,  4.7%)  rmp_serde::decode::Deserializer<R,C>::read_str_data
    5610 (0.6%, 15.3%)     33 (0.2%,  4.8%)  alloc::raw_vec::RawVec<T,A>::allocate_in
    5187 (0.6%, 15.9%)    133 (0.6%,  5.4%)  std::panicking::try::do_catch
    4740 (0.5%, 16.4%)    268 (1.2%,  6.7%)  core::ops::function::FnOnce::call_once
    4527 (0.5%, 16.9%)     54 (0.2%,  6.9%)  core::iter::traits::iterator::Iterator::try_fold
```

Stack sizes diff: https://gist.github.com/konstin/a3f38276aacf1170038a756c8c49793c

---

_Label `performance` added by @konstin on 2024-01-26 16:56_

---

_Label `windows` added by @konstin on 2024-01-26 16:56_

---

_Review requested from @BurntSushi by @konstin on 2024-01-26 16:56_

---

_@BurntSushi approved on 2024-01-26 17:56_

LGTM.

---

_Comment by @charliermarsh on 2024-01-26 21:35_

Any idea why the total number of lines increased? (Am I misreading?)

---

_Comment by @konstin on 2024-01-29 08:07_

I think it's just that each function declaration has a bit overhead; It still feels like a good trade

---

_Merged by @konstin on 2024-01-29 08:31_

---

_Closed by @konstin on 2024-01-29 08:31_

---

_Branch deleted on 2024-01-29 08:31_

---
