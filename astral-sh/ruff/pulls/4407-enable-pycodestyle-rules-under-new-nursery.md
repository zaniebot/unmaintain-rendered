```yaml
number: 4407
title: "Enable `pycodestyle` rules under new \"nursery\" category"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/nursery
created_at: 2023-05-12T23:24:39Z
updated_at: 2023-05-16T21:26:50Z
url: https://github.com/astral-sh/ruff/pull/4407
synced_at: 2026-01-12T15:55:15Z
```

# Enable `pycodestyle` rules under new "nursery" category

---

_@charliermarsh_

## Summary

This PR introduces a "nursery" category, following Clippy's convention, for rules that are under development, but that users can opt into explicitly.

In our case, if a rule is part of `RuleGroup::Nursery`, then it can't be selected via any of the prefix selectors (e.g., `--select E` doesn't enable `E225`, but `--select E225` _does_).

I'm not very happy with the implementation, but our current approach using proc macros makes this hard to do elegantly, in my opinion. The challenge is that we need access to the `RuleGroup` from within the proc macro (`crates/ruff_macros/src/map_codes.rs`), because we need to avoid adding any nursery rules to the statically-generated prefix selectors; but in that proc macro, we only have access to syntactic `Path` data, and not the structs themselves, so we end up with some brittle string matching.

Closes #4360.


---

_Comment by @github-actions[bot] on 2023-05-12 23:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     15.6±0.02ms     2.6 MB/sec    1.00     14.9±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.8±0.00ms     4.4 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.06    395.4±1.35µs     7.5 MB/sec    1.00    374.3±0.89µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.05      6.6±0.01ms     3.9 MB/sec    1.00      6.3±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.07      8.0±0.01ms     5.1 MB/sec    1.00      7.5±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10   1728.9±5.72µs     9.6 MB/sec    1.00   1565.1±6.71µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.08    182.0±0.52µs    16.2 MB/sec    1.00    169.1±2.53µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.6±0.00ms     7.0 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1152.8±1.01µs    14.4 MB/sec    1.01   1159.8±1.86µs    14.4 MB/sec
parser/numpy/globals.py                    1.00    119.6±0.92µs    24.7 MB/sec    1.00    119.6±0.27µs    24.7 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.2 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     23.1±1.28ms  1807.3 KB/sec    1.00     22.7±0.66ms  1834.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.9±0.26ms     2.8 MB/sec    1.00      5.8±0.23ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   693.7±58.18µs     4.3 MB/sec    1.00   695.3±36.64µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.7±0.37ms     2.6 MB/sec    1.00      9.6±0.24ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.03     11.8±0.41ms     3.5 MB/sec    1.00     11.4±0.39ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.5±0.09ms     6.7 MB/sec    1.00      2.4±0.11ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.03   295.9±13.63µs    10.0 MB/sec    1.00   286.3±18.19µs    10.3 MB/sec
linter/default-rules/pydantic/types.py     1.08      5.4±0.26ms     4.7 MB/sec    1.00      5.0±0.21ms     5.1 MB/sec
parser/large/dataset.py                    1.00      9.4±0.45ms     4.3 MB/sec    1.00      9.4±0.61ms     4.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1719.1±93.10µs     9.7 MB/sec    1.01  1740.5±64.16µs     9.6 MB/sec
parser/numpy/globals.py                    1.00   171.9±10.55µs    17.2 MB/sec    1.05    180.2±8.90µs    16.4 MB/sec
parser/pydantic/types.py                   1.00      3.7±0.12ms     6.9 MB/sec    1.04      3.8±0.13ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-05-13 12:35_

Does this implementation exclude the nursery rules from `--ALL` selector? 

---

_Review comment by @MichaReiser on `crates/ruff/src/codes.rs`:45 on 2023-05-13 12:37_

From a conceptual point. How is a `RuleGroup` different from the "Linter" (?) Group? 

E.g. `Pycodestyle` vs `Unspecified`. Could `Nursery` be another variant in the linter group?

---

