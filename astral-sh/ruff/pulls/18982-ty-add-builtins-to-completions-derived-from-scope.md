```yaml
number: 18982
title: "[ty] Add builtins to completions derived from scope"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-builtins
created_at: 2025-06-27T12:30:23Z
updated_at: 2025-07-02T02:00:37Z
url: https://github.com/astral-sh/ruff/pull/18982
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Add builtins to completions derived from scope

---

_Pull request opened by @BurntSushi on 2025-06-27 12:30_

Most of the work here was doing some light refactoring to facilitate
sensible testing. That is, we don't want to list every builtin included
in most tests, so we add some structure to the completion type returned.
Tests can now filter based on whether a completion is a builtin or not.

Otherwise, builtins are found using the existing infrastructure for
`object.attr` completions (where we hard-code the module name
`builtins`).

I did consider changing the sort order based on whether a completion
suggestion was a builtin or not. In particular, it seemed like it might
be a good idea to sort builtins after other scope based completions,
but before the dunder and sunder attributes. Namely, it seems likely
that there is an inverse correlation between the size of a scope and
the likelihood of an item in that scope being used at any given point.
So it *might* be a good idea to prioritize the likelier candidates in
the completions returned.

Additionally, the number of items introduced by adding builtins is quite
large. So I wondered whether mixing them in with everything else would
become too noisy.

However, it's not totally clear to me that this is the right thing to
do. Right now, I feel like there is a very obvious lexicographic
ordering that makes "finding" the right suggestion to activate
potentially easier than if the ranking mechanism is less clear.
(Technically, the dunder and sunder attributes are not sorted
lexicographically, but I'd put forward that most folks don't have an
intuitive understanding of where `_` ranks lexicographically with
respect to "regular" letters. Moreover, since dunder and sunder
attributes are all grouped together, I think the ordering here ends up
being very obvious after even a quick glance.)


---

_Review requested from @carljm by @BurntSushi on 2025-06-27 12:30_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-27 12:30_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-27 12:30_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-27 12:30_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-27 12:30_

---

_Review request for @dcreager removed by @BurntSushi on 2025-06-27 12:30_

---

_Review request for @carljm removed by @BurntSushi on 2025-06-27 12:30_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-06-27 12:30_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-06-27 12:30_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-06-27 12:30_

---

_Comment by @github-actions[bot] on 2025-06-27 12:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-06-27 12:33_

---

_Label `ty` added by @AlexWaygood on 2025-06-27 12:33_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:2261 on 2025-06-27 12:40_

worth renaming this to `completions_sans_builtins`, or similar?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:76 on 2025-06-27 12:46_

nit: we usually use this API for checking whether a module is a specific module from a given search path with a given name

```suggestion
                builtin: module.is_known(KnownModule::Builtins),
```

---

_@AlexWaygood approved on 2025-06-27 12:48_

---

_@BurntSushi reviewed on 2025-06-27 13:08_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:76 on 2025-06-27 13:08_

Oh nice!

---

_@BurntSushi reviewed on 2025-06-27 13:09_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:2261 on 2025-06-27 13:09_

I renamed it to `completions_without_builtins`. Kinda unfortunate since it's used so much, but I think it will make the tests easier to read.

---

_Comment by @codspeed-hq[bot] on 2025-06-27 13:24_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/ag%2Fcompletion-builtins?runnerMode=WallTime)

### Merging #18982 will **not alter performance**

<sub>Comparing <code>ag/completion-builtins</code> (67134b1) with <code>main</code> (a3c79d8)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Merged by @BurntSushi on 2025-06-27 14:20_

---

_Closed by @BurntSushi on 2025-06-27 14:20_

---

_Branch deleted on 2025-06-27 14:20_

---

_Comment by @AlexWaygood on 2025-06-30 11:00_

> I did consider changing the sort order based on whether a completion suggestion was a builtin or not. In particular, it seemed like it might be a good idea to sort builtins after other scope based completions, but before the dunder and sunder attributes. Namely, it seems likely that there is an inverse correlation between the size of a scope and the likelihood of an item in that scope being used at any given point. So it _might_ be a good idea to prioritize the likelier candidates in the completions returned.
> 
> Additionally, the number of items introduced by adding builtins is quite large. So I wondered whether mixing them in with everything else would become too noisy.
> 
> However, it's not totally clear to me that this is the right thing to do. Right now, I feel like there is a very obvious lexicographic ordering that makes "finding" the right suggestion to activate potentially easier than if the ranking mechanism is less clear. (Technically, the dunder and sunder attributes are not sorted lexicographically, but I'd put forward that most folks don't have an intuitive understanding of where `_` ranks lexicographically with respect to "regular" letters. Moreover, since dunder and sunder attributes are all grouped together, I think the ordering here ends up being very obvious after even a quick glance.)

I think I'd be in favour of making this change so that they're sorted below other candidates... in the playground here, it was slightly annoying that `Protocol` was sorted below `PendingDeprecationWarning` in the list of autocompletions. It seems obvious that it's the most likely candidate for what I want, given that I just imported it! 😆

![image](https://github.com/user-attachments/assets/bce2fcbf-c0b7-40a7-adbb-b3521c1747e6)


---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_model.rs`:138 on 2025-07-01 08:26_

nit: Should we use `label` here similar to the one in `lsp_types::CompletionItem`?

---

_@dhruvmanila reviewed on 2025-07-01 08:27_

Looks great!

---

_@BurntSushi reviewed on 2025-07-01 12:19_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:138 on 2025-07-01 12:19_

I actually changed this to `name` since I thought it was clearer by matching the type `Name` that we use internally. I guess I don't feel strongly about it.

---

_Comment by @BurntSushi on 2025-07-01 12:21_

@AlexWaygood I think if we were going to go that route, we'd need to be more thoughtful about our ranking. I'm pretty concerned about having an unpredictable order and that making it difficult for folks to scan the list quickly to find what they're looking for.

---

_Comment by @AlexWaygood on 2025-07-01 12:25_

I definitely get that concern. I think having the likeliest attribute be as close to the top of the list as possible is quite high-value, but I definitely agree that having a consistent and predictable behaviour is also high-value. I'm fine with parking this for now and coming back to it later!

---

_@dhruvmanila reviewed on 2025-07-02 02:00_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_model.rs`:138 on 2025-07-02 02:00_

Yeah, me neither. It's fine to keep it as is.

---
