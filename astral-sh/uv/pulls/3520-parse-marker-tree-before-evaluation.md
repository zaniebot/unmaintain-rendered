```yaml
number: 3520
title: Parse marker tree before evaluation
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: marker-tree-parsing
created_at: 2024-05-10T22:05:08Z
updated_at: 2024-05-14T15:02:58Z
url: https://github.com/astral-sh/uv/pull/3520
synced_at: 2026-01-10T14:37:54Z
```

# Parse marker tree before evaluation

---

_Pull request opened by @ibraheemdev on 2024-05-10 22:05_

## Summary

Parse `MarkerTree` expressions upfront, instead of lazily during evaluation.

This makes implementing https://github.com/astral-sh/uv/issues/3355 a lot easier.

---

_Renamed from "parse marker tree before evaluation" to "Parse marker tree before evaluation" by @ibraheemdev on 2024-05-13 19:17_

---

_Marked ready for review by @ibraheemdev on 2024-05-13 19:34_

---

_Review requested from @konstin by @ibraheemdev on 2024-05-13 19:34_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-05-13 19:34_

---

_Comment by @codspeed-hq[bot] on 2024-05-13 19:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:marker-tree-parsing)

### Merging #3520 will **not alter performance**

<sub>Comparing <code>ibraheemdev:marker-tree-parsing</code> (00c900e) with <code>main</code> (732410f)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker.rs`:912 on 2024-05-13 19:36_

Can anyone think of a better way to represent these inverted variants?

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker.rs`:931 on 2024-05-13 19:38_

Do we need an inverted variant here? Without it the parser becomes lossy and won't preserve the ordering after parsing (which might be useful for reconstructing the input using `Display`), but for our use case it's unnecessary.

---

_@ibraheemdev reviewed on 2024-05-13 19:38_

---

_Comment by @charliermarsh on 2024-05-13 23:32_

@ibraheemdev - do you need help debugging the regression?

---

_Comment by @ibraheemdev on 2024-05-14 01:08_

@charliermarsh I think it's that the `Display` implementation was accidentally changed to not have quotes around some marker values, so the items are not being found in cache. I'll fix that and add tests.

---

_Comment by @charliermarsh on 2024-05-14 01:16_

I don't think markers should affect caching, but maybe the evaluation is changing and so we're including more deps?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker.rs`:931 on 2024-05-14 10:45_

Fine by me

---

_@konstin reviewed on 2024-05-14 10:45_

---

_Review comment by @konstin on `crates/pep508-rs/src/marker.rs`:937 on 2024-05-14 10:46_

Aren't they always false?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker.rs`:1168 on 2024-05-14 10:58_

Needs manual indent (rustfmt gave up because the string is too long)

---

_Review comment by @konstin on `crates/pep508-rs/src/marker.rs`:1220 on 2024-05-14 10:59_

Needs manual indent

---

_Review comment by @konstin on `crates/pep508-rs/src/marker.rs`:1295 on 2024-05-14 11:04_

The option thing feels odd (why are we evaluating markers against an environment when there is no environment?), can you do a `let Some(env) = env else { return false; }` or something like it?

---

_@konstin approved on 2024-05-14 11:12_

I like this!

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker.rs`:912 on 2024-05-14 11:55_

I like this personally. There's a fundamental asymmetry here, so this representation makes sense to me I think.

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker.rs`:1168 on 2024-05-14 11:57_

I would split the string across multiple lines. It's a win-win: rustfmt will format it and you won't have a long line that is harder to read when your editor auto-wraps it. :-)

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker.rs`:1295 on 2024-05-14 12:01_

I added the `Option` in an older PR. If there's no environment, we specifically want all environment marker expressions to evaluate to `true`. But we still evaluate marker expressions involving extras.

This has the effect of evaluating a marker expression in an environment independent way. But since you still need to evaluate extras, you can't just do `return true` when `env` is `None` at the top here. But maybe it could be put at the top of some of the branches to simplify things a little.

It's an odd thing about PEP 508. Everything about marker expressions is tied to the environment _except_ for extras.

---

_@BurntSushi approved on 2024-05-14 12:04_

LGTM assuming we track down the regression. Nice refactor.

---

_@konstin reviewed on 2024-05-14 14:37_

---

_Review comment by @konstin on `crates/pep508-rs/src/marker.rs`:1295 on 2024-05-14 14:37_

This is a discussion beyond this PR, but i've been thinking about turning `MarkerEnvironment` from a dataclass into something more general, e.g. a trait where you can query a value or an expression in and the env can decides which fraction of the fields to evaluate. This would unify the cases for python version, markers and full environment behind a shared interface.

---

_@ibraheemdev reviewed on 2024-05-14 14:38_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker.rs`:937 on 2024-05-14 14:38_

Yeah, updated the docs.

---

_Comment by @ibraheemdev on 2024-05-14 15:02_

Looks like fixing the `Display` implementation fixed the regression, not exactly sure why.

---

_Merged by @ibraheemdev on 2024-05-14 15:02_

---

_Closed by @ibraheemdev on 2024-05-14 15:02_

---

_@BurntSushi reviewed on 2024-05-14 15:02_

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker.rs`:1295 on 2024-05-14 15:02_

I was thinking about something like that too, but the full shape of what it should look like wasn't totally clear to me. What was driving that thought is that I'm not a fan of spreading `Option<&MarkerEnvironment>` everywhere, and I would rather encapsulate that case analysis instead of pushing it out to everyone else.

So... yeah I'm on board with your idea. :-)

---
