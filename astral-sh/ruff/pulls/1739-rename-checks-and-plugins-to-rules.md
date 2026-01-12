```yaml
number: 1739
title: "Rename `checks` and `plugins` to `rules`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rules
created_at: 2023-01-08T23:20:22Z
updated_at: 2023-01-09T06:39:52Z
url: https://github.com/astral-sh/ruff/pull/1739
synced_at: 2026-01-12T05:36:32Z
```

# Rename `checks` and `plugins` to `rules`

---

_Pull request opened by @charliermarsh on 2023-01-08 23:20_

Side note: I think it's useful to maintain the mutating vs. pure distinction in the names, especially given that we've already done the work of separating them.


---

_Comment by @charliermarsh on 2023-01-08 23:20_

\cc @not-my-profile 

---

_Comment by @charliermarsh on 2023-01-08 23:20_

The last major piece is to update the documentation (README and contributing guide), which I'll do last.

---

_Comment by @not-my-profile on 2023-01-09 03:20_

As per #1547 I'd rather have `ruff::rules::<origin>::<rule>` than `ruff::<origin>::rules(_mut)`. So for example I'd prefer `fn line_too_long` to be defined in `src/rules/pycodestyle/line_too_long.rs`.

I don't really see the point of maintaining that distinction in the names.

> especially given that we've already done the work of separating them

The previous terminology "checks" and "plugins" was so confusing that at least I didn't understand that convention and mistakenly placed `banned_attribute_access` in `flake8_tidy_imports/checks.rs`. To be honest I don't think it makes sense to split up the various functions implementing TID251 across different files just because they have different signatures. I believe grouping functions by topic to be more useful and intuitive.

---

_Comment by @charliermarsh on 2023-01-09 03:37_

> As per https://github.com/charliermarsh/ruff/issues/1547 I'd rather have ruff::rules::<origin>::<rule> than ruff::<origin>::rules(_mut). So for example I'd prefer fn line_too_long to be defined in src/rules/pycodestyle/line_too_long.rs

Honestly, I'm unlikely to do this right now. It's useful to have all the functionality for one plugin defined in a single module. If we split the code in that way, then we probably need to make the same change for `settings.rs`, etc. The current system would even allows us to put each plugin in its own crate.

Separately, I'm unlikely to enforce the requirement that every rule go in its own file. It's just convenient in practice to put multiple small checks in the same file sometimes. And there are a few instances where it's necessary for multiple checks to share an implementation (i.e., if there's shared work, like in detecting duplicate dictionary keys). Maybe we do this eventually, but I don't think it's worth prioritizing right now.

> The previous terminology "checks" and "plugins" was so confusing that at least I didn't understand that convention and mistakenly placed banned_attribute_access in flake8_tidy_imports/checks.rs

Right, but we're changing the terminology!

> To be honest I don't think it makes sense to split up the various functions implementing TID251 across different files just because they have different signatures. I believe grouping functions by topic to be more useful and intuitive.

I get this, but it's not just that they have different signatures. It's a fundamentally different API. I think it's useful to know which API a rule is adhering to when you call it. I'm willing to be convinced that this is incorrect, and that we should just drop the distinction.


---

_Comment by @charliermarsh on 2023-01-09 05:55_

> To be honest I don't think it makes sense to split up the various functions implementing TID251 across different files just because they have different signatures. I believe grouping functions by topic to be more useful and intuitive.

I've reflected on this a bit more, and I'm leaning towards dropping the `rules` vs. `rules_mut` distinction. The `_mut` naming convention typically kicks in when you have mutable and non-mutable variants of the same function, which isn't the case here. I think part of my inertia comes from the fact that we already have this distinction, and it's easier to undo it than to reintroduce it. But it's probably not worth the confusion that you're referencing.

---

_Renamed from "Rename `checks` and `plugins` to `rules` and `rules_mut`" to "Rename `checks` and `plugins` to `rules`" by @charliermarsh on 2023-01-09 06:37_

---

_Merged by @charliermarsh on 2023-01-09 06:39_

---

_Closed by @charliermarsh on 2023-01-09 06:39_

---

_Branch deleted on 2023-01-09 06:39_

---
