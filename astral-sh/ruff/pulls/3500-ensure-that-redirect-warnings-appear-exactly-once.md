```yaml
number: 3500
title: Ensure that redirect warnings appear exactly once per code
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/warn
created_at: 2023-03-14T03:28:55Z
updated_at: 2023-03-14T15:30:28Z
url: https://github.com/astral-sh/ruff/pull/3500
synced_at: 2026-01-12T15:55:13Z
```

# Ensure that redirect warnings appear exactly once per code

---

_@charliermarsh_

## Summary

We often have warnings that we want to show users -- but we only want to show those warnings _once_, no matter how many times the triggering codepath executes, since it's not useful to show the same warning multiple times.

Case in point: when the user lists a rule code that has been deprecated and redirected to a _new_ rule code (e.g., `RUF004` was moved to `B026`), that redirect could happen multiple times in a single invocation -- the out-dated rule could appear in multiple parts of the configuration file, it could be provided on the command-line in addition to the configuration file, and so on.

Historically, to show one-time warnings, we used this `warn_user_once!` macro, which relies on an `AtomicBool`. This macro does warn once, but is dependent on the code path. That is: it doesn't support dynamic warnings, since the "state" gets inlined into the call-site.

For the redirect case described above, we currently print that warning multiple times, since we can't use our `warn_user_once!` macro. This PR thus introduces a `warn_user_once_by_id` variant that stores the warnings in a global "registry" with arbitrary keys. By registering each redirect warning based on the redirection code, we avoid repeated warnings even for codes that are redirected multiple times.

Closes #3431.

# Test Plan

Add the following to a `pyproject.toml`:

```toml
[tool.ruff]
select = ["PDV010"]
```

Over an arbitrary file, run: `cargo run -p ruff_cli -- foo.py --select PDV010 -n`.

Verify that exactly one warning appears:

```console
warning: `PDV010` has been remapped to `PD010`.
```



---

_Label `bug` added by @charliermarsh on 2023-03-14 03:28_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-14 03:30_

---

_Review requested from @konstin by @charliermarsh on 2023-03-14 03:30_

---

_Comment by @github-actions[bot] on 2023-03-14 03:40_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/logging.rs`:10 on 2023-03-14 08:21_

Using a `String` as the key has the potential risk that two keys end up with the same name. 

We can use `TypeId` to avoid this. I've done this before for the [`Label`](https://github.com/charliermarsh/ruff/blob/e356ebcc6b6984a2e02dfe55f97f5d36ce0574a5/crates/ruff_formatter/src/format_element/tag.rs#L243) struct in the formatter. 

```rust
#[derive(Eq, PartialEq, Copy, Clone, Debug)]
pub struct WarningId {
    id: TypeId,
	// Pretty name for debugging
    #[cfg(debug_assertions)]
    label: &'static str,
}

impl WarningId {
    pub fn of<T: ?Sized + 'static>() -> Self {
        Self {
            id: TypeId::of::<T>(),
            #[cfg(debug_assertions)]
            label: type_name::<T>(),
        }
    }
}
```

Usage: 

```rust
struct MyWarning;

let id = WarningId::of::<MyWarning>("Some name");
```


Using `TypeId` further has the advantage that using a simple `Vec` and scanning it is probably cheaper (this is based on the assumption that users will have few warnings, let's say less than 16). 

---

_Review comment by @MichaReiser on `crates/ruff/src/logging.rs`:10 on 2023-03-14 08:25_

Hm, okay, you indeed want to match on a string here. The above approach could still work but may be more complicated. 

I would still argue that using a `Vec` instead of a `FxHashMap` is probably cheaper since the lookups are rare and the table only contains few entries. 

---

_@MichaReiser approved on 2023-03-14 08:25_

Can you extend the PR with a test plan.

---

_Review comment by @konstin on `foo.py`:1 on 2023-03-14 10:04_

huh?

---

_@konstin approved on 2023-03-14 10:05_

There's also https://github.com/Luthaf/log-once which might be relevant

---

_Comment by @MichaReiser on 2023-03-14 12:20_

Unrelated to the fix but related to our `warn` and `warn_once` outputs:

We should refactor our code in the long term to avoid writing to stdout/stderr during analysis because they, at best, get printed to stdout when running in WebAssembly and are inaccessible to other APIs. That's why we should only rely on Diagnostics to communicate output to the users. 

Relying on global-state is related. WebAssembly or other APIs can call the linter multiple times, and calling `lint` with the same content should produce the exact result. That means that our internals shouldn't rely on any global state that changes between invocations.  

---

_Comment by @charliermarsh on 2023-03-14 14:29_

> We should refactor our code in the long term to avoid writing to stdout/stderr during analysis...

Wow I hadn't thought about that (RE WebAssembly, APIs, etc.).

---

_Comment by @charliermarsh on 2023-03-14 14:29_

Wondering whether to refactor to use `log_once`, that crate looks reasonable enough.

---

_Comment by @MichaReiser on 2023-03-14 14:33_

> Wondering whether to refactor to use `log_once`, that crate looks reasonable enough.

Our code looks simple enough and this is a pattern that we, ideally, shouldn't encourage. That's why I would prefer our code over having another dependency (dependencies have a maintenance cost too)

---

_Merged by @charliermarsh on 2023-03-14 15:22_

---

_Closed by @charliermarsh on 2023-03-14 15:22_

---

_Branch deleted on 2023-03-14 15:22_

---
