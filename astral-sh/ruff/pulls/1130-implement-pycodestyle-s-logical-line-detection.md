```yaml
number: 1130
title: "Implement pycodestyle's logical line detection"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pycodestyle
created_at: 2022-12-07T22:11:55Z
updated_at: 2023-02-19T15:26:30Z
url: https://github.com/astral-sh/ruff/pull/1130
synced_at: 2026-01-12T04:39:44Z
```

# Implement pycodestyle's logical line detection

---

_Pull request opened by @charliermarsh on 2022-12-07 22:11_

Along with the logical line detection, this adds 14 of the missing `pycodestyle` rules.

For now, this is all gated behind a `logical_lines` feature that's off-by-default, which will let us implement all rules prior to shipping, since we want to couple the release of these rules with new defaults and instructions.


---

_Comment by @charliermarsh on 2023-01-06 15:50_

Closing for now to keep the PR list up-to-date, can always reopen.

---

_Closed by @charliermarsh on 2023-01-06 15:50_

---

_Reopened by @charliermarsh on 2023-01-31 23:16_

---

_Marked ready for review by @charliermarsh on 2023-01-31 23:16_

---

_Comment by @charliermarsh on 2023-01-31 23:17_

Rebased + re-opened.

---

_Comment by @charliermarsh on 2023-02-01 04:32_

The main blocker here is that I don't really want to turn these on by default. I want them to be explicitly opt-in.

---

_Comment by @charliermarsh on 2023-02-01 04:57_

@not-my-profile - No obligation to reply, but since we've talked about this kind of thing a few times: any thoughts on how to enable the above? I want to add these new `E` rules, but I don't want users with `E` enabled to get them "automatically".

I'm learning towards adding them in their own category, with their own prefix (e.g., `EX` or something), so that they're opt-in. It's a little awkward, since we're then changing the codes vs. Flake8. But no one that uses an autoformatter needs these rules, and they have a greater-than-average performance penalty. (Lots of users have requested them, so I don't mind supporting them, but I want them to be off-by-default.)


---

_Comment by @not-my-profile on 2023-02-01 06:47_

Our rule codes consist of:

* a common prefix[^1] (that is shared by all rule codes from the linter)
* a linter-specific code (that identifies the rule within the linter namespace)

Ruff often changes the common prefix from the upstream rule codes to avoid namespace collisions ... which I think makes perfect sense. However I think that we should avoid changing the linter-specific codes at all cost.

The common prefix for pycodestyle is the empty string "", "E" is therefore part of the linter-specific code, so I strongly think that we should **not** change linter-specific codes such as E221, E222, E223, and E224.

Something like #2292 could be a short-term solution, but I think long-term we clearly want to use categories for this (#1774).

I'd like us to put this on hold for now and firstly:

1. Do the `crates/` migration.
2. Implement the many-to-many mapping.

[^1]: See also b532fce79268de1b6a93a0c5180e63791127f159.

---

_Comment by @charliermarsh on 2023-02-03 05:15_

Yeah I agree RE changing the prefix. I don't think we have great options in the interim. We could change the default configuration to use `["E4", "E5", "E7", "E9"]` rather than `["E"]`, which helps, but users that have `E` enabled already will now be paying this performance cost and probably won't realize it. (I do think new users tend to copy over the default configuration, so it might be ~fine from that perspective.)

I'm going to keep implementing these because I do want to ship them (a lot of it is now mechanical since it's a straight 1:1 port of `pycodestyle` with some performance optimizations). But I'm still undecided on the right compromise here.


---

_Comment by @charliermarsh on 2023-02-03 05:15_

Maybe we just change the default going forward and move on.

---

_Comment by @charliermarsh on 2023-02-05 05:18_

Gonna merge this tomorrow, possibly behind a feature flag while we figure out how to categorize and enable etc.

---

_Comment by @not-my-profile on 2023-02-05 05:34_

The `define_rule_mapping!` macro currently doesn't support feature flags.

---

_Comment by @charliermarsh on 2023-02-05 05:37_

I’ve already run into that :)

---

_Comment by @charliermarsh on 2023-02-05 16:25_

I got `define_rule_mapping!` to propagate attribute macros. But I'll merge it as a separate PR once it passes CI here.

---

_Merged by @charliermarsh on 2023-02-05 20:06_

---

_Closed by @charliermarsh on 2023-02-05 20:06_

---

_Branch deleted on 2023-02-05 20:06_

---

_Comment by @jonathan-s on 2023-02-19 15:26_

@charliermarsh, just a heads up, as a user of of ruff. The way I'm thinking about it is that ruff is in development and I'm expecting that new rules may be added, so I'm keeping my version pinned to get consistent result. So I'm expecting as more rules are added to the same category ("E" for pycodestyles) I might need to update my code. 

So in other words I wouldn't want or expect more categories for more rules of pycodestyles.   

---
