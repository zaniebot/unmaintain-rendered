```yaml
number: 18648
title: "[ty] Allow overriding rules for specific files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/overrides
created_at: 2025-06-12T14:25:07Z
updated_at: 2025-06-15T13:27:40Z
url: https://github.com/astral-sh/ruff/pull/18648
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Allow overriding rules for specific files

---

_Pull request opened by @MichaReiser on 2025-06-12 14:25_

## Summary

This PR adds a new `[[overrides]]` configuration section that allows users to toggle rules at a file/path level.

```toml
[tool.ty.rules]
division-by-zero = "error"

# First override: all test files
[[tool.ty.overrides]]
include = ["tests/**"]

[tool.ty.overrides.rules]
division-by-zero = "warn"

# Second override: specific test file (takes precedence)
[[tool.ty.overrides]]
include = ["tests/important.py"]

[tool.ty.overrides.rules]
division-by-zero = "ignore"
```

A file can match zero or many overrides sections. Later overrides take precedence over earlier overrides. Overrides inherit from the global configuration. 

I thought long whether we should allow at most one match (always taking the last) or multiple matches. I did some ecosystem research to see how `per-file-ignores` is used. The most common pattern that I've found is too disable some rules for a sub directory and within that subdirectory, disable more rules for specific files. From airflow:

```toml
"src/tests_common/*" = ["S101", "TRY002"]
# Test compat imports banned imports to allow testing against older airflow versions
"src/tests_common/test_utils/compat.py" = ["TID251", "F401"]
"src/tests_common/pytest_plugin.py" = ["F811"]
```

This is why I ultimately decided to allow multiple matches. This complicates the implementation a bit but not too much. The main downside is that it will make it harder to warn if combining two overrides results in incompatible options because the validation happens lazily. 

The other downside is that it there's no real way override an existing match other than overriding every single. E.g. when the user configuration has an `overrides` that a project wants to override. I think that this is less common and we could consider introducing an option that would mark an `overrides` as the "root", so that ty doesn't apply any more overrides.


Closes https://github.com/astral-sh/ty/issues/178

### Glob syntax

I decided to use the same `include` and `exclude` syntax as we use at the `src` level. Technically, a single `include` option that allows both positive and negative patterns would be enough (and negative `exclude`s doesn't really make sense). However, I opted for the `src` syntax because I think a familiar syntax is important: It woudl be confusing if we allow different glob patterns in different `include` fields.


### Naming

I went with `overrides`, similar to what mypy uses (although mypy filters on modules not paths). I think the name is fine but `overrides` are something entirely different in uv. 

I'm curious to hear more opinions. Alternatives are:

* `files` 
* `sub_configurations` 
* ...? 
* 

   



---

_Label `configuration` added by @MichaReiser on 2025-06-12 14:25_

---

_Label `ty` added by @MichaReiser on 2025-06-12 14:25_

---

_Comment by @github-actions[bot] on 2025-06-12 14:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-12 14:44_

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

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:601 on 2025-06-13 09:18_

I only moved this code

---

_@MichaReiser reviewed on 2025-06-13 09:18_

---

_@MichaReiser reviewed on 2025-06-13 09:18_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:732 on 2025-06-13 09:18_

This is new

---

_Review requested from @BurntSushi by @MichaReiser on 2025-06-13 11:11_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-06-13 11:11_

---

_Marked ready for review by @MichaReiser on 2025-06-13 11:11_

---

_Review requested from @carljm by @MichaReiser on 2025-06-13 11:11_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-13 11:11_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-13 11:11_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-13 11:11_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:675 on 2025-06-13 14:08_

```suggestion
    /// An `include` setting without any globs won't match any files. This is probably a mistake and
```

---

_Review comment by @BurntSushi on `crates/ruff_macros/src/rust_doc.rs`:51 on 2025-06-13 14:13_

I think you can just do `path_idents.eq(path)` here? Via [`Iterator::eq`](https://doc.rust-lang.org/nightly/core/iter/trait.Iterator.html#method.eq).

---

_Review comment by @BurntSushi on `crates/ty/docs/configuration.md`:348 on 2025-06-13 14:30_

```suggestion
`exclude` takes precedence over `include`.
```

---

_Review comment by @BurntSushi on `crates/ty/docs/configuration.md`:346 on 2025-06-13 14:32_

I think this part is in contradiction with the third bullet point above.

Also, assuming this part is right and the bullet above is wrong, can you say more about why `include` and `exclude` are different in this regard? I feel like they should be treated the same?

---

_Review comment by @BurntSushi on `crates/ty/src/args.rs`:211 on 2025-06-13 15:03_

RIP eta reductions

---

_Review comment by @BurntSushi on `crates/ty_project/src/metadata/options.rs`:865 on 2025-06-13 15:11_

```suggestion
            "Please open an issue on the ty repository and share the patterns that caused the error.",
