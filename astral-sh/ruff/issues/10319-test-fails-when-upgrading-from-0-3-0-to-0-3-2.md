```yaml
number: 10319
title: Test fails when upgrading from 0.3.0 to 0.3.2
type: issue
state: closed
author: eli-schwartz
labels:
  - bug
  - testing
assignees: []
created_at: 2024-03-10T08:04:17Z
updated_at: 2024-03-12T21:11:19Z
url: https://github.com/astral-sh/ruff/issues/10319
synced_at: 2026-01-12T15:54:50Z
```

# Test fails when upgrading from 0.3.0 to 0.3.2

---

_@eli-schwartz_

I tried to bump the version of ruff and rebuild it for Gentoo. Running the tests failed:

```
running 22 tests
test helpers::tests::any_over_stmt_type_alias ... ok
test helpers::tests::any_over_type_param_type_var_tuple ... ok
test helpers::tests::any_over_type_param_param_spec ... ok
test helpers::tests::resolve_import ... ok
test identifier::tests::extract_global_names ... ok
test helpers::tests::any_over_type_param_type_var ... ok
test name::tests::empty_vec ... ok
test name::tests::extend_from_slice_heap ... ok
test name::tests::extend_from_slice_stack ... ok
test name::tests::extend_from_slice_stack_spill ... ok
test name::tests::extend_heap ... ok
test name::tests::extend_stack ... ok
test name::tests::extend_stack_spilled ... ok
test name::tests::from_slice_heap ... ok
test name::tests::from_slice_stack ... ok
test name::tests::from_slice_stack_capacity ... ok
test name::tests::pop_heap ... ok
test name::tests::pop_stack ... ok
test name::tests::push_stack ... ok
test name::tests::push_stack_spill ... ok
test nodes::tests::size ... FAILED
test str::tests::prefix_uniqueness ... ok

failures:

---- nodes::tests::size stdout ----
thread 'nodes::tests::size' panicked at crates/ruff_python_ast/src/nodes.rs:4151:9:
assertion `left == right` failed
  left: 56
 right: 48
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    nodes::tests::size

test result: FAILED. 21 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

error: test failed, to rerun pass `-p ruff_python_ast --lib`
```

---

_Comment by @charliermarsh on 2024-03-10 16:12_

Do you know what version of Rust you're compiling with?

---

_Comment by @eli-schwartz on 2024-03-10 16:17_

```
$ rustc --version
rustc 1.74.1 (a28077b28 2023-12-04) (gentoo)
```

---

_Comment by @charliermarsh on 2024-03-10 18:59_

I think those tests effectively require `1.76` (our `rust-toolchain.tml`), since Rust's enum layout improved. We may want to gate them on Rust version or something similar.

---

_Comment by @dhruvmanila on 2024-03-11 03:00_

We do have a case where it tests against 2 values although not gated by anything. I think we can just use that pattern for any other types.

---

_Label `bug` added by @dhruvmanila on 2024-03-11 03:01_

---

_Comment by @zanieb on 2024-03-11 16:52_

@dhruvmanila can you link to that?

---

_Label `testing` added by @zanieb on 2024-03-11 16:52_

---

_Comment by @dhruvmanila on 2024-03-11 17:00_

Huh, I thought I pasted the link but nevertheless:

https://github.com/astral-sh/ruff/blob/ad84eedc18e1af2de1f297ab307e7cb8548335ed/crates/ruff_python_ast/src/nodes.rs#L4136-L4137

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-12 19:29_

---

_Comment by @charliermarsh on 2024-03-12 19:30_

It's a bit of a shame because it doesn't really test the same thing. We _could_ regress size there and not notice it.

---

_Closed by @charliermarsh on 2024-03-12 19:46_

---

_Comment by @charliermarsh on 2024-03-12 19:46_

Should be fixed in the next release (or on `main`, since you're building from source).

---

_Comment by @eli-schwartz on 2024-03-12 21:11_

Thanks, I can backport the commit.

> It's a bit of a shame because it doesn't really test the same thing. We _could_ regress size there and not notice it.

The obvious and straightforward answer here is to use a cfg for checking the compiler version and deciding whether to compile that test assertion. However my understanding is that there is no builtin cfg for it so you'll have to write and compile a rust program that then runs rustc --version, parses the output, and prints cfg flags to stdout for cargo...

---
