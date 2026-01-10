```yaml
number: 10852
title: "Support negated patterns in [extend-]per-file-ignores"
type: pull_request
state: merged
author: carljm
labels:
  - configuration
assignees: []
merged: true
base: main
head: cjm/select
created_at: 2024-04-10T01:37:25Z
updated_at: 2024-04-10T16:14:11Z
url: https://github.com/astral-sh/ruff/pull/10852
synced_at: 2026-01-10T22:37:01Z
```

# Support negated patterns in [extend-]per-file-ignores

---

_Pull request opened by @carljm on 2024-04-10 01:37_

Fixes #3172 

## Summary

Allow prefixing [extend-]per-file-ignores patterns with `!` to negate the pattern; listed rules / prefixes will be ignored in all files that don't match the pattern.

## Test Plan

Added tests for the feature.

Rendered docs and checked rendered output.

---

_@carljm reviewed on 2024-04-10 01:39_

---

_Review comment by @carljm on `crates/ruff_linter/src/settings/types.rs`:314 on 2024-04-10 01:39_

This might look a little odd, but it's zero-allocation, unlike the version using `.strip_prefix()`.

Perf probably doesn't matter much here in config-parsing; if you'd prefer the version that takes a non-mut String and uses `.strip_prefix()`, I can switch to that, though the code actually ends up longer than this.

---

_@carljm reviewed on 2024-04-10 01:41_

---

_Review comment by @carljm on `crates/ruff_linter/src/settings/types.rs`:606 on 2024-04-10 01:41_

I think this should be a named struct (`CompiledPerFileIgnore`? `PerFileIgnoreMatcher`?) with named fields rather than a tuple, but for ease of review I didn't make that change here; will push it as a separate PR (unless a reviewer suggests I shouldn't.)

---

_Comment by @github-actions[bot] on 2024-04-10 01:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @zanieb on `crates/ruff/tests/lint.rs`:1181 on 2024-04-10 02:06_

FWIW my preference is to use the test rules instead of real rules, they're less likely to change and it avoids mixing concerns — most tests don't use them yet because they're relatively new. If it's a pain this is fine though.

---

_@zanieb reviewed on 2024-04-10 02:06_

---

_@carljm reviewed on 2024-04-10 02:15_

---

_Review comment by @carljm on `crates/ruff/tests/lint.rs`:1181 on 2024-04-10 02:15_

Makes sense, will do. 

---

_@charliermarsh reviewed on 2024-04-10 03:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/types.rs`:314 on 2024-04-10 03:25_

I'm cool with this.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/types.rs`:314 on 2024-04-10 03:26_

\cc @BurntSushi may have interesting things to say here :)

---

_@charliermarsh reviewed on 2024-04-10 03:26_

---

_@charliermarsh reviewed on 2024-04-10 03:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/types.rs`:606 on 2024-04-10 03:26_

Agreed.

---

_@charliermarsh approved on 2024-04-10 03:28_

Looks great.

---

_Label `configuration` added by @charliermarsh on 2024-04-10 03:29_

---

_Merged by @carljm on 2024-04-10 03:53_

---

_Closed by @carljm on 2024-04-10 03:53_

---

_Branch deleted on 2024-04-10 03:53_

---

_@jamesmyatt reviewed on 2024-04-10 08:06_

---

_Review comment by @jamesmyatt on `crates/ruff_workspace/src/options.rs`:918 on 2024-04-10 08:06_

Is this comment correct? Looks like it ignored F401 not D

---

_Review comment by @BurntSushi on `crates/ruff/tests/lint.rs`:1250 on 2024-04-10 11:32_

It might be worth adding some more tests where there are both regular patterns and negated patterns. And in particular, cases where a negated pattern might try to "override" a previous pattern. e.g.,

```
"*.py" = ["RUF"]
"!foo.py" = ["RUF"]
```

Or something like that.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/settings/types.rs`:314 on 2024-04-10 11:35_

I think this is fine personally. I agree that saving an alloc probably doesn't matter here (I assume these ultimately get converted to a glob matcher, and that is an enormous amount of work compared to saving an alloc) so I'd write whatever is clear. And this seems clear to me. :)

---

_@BurntSushi reviewed on 2024-04-10 11:36_

Nice!

---

_@charliermarsh reviewed on 2024-04-10 15:10_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:918 on 2024-04-10 15:10_

Ah yeah, it should be `D` below (or the comment should change to `F401`) \cc @carljm

---

_@carljm reviewed on 2024-04-10 16:05_

---

_Review comment by @carljm on `crates/ruff_workspace/src/options.rs`:918 on 2024-04-10 16:05_

Oops, thanks, will fix!

---

_@carljm reviewed on 2024-04-10 16:14_

---

_Review comment by @carljm on `crates/ruff/tests/lint.rs`:1250 on 2024-04-10 16:14_

Yeah, that's a great point, I'll put up an update with a test for this case, and probably also more clearly document how this works.

(I don't expect to change the implementation as part of this; I think the way it needs to work is how it does now: we go through each pattern, and if the pattern matches (whether that's a positive match or a negated non-match), we add the listed rules as ignored; we never un-ignore rules based on failure to match a pattern. I think that would get too complex to understand pretty quickly, and also un-ignoring doesn't really make sense for an option named `per-file-ignores`.)

---