```

---

_Review comment by @BurntSushi on `crates/ty_project/src/metadata/options.rs`:791 on 2025-06-13 15:11_

```suggestion
            "Please open an issue on the ty repository and share the patterns that caused the error.",
```

---

_@BurntSushi approved on 2025-06-13 15:13_

I think this looks great! I feel like the semantics here are very intuitive and also very flexible. I also agree with using two lists to make for a consistent UX.

---

_@MichaReiser reviewed on 2025-06-13 15:47_

---

_Review comment by @MichaReiser on `crates/ty/docs/configuration.md`:346 on 2025-06-13 15:47_

Only explaining why they're different. Anchoring `include` paths has the advantage that we can skip directories that we know can't match any pattern. However, this only works for as long as there's no pattern starting with a wildcard pattern like `**/src`. 

The same problem doesn't exist with `exclude`.

Given that most users probably mean `./src` when writing source in an `include` list and the performance optimization that anchoring allows us to do, this seemed the better default.

---

_@BurntSushi reviewed on 2025-06-13 16:00_

---

_Review comment by @BurntSushi on `crates/ty/docs/configuration.md`:346 on 2025-06-13 16:00_

Yeah hmmm. I guess that does mean users have to write `**/*.pyi` for an include rule when they want different rules for, say, stub files. I feel like most users are going to expect that `*.pyi` should work on its own. But I agree that this semantic implies there is some unavoidable costs imposed by more traversals. Because of that, I think I like what you settled on. But it might be worth highlighting in the docs somewhere that, e.g., `*.pyi` (or some other example) might not do what you expect.

The other thing I might suggest is if we can make `exclude` consistent with `include`? I realize that it isn't needed as an optimization, but maybe a consistent semantic across `include` and `exclude` is less confusing? If users use `*.pyi` to exclude stub files _anywhere_, then they might expect `*.pyi` to work in `include` for stub files anywhere.

---

_@MichaReiser reviewed on 2025-06-13 16:39_

---

_Review comment by @MichaReiser on `crates/ty/docs/configuration.md`:346 on 2025-06-13 16:39_

I sort of like it because it allows you to write `exclude`'s in a more concise way. It also is the same behavior as uv's build backend (with the exception that uv doesn't anchor any exclude patterns, whereas we follow the gitignore semantic where any path with a `/` is anchored)

I do acknowledge that the `*.pyi` example might be somewhat suprising but not more than `src`.

---

_@BurntSushi reviewed on 2025-06-13 16:47_

---

_Review comment by @BurntSushi on `crates/ty/docs/configuration.md`:346 on 2025-06-13 16:47_

Yeah I guess I just think the consistency here we'll lead to less overall confusion, even at the expense of concision.

But I think reasonable people can disagree here and I'm happy to defer to you. :-)

---

_@MichaReiser reviewed on 2025-06-13 16:59_

---

_Review comment by @MichaReiser on `crates/ty/docs/configuration.md`:346 on 2025-06-13 16:59_

Let's hope some more folks chime in :)

---

_@carljm reviewed on 2025-06-13 22:45_

---

_Review comment by @carljm on `crates/ty/docs/configuration.md`:346 on 2025-06-13 22:45_

I don't have a need to weigh in on this, but since it was requested, my preference would be for consistency over concision. I think it's confusing if `include` and `exclude` work differently, and this will trip people up. I really don't think it's a problem if you have to write `**/*.pyi` in your exclude pattern instead of just `*.pyi` -- I think that's better than it working one way in `include` and a different way in `exclude`.

---

_@AlexWaygood reviewed on 2025-06-13 22:57_

---

_Review comment by @AlexWaygood on `crates/ty/docs/configuration.md`:346 on 2025-06-13 22:57_

I also agree with Carl and Andrew on this one!

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-14 07:39_

---

_@MichaReiser reviewed on 2025-06-15 06:15_

---

_Review comment by @MichaReiser on `crates/ty/docs/configuration.md`:346 on 2025-06-15 06:15_

Alright, I'll change this in a separate PR because this behavior existed before this PR (it only surfaced in this PR because I fixed the docs)

---

_Merged by @MichaReiser on 2025-06-15 13:27_

---

_Closed by @MichaReiser on 2025-06-15 13:27_

---

_Branch deleted on 2025-06-15 13:27_

---
