```yaml
number: 9202
title: "Replace `String` with `SmolStr` for nodes `Identifier` and `ExprName`"
type: pull_request
state: closed
author: LaBatata101
labels:
  - performance
  - parser
assignees: []
base: main
head: smolstr-id
created_at: 2023-12-19T21:11:48Z
updated_at: 2025-05-08T21:29:16Z
url: https://github.com/astral-sh/ruff/pull/9202
synced_at: 2026-01-12T15:55:28Z
```

# Replace `String` with `SmolStr` for nodes `Identifier` and `ExprName`

---

_@LaBatata101_

## Summary

Changes the type of the `id` field in the `Identifier` and `ExprName` nodes from `String` to `SmolStr`.  This is a required change for the new parser.

This PR is part of #9152 

## Test Plan
`cargo test --lib`

---

_Label `performance` added by @charliermarsh on 2023-12-19 21:12_

---

_Label `parser` added by @charliermarsh on 2023-12-19 21:12_

---

_Comment by @github-actions[bot] on 2023-12-19 21:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:750 on 2023-12-19 23:07_

This is unrelated to your change and doesn't need to be fixed as part of this PR: @charliermarsh should we use `HashSet`s instead of `Vec` for these sets (convert from `Vec` to `Set` when converting from `Options` to `Settings`)? Or is the reason we don't do this because we only use `Options` for plugin specific configurations.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/no_method_decorator.rs`:134 on 2023-12-19 23:18_

GitHub doesn't allow me to make a suggestion on the entire code. We can avoid calling `to_string` here by changing `explicit_decorator_calls` to `HashMap<&str, TextRange>`. 

The `id.to_string` call on linne 128 also seems unnecessary (we can use the `SmolStr` directly or use `as_str`)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1743 on 2023-12-19 23:35_

How did you decide where to use `SmolStr`? Should `ExprIpyEscapeCommand`, `StringLiteral`, and `FStringElement` use `SmolStr` too or is it intentional that they do not because they're less likely to be short?

---

_@MichaReiser reviewed on 2023-12-19 23:39_

Thank you. 

The code changes look good to me. The two main questions that I have are:

* How did you decide on `smol_str` over any of the other small string crates? For example, [`compact_str`](https://crates.io/crates/compact_str) supports up to 24 bytes and 12 bytes on 32-bit architectures. But I'm unfamiliar with all the crates, so there might be other considerations. The intention isn't that you change the small string crate, but that we document how we made the decision in case we want to re-visit it in the future.
* How did you decide on where to use `SmolStr`? E.g. you use it for Names and Identifiers but not for String literals.

---

_@MichaReiser approved on 2023-12-19 23:47_

---

_@charliermarsh reviewed on 2023-12-20 00:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:750 on 2023-12-20 00:21_

@MichaReiser -- We could change to `Set`, I don't think there's a specific reason for it. But one problem is that if we _do_ use`HashSet<String>`, then we would need to allocate here to convert from `SmolStr` to `String` for the `contains` check, right?

---

_@MichaReiser reviewed on 2023-12-20 01:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:750 on 2023-12-20 01:02_

Hmm yeah that might be, because `contains` takes `&T` and not `Deref<T>`. Except if we use `SmolStr` everywhere...

---

_@charliermarsh reviewed on 2023-12-20 01:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:750 on 2023-12-20 01:12_

That is true... I can try it separately.

---

_@MichaReiser reviewed on 2023-12-20 04:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:750 on 2023-12-20 04:01_

I should not have brought this up. It's probably not worth it, considering that these vectors only contain very few items and hashing might be more expensive than just comparing the length of every string first.

---

_@charliermarsh reviewed on 2023-12-20 04:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:750 on 2023-12-20 04:04_

I think that's why I left them as vectors, because the size of these is typically very small.

---

_Comment by @LaBatata101 on 2023-12-20 20:58_

> How did you decide on `smol_str` over any of the other small string crates? For example, [`compact_str`](https://crates.io/crates/compact_str) supports up to 24 bytes and 12 bytes on 32-bit architectures. But I'm unfamiliar with all the crates, so there might be other considerations. The intention isn't that you change the small string crate, but that we document how we made the decision in case we want to re-visit it in the future.

I haven't investigated much about the different string crates, I found out about `smol_str` when l was looking into rust-analyzer parser source code. I tested in the new parser to see if it had any performance improvement, and it gave a slight performance increase IIRC, that was the reason I chose it 🤣. But it's probably a good idea to do a proper comparison between different string crates in the future.

> How did you decide on where to use `SmolStr`? E.g. you use it for Names and Identifiers but not for String literals.

Since `SmolStr` allocates strings up to 23 bytes long on the stack, and strings in Python are also used for documentation, they can get quite large, so I didn't see much benefit from using `SmolStr` for string literals. However, for Names and Identifiers, which are usually small, and are a common token in the code, using `SmolStr` for them will, probably, save quite a lot of allocations.

---

_@charliermarsh approved on 2023-12-20 20:59_

---

_Comment by @charliermarsh on 2023-12-20 20:59_

Thank for separating this out!

---

_@LaBatata101 reviewed on 2023-12-20 21:06_

---

_Review comment by @LaBatata101 on `crates/ruff_python_ast/src/nodes.rs`:1743 on 2023-12-20 21:06_

I already answered most of this question in another comment, but I didn't mention `ExprIpyEscapeCommand`. I haven't seen many examples of real world use of `ExprIpyEscapeCommand` so I don't know if they are usually short, like `Names` and `Identifiers`. Can't really tell if they are a suitable candidate to use `SmolStr`. 

---

_Comment by @charliermarsh on 2023-12-20 21:08_

Do you mind either merging `main` (there's a type error against new changes), or giving me edit privileges on this branch to clean up prior to merging?

---

_Comment by @LaBatata101 on 2023-12-20 21:21_

> Do you mind either merging `main` (there's a type error against new changes), or giving me edit privileges on this branch to clean up prior to merging?

I think you already have edit privileges on this branch

---

_Comment by @charliermarsh on 2023-12-20 21:26_

Ah right, thank you, it was an HTTPS vs. SSH thing.

---

_Comment by @BurntSushi on 2023-12-20 22:17_

It looks like there is almost [no difference](https://codspeed.io/astral-sh/ruff/branches/LaBatata101:smolstr-id) in the micro-benchmarks? Is this change still worth doing?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/soft_keywords.rs`:205 on 2023-12-20 23:50_

