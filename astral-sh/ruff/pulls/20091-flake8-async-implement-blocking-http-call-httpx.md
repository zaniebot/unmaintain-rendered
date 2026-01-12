```yaml
number: 20091
title: "[`flake8-async`] Implement `blocking-http-call-httpx` (`ASYNC212`)"
type: pull_request
state: merged
author: amyreese
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: async212
created_at: 2025-08-26T00:18:42Z
updated_at: 2025-08-28T16:56:31Z
url: https://github.com/astral-sh/ruff/pull/20091
synced_at: 2026-01-12T15:56:54Z
```

# [`flake8-async`] Implement `blocking-http-call-httpx` (`ASYNC212`)

---

_@amyreese_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds new rule to find and report use of `httpx.Client` in synchronous
functions.

See issue #8451

## Test Plan

New snapshots for `ASYNC212.py` with `cargo insta test`.


---

_Comment by @amyreese on 2025-08-26 00:23_

Still needs some work (and learning) to catch usage of `httpx.Client` methods, not just instantiation.

---

_Comment by @github-actions[bot] on 2025-08-26 00:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@amyreese reviewed on 2025-08-27 00:24_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC212.py`:61 on 2025-08-27 00:24_

This one still isn't caught, presumably because the TypeChecker bits only capture annotations at the function level?

---

_Marked ready for review by @amyreese on 2025-08-27 00:24_

---

_Label `rule` added by @ntBre on 2025-08-27 12:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_http_call_httpx.rs`:45 on 2025-08-27 13:03_

You don't have to do this, but if you want to include the blocking call and/or the client in the message, you can include their names on the struct. Something like:

```rust
#[derive(ViolationMetadata)]
pub(crate) struct BlockingHttpCallHttpxInAsyncFunction {
    call: String,
    client: String,
}

impl Violation for BlockingHttpCallHttpxInAsyncFunction {
    #[derive_message_formats]
    fn message(&self) -> String {
        format!(
            "Blocking sync HTTP call {call} on httpx object {client}, use httpx.AsyncClient.",
            call = self.call, 
            client = self.client,
        )
    }
}
```

Nice work on the upstream contribution, I saw it when checking the upstream rule docs this morning :)

We also don't have to match the upstream phrasing, just using that as an idea to let you know this was possible.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_http_call_httpx.rs`:73 on 2025-08-27 13:07_

I'm not sure we need this `traverse_union_and_optional` call, but to be honest I haven't written one of these `TypeChecker`s myself. Would something more like our `PathlibPathChecker` work:

https://github.com/astral-sh/ruff/blob/147f7a7f28c2c23ae5c749b7b57cadfc4a776ec3/crates/ruff_python_semantic/src/analyze/typing.rs#L914-L917

or did you run into cases where we weren't handling unions correctly? (That might be an issue with our other type checkers if so)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_http_call_httpx.rs`:93 on 2025-08-27 13:12_

This looks great! My one tiny nit is to make this a sequence of early returns:


```suggestion
    if !semantic.in_async_context() {
        return;
    }
    
    let Some(ast::ExprAttribute { value, attr, .. }) = call.func.as_attribute_expr() else {
        return;
    }
    
    let Some(name) = value.as_name_expr() else {
        return;
    }
```

That's a bit more verbose here, but I think that's our general style.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC212.py`:61 on 2025-08-27 13:21_

I think this _should_ work. I played around a bit with the PTH210 rule, which uses the `PathlibPathChecker`, and it will detect both module-level and function-level bindings like this:

```py
def foo():
    path: Path = ...
    path.with_suffix(".")
```

I'm wondering if the union traversal might be disrupting this check.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC212_ASYNC212.py.snap`:137 on 2025-08-27 13:29_

Ah okay, this doesn't work for PTH210. I understand the `traverse_union` call now. I'd probably lean toward not traversing unions and matching our other type inference calls, but I don't feel too strongly if you think this is an important use case. We could also consider making a more general change to our `TypeChecker` code if this would always be helpful.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_http_call_httpx.rs`:73 on 2025-08-27 13:35_

I saw below that you do need this check. If I'm following the `traverse_union_options` code correctly, I think it's actually not "traversing" the top-level expression:

https://github.com/astral-sh/ruff/blob/147f7a7f28c2c23ae5c749b7b57cadfc4a776ec3/crates/ruff_python_semantic/src/analyze/typing.rs#L528-L531

which would explain the basic annotation check not working. You might just need to call your closure on `annotation` itself before passing it to `traverse_union_and_optional` to cover both cases.

---

_@ntBre approved on 2025-08-27 13:38_

This looks great, thank you! I just had a couple of small suggestions and some ideas about the union handling that might help with the simple annotation case.

---

_@amyreese reviewed on 2025-08-27 19:08_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC212_ASYNC212.py.snap`:137 on 2025-08-27 19:08_

I was doing this mostly to match behavior with the upstream ASYNC212 implementation, which explicitly includes these unions/optionals in its test cases.

---

_@ntBre reviewed on 2025-08-27 19:18_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC212_ASYNC212.py.snap`:137 on 2025-08-27 19:18_

That makes sense! If we can handle both this and the regular annotation case, it's great to handle both. We can punt on trying to update any other `TypeChecker`s for now too.

---

_@ntBre approved on 2025-08-27 21:22_

Nice, thanks!

Would you mind updating the title to:

[`flake8-async`] Implement `blocking-http-call-httpx` (`ASYNC212`)

(just wrapping the linter name and rule code in backticks) 

We pull the PR titles for changelog entries, and that's how we usually stylize them.

Other than that, feel free to merge whenever you're ready!

---

_Label `preview` added by @ntBre on 2025-08-27 21:22_

---

_Renamed from "[flake8-async] Implement `blocking-http-call-httpx` (ASYNC212)" to "[`flake8-async`] Implement `blocking-http-call-httpx` (`ASYNC212`)" by @amyreese on 2025-08-27 22:18_

---

_Merged by @amyreese on 2025-08-27 22:19_

---

_Closed by @amyreese on 2025-08-27 22:19_

---

_Branch deleted on 2025-08-28 16:56_

---