_Comment by @charliermarsh on 2023-05-13 12:37_

Yeah -- those are the changes in `crates/ruff/src/rule_selector.rs`.

---

_Review comment by @MichaReiser on `crates/ruff/src/rule_selector.rs`:157 on 2023-05-13 12:38_

Does this work?
```suggestion
                RuleSelectorIter::All(Rule::into_iter().filter(select_all))
```

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:28 on 2023-05-13 12:41_

I agree with clippy ;)

Can we define a struct for the value of the inner `BTreeMap`? Or a struct that hides the details of the outer `BTreeMap` value?

---

_@MichaReiser approved on 2023-05-13 12:42_

---

_Review comment by @charliermarsh on `crates/ruff_macros/src/map_codes.rs`:28 on 2023-05-13 12:53_

Me too, but this whole thing should be refactored, in my opinion. I'll correct this.

---

_@charliermarsh reviewed on 2023-05-13 12:53_

---

_@charliermarsh reviewed on 2023-05-13 12:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:45 on 2023-05-13 12:57_

I can try it. My initial thinking was that it wouldn't be possible. The Linter encodes a prefix, so if you look at the bugbear rules as an example, they're defined like:

```rust
(Flake8Bugbear, "002") => Rule::UnaryPrefixIncrement,
```

This rule is selectable as `B002`, despite `B` not appearing in the definition, because `B` is associated with `Flake8Bugbear` rather than the rule. So on the surface, there'd be no way to use a `Nursery` group and associate rules with (e.g.) `E225`.

However, maybe it does work somehow? Pycodestyle is associated with two prefixes:

```rust
#[prefix = "E"]
#[prefix = "W"]
Pycodestyle,
```

I don't really know how that works. I'll look into it.

---

_@charliermarsh reviewed on 2023-05-13 12:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:45 on 2023-05-13 12:58_

Mmm, thinking on it more, there are other benefits to keeping these as separate concepts. For example, with the current setup, these rules will appear under the Pycodestyle section in the docs, which I think is good?

---

_@MichaReiser reviewed on 2023-05-13 12:58_

---

_Review comment by @MichaReiser on `crates/ruff/src/codes.rs`:45 on 2023-05-13 12:58_

That makes sense. Don't spend too much time on it. I now understand the reasoning behind it. Thank you.

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:45 on 2023-05-13 13:02_

Do you think `RuleGroup` should instead be defined as part of the violation (ideally -- it's actually not possible to do with the proc macro, but ideally)? Alternatively, should we nix `RuleGroup` and _just_ do a nursery-oriented design for now?

---

_@charliermarsh reviewed on 2023-05-13 13:02_

---

_@MichaReiser reviewed on 2023-05-13 13:10_

---

_Review comment by @MichaReiser on `crates/ruff/src/codes.rs`:45 on 2023-05-13 13:10_

I'm not very familiar with our setup yet. I would find it ideal if there's a single place where I have to look for and define metadata for a rule. Whether this is on the rule or in the violation seems less important to me. 

Can you explain what you mean by a nursery-oriented design?

---

_@charliermarsh reviewed on 2023-05-13 13:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:45 on 2023-05-13 13:17_

By “nursery-oriented design”, I mean an approach that models the data as “nursery or not” (eg it could be a Boolean) rather than as a generic “rule group” concept and enumeration.

---

_@MichaReiser reviewed on 2023-05-15 07:50_

---

_Review comment by @MichaReiser on `crates/ruff/src/codes.rs`:45 on 2023-05-15 07:50_

I don't think I have a preference. A `bool` `RuleGroup` seems very similar to a two-variant enum `RuleGroup` to me. Let's not overthink this now. 

---

_@charliermarsh reviewed on 2023-05-16 20:50_

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:157 on 2023-05-16 20:50_

Unfortunately not :/

---

_Merged by @charliermarsh on 2023-05-16 21:21_

---

_Closed by @charliermarsh on 2023-05-16 21:21_

---

_Branch deleted on 2023-05-16 21:21_

---
