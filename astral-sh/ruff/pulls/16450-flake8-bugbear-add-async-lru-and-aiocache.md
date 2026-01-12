```yaml
number: 16450
title: "[flake8-bugbear] Add async-lru and aiocache decorators to the B019 rule checker"
type: pull_request
state: open
author: LuckySting
labels:
  - breaking
  - needs-decision
assignees: []
base: main
head: extend-flake8-bugbear-with-async-decorators
created_at: 2025-03-01T16:03:00Z
updated_at: 2025-03-04T10:35:46Z
url: https://github.com/astral-sh/ruff/pull/16450
synced_at: 2026-01-12T15:55:55Z
```

# [flake8-bugbear] Add async-lru and aiocache decorators to the B019 rule checker

---

_@LuckySting_

## Summary

Resolves #16436.

This change adds support for checking the `B019` rule against async cache decorators from the two most commonly used third-party libraries ([aiocache](https://github.com/aio-libs/aiocache) and [async-lru](https://github.com/aio-libs/async-lru)). It also updates the error message to clearly indicate which decorator was used.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-03-01 16:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `breaking` added by @MichaReiser on 2025-03-03 10:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/cached_instance_method.rs`:69 on 2025-03-03 10:20_

Nit: I'd suggest using an enum here to avoid unnecessary `String` allocation


```rust
#[derive(Copy, Clone, Deubg)]
enum LruDecorator {
	FuncToolsLruCache,
	FunctoolsCache,
	AsyncLru,
	AiocacheCached,
	AiocacheCachedStampede,
	AiocacheMultiCached
}

impl LruDecorator {
	fn from_qualified_name(name: &[str]) -> Option<Self> {
		match qualified_name.segments() {
            ["functools", "lru_cache"] => Some(Self::FunctoolsLruCache),
						...
            _ => None,
	}
}

impl std::fmt::Display for LruDecorator {
	fn fmt(f: &mut std::fmt::Formatter) -> std::fmt::Result {
		match self {
			Self::FuncToolsLruCache => f.write_str("functools.lru_cache"),
			...
		}
	}
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/cached_instance_method.rs`:168 on 2025-03-03 10:22_

We have to gate this new behavior behind preview mode because it's a considerable extension of the rule's intent (`checker.settings().preview.is_enabled()`)

---

_@MichaReiser reviewed on 2025-03-03 10:27_

Thank you.


I'm a bit hesitant to accept this change because it is for third party libraries. Most of our rules are limited to first-party modules or very popular first-party modules.

I'm not sure `async-lru` meets that bar 

---

_Label `needs-decision` added by @MichaReiser on 2025-03-03 10:27_

---

_Comment by @LuckySting on 2025-03-03 11:09_

> I'm not sure `async-lru` meets that bar

Hello! Here the download statistics for these two:
[async-lru](https://pypistats.org/packages/async-lru) More then 20kk downloads last month
[aiocache](https://pypistats.org/packages/aiocache) About 2kk downloads last month

It seems that `async-lru` just a young lib, which is getting popular, and `aiocache` is old-but-gold one. What do you think, could we include them both now?

---

_@LuckySting reviewed on 2025-03-03 11:51_

---

_Review comment by @LuckySting on `crates/ruff_linter/src/rules/flake8_bugbear/rules/cached_instance_method.rs`:69 on 2025-03-03 11:51_

Oh, really, thanks!
I used to think about allocation

---

_@LuckySting reviewed on 2025-03-03 11:57_

---

_Review comment by @LuckySting on `crates/ruff_linter/src/rules/flake8_bugbear/rules/cached_instance_method.rs`:168 on 2025-03-03 11:57_

I don't understand how to write insta tests in this case, could you explain it to me or give an example?

---

_Comment by @MichaReiser on 2025-03-04 10:35_

@zanieb, do you know how popular/recommended these libraries are in the Python ecosystem? E.g. are they as popular as `attrs`?

---
