```yaml
number: 2289
title: Remove conflicting D203 and D213 pydocstyle rules
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/deprecate
created_at: 2023-01-28T03:48:58Z
updated_at: 2023-02-01T15:10:41Z
url: https://github.com/astral-sh/ruff/pull/2289
synced_at: 2026-01-12T15:55:07Z
```

# Remove conflicting D203 and D213 pydocstyle rules

---

_@charliermarsh_

These rules both have conflicting variants (`D203` conflicts with `D211`, and `D213` conflicts with `D212`). The most common error I see in Ruff is users hitting an infinite autofix iteration loop due to having pairs of these rules enabled (e.g., Ruff goes back-and-forth, adding and removing a newline between the docstring and the `class` definition, until you hit 100 iterations). We display a warning, but... it's just a bad experience, and it comes up way too often.

These are the only two codes that are included in none of the built-in conventions, and it seems like `D203` was actually removed from PEP 257 anyway (at least according to [this](https://opendev.org/openstack/keystoneauth/commit/cf520d9ca45dfbb09ca9bab90fa3d615bc950326)). I did some extensive searching via GitHub Code Search, and I can only ever find these codes referenced in `ignore` lists. My guess is that there isn't a single Ruff user that has these enabled, at least not intentionally.

Here are some examples of this coming up:

- https://github.com/scipy/scipy/pull/17812#issuecomment-1406908750
- https://github.com/charliermarsh/ruff/issues/2281
- https://github.com/charliermarsh/ruff/issues/1974
- https://github.com/charliermarsh/ruff/issues/1575

As an alternative, I considered:

1. Set `convention = "pep257"` by default. This would have the effect of disabling the incompatible rules. But, it's a breaking change, and doesn't seem worth it to me.
2. Force-disable those rules if they're both set via `D` or `ALL` (but not via `D203` or `D213`). That would work, but it adds complexity, and it strikes me as a waste of time.


---

_Comment by @charliermarsh on 2023-01-28 03:49_

@not-my-profile - Do you have an opinion on the cleanest way to log a warning when the user has one of these codes in their `pyproject.toml` (but not throw a hard error)? They're in a lot of `ignore` lists, and I don't want it to cause a hard failure for now.

---

_Comment by @not-my-profile on 2023-01-28 04:13_

Firstly I am confused that the PR & commit title says "Deprecate" when it in fact removes those rules? I think deprecation is what you do before removal.^^

> Set `convention = "pep257"` by default.

I think this would actually be a sensible change.

> But, it's a breaking change, and doesn't seem worth it to me.

We could make it non-breaking by using the Rust `edition` trick I mentioned earlier.

I think the bigger problem of course is that our default rule set (E & F) doesn't include most of our rules but selecting `ALL` selects several rules that are just annoying.

---

_Renamed from "Deprecate conflicting D203 and D213 pydocstyle rules" to "Remove conflicting D203 and D213 pydocstyle rules" by @charliermarsh on 2023-01-28 04:16_

---

_Comment by @charliermarsh on 2023-01-28 04:18_

Sorry, yeah, I just want to remove these.

Honestly I'd rather just remove them than accommodate them in any form, to avoid confusion. But I'll sleep on it. Maybe I'm missing something.


---

_Comment by @not-my-profile on 2023-01-28 04:19_

So yes I think the big problem we should actually address is that we don't have a "default ruleset" that contains all rules that are generally desirable.

I think this could be solved via *rule categorization* (#1774), by introducing a category "experimental" (like clippy's "nursery") and categorizing all problematic rules as "experimental"/"nursery" and letting users simply select all other categories.

---

_Comment by @charliermarsh on 2023-01-28 04:20_

I agree, but I just want to fix this specific case ASAP since it keeps coming up. And even with categorization, we'd run into the problem posed by these specific, conflicting rules: `D212` and `D213` would fit into the same category, but would conflict!

---

_Comment by @charliermarsh on 2023-01-28 04:21_

Maybe setting `convention = "pep257"` will in fact fix this for most people.

---

_Comment by @charliermarsh on 2023-01-28 04:21_

The other thing we could do is remove these from `D` and `ALL`, so they have to be enabled explicitly, but that requires additional work somewhere.

---

_Comment by @not-my-profile on 2023-01-28 04:23_

> And even with categorization, we'd run into the problem posed by these specific, conflicting rules

I don't really see this as a problem ... I'd say: If you enable experimental rules you're on you're own ... you accept the risk that these may lead to a bad user experience. We do not guarantee that experimental rules work well on their own or with any other rules (be they non-experimental or experimental).

> The other thing we could do is remove these from D and ALL, so they have to be enabled explicitly, but that requires additional work somewhere.

Yes I think it would make sense to make prefix selectors & `ALL` exclude experimental rules.

---

_Comment by @charliermarsh on 2023-01-28 04:26_

But, are D203 and D213 really _experimental_?

---

_Comment by @not-my-profile on 2023-01-28 04:35_

The exact name of that category is of course subject to bikeshedding^^.

Well D203 conflicts with [PEP 257](https://peps.python.org/pep-0257/) ... so yes I think it should certainly not be part of the default ruff experience. And neither should be conflicting rules.

Let's discuss some other examples:

https://github.com/charliermarsh/ruff/blob/8c70247188e379acb52fe3d9aa2cb99139214c59/scripts/pyproject.toml#L6-L7

I consider the current implementation of both of these rules annoying and don't think they should be part of the default ruff experience. Both comparisons with strings and `assert` statements are incredibly common in Python code and neither of these is generally considered to be an anti-pattern.

---

_Comment by @not-my-profile on 2023-01-28 04:51_

Since I think rule categories should be implemented in conjunction with more thorough rule documentation, and I'd like to implement the many-to-many mapping first, I'll quickly implement a "nursery" classification (inspired by clippy). I think "nursery" is a better term than experimental since it's less judgemental.

Edit: opened #2292

---

_Comment by @charliermarsh on 2023-02-01 15:10_

We now disable these if incompatible pairs are enabled -- which is fine with me for now.

---

_Closed by @charliermarsh on 2023-02-01 15:10_

---

_Branch deleted on 2023-02-01 15:10_

---
