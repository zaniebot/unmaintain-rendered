```yaml
number: 5743
title: "Avoid monomorphization of `untar`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/mono
created_at: 2024-08-03T01:24:54Z
updated_at: 2024-08-03T23:29:18Z
url: https://github.com/astral-sh/uv/pull/5743
synced_at: 2026-01-10T13:37:23Z
```

# Avoid monomorphization of `untar`

---

_Pull request opened by @charliermarsh on 2024-08-03 01:24_

## Summary

This reduces the LLVM lines of the entire project by about 15%.

Before:

```
  Lines                  Copies               Function name
  -----                  ------               -------------
  1742332                42054                (TOTAL)
    22384 (1.3%,  1.3%)     16 (0.0%,  0.0%)  tokio_tar::entry::EntryFields<R>::unpack::{{closure}}
    19113 (1.1%,  2.4%)     69 (0.2%,  0.2%)  alloc::raw_vec::RawVec<T,A>::grow_amortized
    18352 (1.1%,  3.4%)     80 (0.2%,  0.4%)  tokio_tar::entry::EntryFields<R>::unpack::{{closure}}::{{closure}}
    16871 (1.0%,  4.4%)    613 (1.5%,  1.9%)  core::result::Result<T,E>::map_err
    14747 (0.8%,  5.2%)    594 (1.4%,  3.3%)  <core::result::Result<T,E> as core::ops::try_trait::Try>::branch
    12576 (0.7%,  6.0%)     16 (0.0%,  3.3%)  <tokio_tar::archive::Entries<R> as futures_core::stream::Stream>::poll_next
    12314 (0.7%,  6.7%)    226 (0.5%,  3.8%)  <alloc::boxed::Box<T,A> as core::ops::drop::Drop>::drop
    11895 (0.7%,  7.4%)    296 (0.7%,  4.5%)  std::panicking::try
    11718 (0.7%,  8.0%)     63 (0.1%,  4.7%)  alloc::raw_vec::RawVec<T,A>::try_allocate_in
    11152 (0.6%,  8.7%)     16 (0.0%,  4.7%)  uv_extract::stream::untar_in::{{closure}}
    10977 (0.6%,  9.3%)      1 (0.0%,  4.7%)  uv::run::{{closure}}::{{closure}}
    10859 (0.6%,  9.9%)     77 (0.2%,  4.9%)  <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
    10508 (0.6%, 10.5%)     18 (0.0%,  5.0%)  <core::iter::adapters::flatten::FlattenCompat<I,U> as core::iter::traits::iterator::Iterator>::size_hint
    10260 (0.6%, 11.1%)    138 (0.3%,  5.3%)  core::iter::traits::iterator::Iterator::try_fold
    10196 (0.6%, 11.7%)      8 (0.0%,  5.3%)  uv_extract::stream::unzip::{{closure}}
    10178 (0.6%, 12.3%)      7 (0.0%,  5.3%)  uv_client::cached_client::CachedClient::get_cacheable::{{closure}}::{{closure}}
     9698 (0.6%, 12.8%)    293 (0.7%,  6.0%)  tokio::loom::std::unsafe_cell::UnsafeCell<T>::with_mut
```

After:

