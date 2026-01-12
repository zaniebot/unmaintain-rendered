```yaml
number: 6702
title: "[refurb] Implement `repeated-append` rule (`FURB113`)"
type: pull_request
state: merged
author: SavchenkoValeriy
labels:
  - rule
assignees: []
merged: true
base: main
head: refurb/furb113
created_at: 2023-08-20T12:35:40Z
updated_at: 2023-08-29T10:41:20Z
url: https://github.com/astral-sh/ruff/pull/6702
synced_at: 2026-01-12T02:45:38Z
```

# [refurb] Implement `repeated-append` rule (`FURB113`)

---

_Pull request opened by @SavchenkoValeriy on 2023-08-20 12:35_

## Summary

As an initial effort with replicating `refurb` rules (#1348 ), this PR adds support for [FURB113](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/list_extend.py) and adds a new category of checks.

## Test Plan

I included a new test + checked that all other tests pass.


---

_Comment by @github-actions[bot] on 2023-08-20 12:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-08-26 21:18_

@SavchenkoValeriy - Thanks for this. Regarding the limitation that we can't perform inference across files (or full static inference in general), I'm still open to merging these kinds of rules, for two reasons...

1. The lack of inference means we'll have false negatives rather than false positives, which is preferable, especially because...
2. These rules are nice-to-haves, and so missing a few is more acceptable than it would be if the rule were aimed at preventing production issues (like detecting undefined variables).

So, I think it's still reasonable to pursue implementing and merging these. It may take me a bit of time to do a proper review though since it is admittedly quite a bit of code.


---

_@charliermarsh reviewed on 2023-08-26 21:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:43 on 2023-08-26 21:23_

Can you take a look at the `SourceCodeSnippet` abstraction and use it here? Otherwise, if the input code is long or breaks over multiple lines, we end up with really long diagnostics. We should support a generic, truncated message here based on the `SourceCodeSnippet` concept.

---

_@charliermarsh reviewed on 2023-08-26 21:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:61 on 2023-08-26 21:24_

Nit: convention is to name these identically to the rule (like `repeated_append`).

---

_@charliermarsh reviewed on 2023-08-26 21:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:120 on 2023-08-26 21:25_

Nit: can we use an empty `Vec` rather than the `Option` state?

---

_@charliermarsh reviewed on 2023-08-26 21:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:138 on 2023-08-26 21:26_

Maybe we can add a method to `traversal` to support iterating over the siblings? We already have `next_sibling`.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:172 on 2023-08-26 21:26_

Hmm, it would be nice if `traversal` did the right thing here... But, it can be a TODO.

---

_@charliermarsh reviewed on 2023-08-26 21:26_

---

_@charliermarsh reviewed on 2023-08-26 21:27_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:160 on 2023-08-26 21:27_

Consider using a mutable iterator over `appends` with `iter.next()` to get the first, then iterating over the remainder with `for appends in iter`? That way, you don't have the implicit coupling between this line and line 164 (and you avoid the unwrap).

---

_@charliermarsh reviewed on 2023-08-26 21:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:251 on 2023-08-26 21:28_

For any methods here that only rely on `checker.semantic()`, I'd prefer to pass in `semantic: &'a SemanticModel` rather than the entire checker.

---

_@charliermarsh reviewed on 2023-08-26 21:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:259 on 2023-08-26 21:28_

Can we add rustdoc for some of these more involved methods? Just to outline its purpose.

---

_@charliermarsh reviewed on 2023-08-26 21:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:221 on 2023-08-26 21:29_

Can you include an example here of the Python code this should catch? (I think I understand but it would be useful to see.)


---

_@SavchenkoValeriy reviewed on 2023-08-26 21:30_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:160 on 2023-08-26 21:30_

Sounds good, will do!

---

_@charliermarsh reviewed on 2023-08-26 21:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:179 on 2023-08-26 21:30_

Nit: wonder if this should be the responsibility of the caller.

---

_@charliermarsh reviewed on 2023-08-26 21:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:286 on 2023-08-26 21:31_

We generally prefer:

```rust
let [binding] = bindings.as_slice() else {
    return;
};
```


---

_@charliermarsh reviewed on 2023-08-26 21:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:263 on 2023-08-26 21:31_

We'd generally prefer:

```rust
let [arg] = arguments.args.as_slice() else {
    return;
}
```

Which lets us avoid the unwrap further down.

---

_@charliermarsh reviewed on 2023-08-26 21:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:303 on 2023-08-26 21:32_

This got folded into a more general trait in a future PR, right?

---

_@charliermarsh reviewed on 2023-08-26 21:32_

Nice work, this is impressive!

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:32_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:179 on 2023-08-26 21:32_

Agree, it creates a better separation

---

_@charliermarsh reviewed on 2023-08-26 21:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:133 on 2023-08-26 21:33_

It would be nice to document what each of these fields means with a Python example, similar to the style of documentation you see on `BindingKind`.

---

_@charliermarsh reviewed on 2023-08-26 21:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:80 on 2023-08-26 21:33_

Consider implementing `Ranged` on `AppendGroup`? Then you could just do `group.start()` and `group.end()`, and make this part of the struct.

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:34_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:221 on 2023-08-26 21:34_

Sure!

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:35_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:259 on 2023-08-26 21:35_

Sure, I just didn't know what's the official policy on private functions and documentation. 

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:251 on 2023-08-26 21:35_

No problem!

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:35_

---

_@charliermarsh reviewed on 2023-08-26 21:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:186 on 2023-08-26 21:36_

It would be nice to find a way to avoid this lookup if the current statement _isn't_ an append, since the iterator at the end will be empty anyway, and I think `traversal::suite` has some cost.

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:37_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:138 on 2023-08-26 21:37_

Do you want me to extend traversal to include the logic about the global scope statements?

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:38_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:120 on 2023-08-26 21:38_

Sure!

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:39_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:61 on 2023-08-26 21:39_

I even thought about it when implementing next checkers 😅 will do!

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:41_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:43 on 2023-08-26 21:41_

I wondered about that, good to know that there is a centralized solution for that. Will do!

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:44_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:133 on 2023-08-26 21:44_

Yeah, I guess this checker is a bit more complex than an average checker, so it totally makes sense to describe these internals. Thanks!

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:44_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:80 on 2023-08-26 21:44_

That's nice 🙂 will do

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:46_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:186 on 2023-08-26 21:46_

I thought about it and didn't figure out an elegant solution to do so. On the other hand, I'm a Rust newbie, so it's very likely that I'm missing it. 

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:50_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:303 on 2023-08-26 21:50_

Yep, the next PR folds it and adds is_dict, and the one after that is_set

---

_@SavchenkoValeriy reviewed on 2023-08-26 21:51_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:263 on 2023-08-26 21:51_

Yep, I started using this pattern only later 😅 thanks!

---

_Comment by @charliermarsh on 2023-08-26 22:07_

@SavchenkoValeriy - Specifically, I am very impressed by how you were able to navigate the semantic model!

---

_@charliermarsh reviewed on 2023-08-26 22:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:186 on 2023-08-26 22:09_

Hmm... We could perhaps do something like:

```rust
// Match the current statement, to see if it's an append.
let append = match_append(checker, stmt)?;

// Then do the rest of the work that this function already does today, and end with...
std::iter::once(append).chain(
    siblings
        .iter()
         // Skip the current statement, since we already tested it...
        .skip(stmt_index + 1)
        .map_while(|next| match_append(checker, next))
).collect()
```

---

_@SavchenkoValeriy reviewed on 2023-08-27 08:17_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:186 on 2023-08-27 08:17_

That's pretty cool! Thanks

---

_@SavchenkoValeriy reviewed on 2023-08-27 10:11_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:138 on 2023-08-27 10:11_

`traversal::suite` already returns siblings, but doesn't handle the situation with the top level statements. Right now traversal is part of AST, but in order to resolve this issue, it needs access to semantics. I can move it to a different crate and make that change. 

It can probably be a separate PR, though. What do you think it should be?

---

_Comment by @codspeed-hq[bot] on 2023-08-27 10:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/SavchenkoValeriy:refurb/furb113)

### Merging #6702 will **not alter performance**

<sub>Comparing <code>SavchenkoValeriy:refurb/furb113</code> (b387921) with <code>main</code> (f33277a)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Label `rule` added by @charliermarsh on 2023-08-28 22:41_

---

_Merged by @charliermarsh on 2023-08-28 22:51_

---

_Closed by @charliermarsh on 2023-08-28 22:51_

---

_@charliermarsh reviewed on 2023-08-28 22:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:165 on 2023-08-28 22:52_

@SavchenkoValeriy - I ended up reverting to `Option`, your initial version was better :joy:

---

_@charliermarsh reviewed on 2023-08-28 22:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:88 on 2023-08-28 22:52_

Writing this as a map felt a bit more idiomatic to me.

---

_Comment by @charliermarsh on 2023-08-28 22:53_

Thanks @SavchenkoValeriy!

---

_@SavchenkoValeriy reviewed on 2023-08-29 10:38_

---

_Review comment by @SavchenkoValeriy on `crates/ruff/src/rules/refurb/rules/repeated_append.rs`:88 on 2023-08-29 10:38_

Looks better, agree!

---

_Branch deleted on 2023-08-29 10:41_

---