```suggestion
    Tok::Name { name: ComactString::new_inline(name) }
```

---

_@MichaReiser reviewed on 2023-12-20 23:51_

---

_Comment by @MichaReiser on 2023-12-20 23:53_

> It looks like there is almost [no difference](https://codspeed.io/astral-sh/ruff/branches/LaBatata101:smolstr-id) in the micro-benchmarks? Is this change still worth doing?

That's the observation I made as well when I did this change a while back https://github.com/astral-sh/ruff/pull/6174

I wouldn't expect any changes for all benchmarks other than the lexer because our (old) parser allocates way too much, so any other improvements are lost in the noise. 

I'm surprised that the lexer benchmarks are regressing. But the microbenchmarks might just be too short and exercise the exact same memory pattern for each run, allowing the allocator to re-use the allocations cheaply. 

Or the change indeed is not as impactful as we thought.

---

_Comment by @MichaReiser on 2023-12-21 10:38_

Using [`CompactString`](https://github.com/astral-sh/ruff/pull/9229) seems more promising (but the API is more annoying). The main difference to `SmolStr` is that:

* `CompactString::clone` is `O(n)` compared to `SmolStr`'s `O(1)`. Cloning in `O(1)` could be useful but we don't see any significant benefits from it from our existing benchmarks. Maybe we would need to update some downstream code to use `SmolStr` too. 
* `CompactString` has no optimisation to store more than 23 whitespace only characters on the stack. This is an optimisation that we don't need.

---

_Comment by @MichaReiser on 2023-12-22 03:41_

Copying this here from the Discord conversation with @LaBatata101 

> My current thinking on small-string crates:
> 
> * SmolStr shows too little benefits at the moment to justify adding a new dependency. That's why I wouldn't merge the PR just yet. We can revisit after switching the parser to see if it brings greater benefits (I'm doubtful but performance is hard to predict)
> CompactStr: It improves the lexer performance by about 5%, but the speedups don't manifest in parser or linter improvements. I suspect that it is because our current parser is slow, and that it reduces the number of allocations for an Identifier from 2 to one, which is significant but maybe not significant enough. The API is a bit cumbersome when it comes to comparing with  str, which raises the entry barrier for new contributors (applies in general, yet another string type in Rust for them to understand)
> * Hipstr: As @Labatata101 pointed out, it requires adding lifetimes to Token and all Nodes which is too big a refactor for now.
> 
> I'm leaning towards deferring the refactor until we've switched to the new parser to re-evaluate the benefits. 
It's also worth considering alternatives or combining it with other improvements reducing the number of allocations. For example, we could explore using an Arena for the AST. This greatly reduces the number of allocations (and removes the Drop time), with the result that allocating for Names proportionally becomes more significant (and removing it yields greater performance improvements). 
>
> What I haven't analyzed yet is whether using a small string crate improves Ruff's memory consumption. A reduced memory consumption on its own would be justification enough for me to switch to a small string crate.

@LaBatata101 do you want to analyze the memory consumption or should we move forward without the small string optimisation for now (and close our both PRs?)

---

_Comment by @LaBatata101 on 2023-12-23 20:48_

> do you want to analyze the memory consumption or should we move forward without the small string optimisation for now (and close our both PRs?)

I think it's better to close both of our PRs for now, and do an in-depth analysis after the new parser is merged.

---

_Closed by @LaBatata101 on 2023-12-23 20:48_

---

_Branch deleted on 2025-05-08 21:29_

---