```
  Lines                  Copies               Function name
  -----                  ------               -------------
  1496463                37891                (TOTAL)
    14958 (1.0%,  1.0%)     54 (0.1%,  0.1%)  alloc::raw_vec::RawVec<T,A>::grow_amortized
    13997 (0.9%,  1.9%)    564 (1.5%,  1.6%)  <core::result::Result<T,E> as core::ops::try_trait::Try>::branch
    12776 (0.9%,  2.8%)    463 (1.2%,  2.9%)  core::result::Result<T,E>::map_err
    12381 (0.8%,  3.6%)    227 (0.6%,  3.5%)  <alloc::boxed::Box<T,A> as core::ops::drop::Drop>::drop
    11895 (0.8%,  4.4%)    296 (0.8%,  4.2%)  std::panicking::try
    10977 (0.7%,  5.1%)      1 (0.0%,  4.2%)  uv::run::{{closure}}::{{closure}}
    10859 (0.7%,  5.9%)     77 (0.2%,  4.4%)  <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
    10508 (0.7%,  6.6%)     18 (0.0%,  4.5%)  <core::iter::adapters::flatten::FlattenCompat<I,U> as core::iter::traits::iterator::Iterator>::size_hint
    10196 (0.7%,  7.3%)      8 (0.0%,  4.5%)  uv_extract::stream::unzip::{{closure}}
    10178 (0.7%,  7.9%)      7 (0.0%,  4.5%)  uv_client::cached_client::CachedClient::get_cacheable::{{closure}}::{{closure}}
     9698 (0.6%,  8.6%)    293 (0.8%,  5.3%)  tokio::loom::std::unsafe_cell::UnsafeCell<T>::with_mut
     9078 (0.6%,  9.2%)      9 (0.0%,  5.3%)  core::slice::sort::partition_in_blocks
     8928 (0.6%,  9.8%)     48 (0.1%,  5.4%)  alloc::raw_vec::RawVec<T,A>::try_allocate_in
     8288 (0.6%, 10.3%)    296 (0.8%,  6.2%)  std::panicking::try::do_catch
     8190 (0.5%, 10.9%)    108 (0.3%,  6.5%)  core::iter::traits::iterator::Iterator::try_fold
     7540 (0.5%, 11.4%)    466 (1.2%,  7.7%)  core::ops::function::FnOnce::call_once
     6612 (0.4%, 11.8%)    296 (0.8%,  8.5%)  std::panicking::try::do_call
     6513 (0.4%, 12.3%)     56 (0.1%,  8.7%)  tokio::runtime::task::core::Cell<T,S>::new
     6438 (0.4%, 12.7%)    269 (0.7%,  9.4%)  alloc::boxed::Box<T>::new
     6360 (0.4%, 13.1%)     20 (0.1%,  9.4%)  <toml_edit::de::value::ValueDeserializer as serde::de::Deserializer>::deserialize_any
```


---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-03 01:24_

---

_Label `internal` added by @charliermarsh on 2024-08-03 01:25_

---

_Marked ready for review by @charliermarsh on 2024-08-03 01:25_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-03 01:28_

---

_@charliermarsh reviewed on 2024-08-03 01:29_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:108 on 2024-08-03 01:29_

@BurntSushi @ibraheemdev -- How significant would you expect the penalty to be here for using dynamic dispatch?

---

_@ibraheemdev reviewed on 2024-08-03 02:02_

---

_Review comment by @ibraheemdev on `crates/uv-extract/src/stream.rs`:108 on 2024-08-03 02:02_

I suspect the cost of I/O greatly outweights the extra indirection here, though the massive line reduction suggests the calls to `poll_read` were being inlined, so there likely will be some penalty. I'm not sure how hot of a path `untar` is that it would be noticeable.

Do we need the `Box` here? `&mut dyn AsyncRead` should work, I think.

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:108 on 2024-08-03 17:38_

I can't quite get `&mut dyn AsyncRead` to work:

```
error[E0308]: mismatched types
   --> crates/uv-extract/src/stream.rs:170:17
    |
170 |     Ok(untar_in(archive, target.as_ref()).await?)
    |        -------- ^^^^^^^ expected `Archive<&mut ...>`, found `Archive<&mut GzipDecoder<...>>`
    |        |
    |        arguments to this function are incorrect
    |
    = note: expected struct `tokio_tar::Archive<&mut dyn tokio::io::AsyncRead + Unpin>`
               found struct `tokio_tar::Archive<&mut async_compression::tokio::bufread::GzipDecoder<tokio::io::BufReader<R>>>`
    = help: `async_compression::tokio::bufread::GzipDecoder<tokio::io::BufReader<R>>` implements `AsyncRead` so you could box the found value and coerce it to the trait object `Box<dyn AsyncRead>`, you will have to change the expected type as well
```

---

_@charliermarsh reviewed on 2024-08-03 17:38_

---

_@ibraheemdev reviewed on 2024-08-03 19:14_

---

_Review comment by @ibraheemdev on `crates/uv-extract/src/stream.rs`:108 on 2024-08-03 19:14_

You might have to do `(&mut reader as &mut dyn ...)` before wrapping it in `Archive`.

---

_@charliermarsh reviewed on 2024-08-03 23:06_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:108 on 2024-08-03 23:06_

Success, thank you!

---

_Merged by @charliermarsh on 2024-08-03 23:29_

---

_Closed by @charliermarsh on 2024-08-03 23:29_

---

_Branch deleted on 2024-08-03 23:29_

---
