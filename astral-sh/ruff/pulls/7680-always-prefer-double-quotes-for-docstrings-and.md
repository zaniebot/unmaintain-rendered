```yaml
number: 7680
title: Always prefer double quotes for docstrings and triple-quoted srings
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
  - formatter
assignees: []
merged: true
base: main
head: charlie/quote-style
created_at: 2023-09-27T20:51:38Z
updated_at: 2023-09-28T19:11:35Z
url: https://github.com/astral-sh/ruff/pull/7680
synced_at: 2026-01-12T02:39:10Z
```

# Always prefer double quotes for docstrings and triple-quoted srings

---

_Pull request opened by @charliermarsh on 2023-09-27 20:51_

## Summary

At present, `quote-style` is used universally. However, [PEP 8](https://peps.python.org/pep-0008/) and [PEP 257](https://peps.python.org/pep-0257/) suggest that while either single or double quotes are acceptable in general (as long as they're consistent), docstrings and triple-quoted strings should always use double quotes. In our research, the vast majority of Ruff users that enable the `flake8-quotes` rules only enable them for inline strings (i.e., non-triple-quoted strings). 

Additionally, many Black forks (like Blue and Pyink) use double quotes for docstrings and triple-quoted strings.

Our decision for now is to always prefer double quotes for triple-quoted strings (which should include docstrings). Based on feedback, we may consider adding additional options (e.g., a `"preserve"` mode, to avoid changing quotes; or a `"multiline-quote-style"` to override this).

Closes https://github.com/astral-sh/ruff/issues/7615.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-27 20:51_

---

_Review requested from @konstin by @charliermarsh on 2023-09-27 20:51_

---

_Label `formatter` added by @charliermarsh on 2023-09-27 20:51_

---

_@charliermarsh reviewed on 2023-09-27 20:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:334 on 2023-09-27 20:51_

I think this logic could go at multiple different levels in this call stack, but this felt simplest.

---

_Comment by @codspeed-hq[bot] on 2023-09-27 21:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/quote-style)

### Merging #7680 will **not alter performance**

<sub>Comparing <code>charlie/quote-style</code> (f546496) with <code>main</code> (46b85ab)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_@konstin approved on 2023-09-27 21:08_

i thought i'd just quickly implement this tomorrow and now you've already done it :D

not opinionated where the if-triple-quotes part goes, maybe micha has opinions though

---

_Comment by @github-actions[bot] on 2023-09-27 21:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-09-27 22:09_

@konstin - Lol don't worry, there are plenty of accepted formatter issues now :D

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:330 on 2023-09-28 06:56_

Nit: The variable here is called `preferred_style` but it is called `configured_style` in `peferred_quotes_raw` and `preferred_quotes`. 

I would be okay calling it `configured_style` or maybe `default_style` to make it clear it's what we would default to. 

---

_@MichaReiser approved on 2023-09-28 06:57_

Should we rename the `quote-style` setting to `inline-string-quote-style` (or similar) to align with flake8-quotes? The one issue I see with it is that it ties our hands with adding a `preserve` option, because that would then only apply to inline strings and not strings.


Should we mark this as breaking? It's not breaking as far as our versioning policy is concerned but it's worth giving it an explicit call out in the changelog.

---

_@charliermarsh reviewed on 2023-09-28 14:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:330 on 2023-09-28 14:53_

Yeah I went back and forth on these a few times :joy:

---

_Label `breaking` added by @charliermarsh on 2023-09-28 18:49_

---

_Comment by @charliermarsh on 2023-09-28 19:11_

I'm on the fence but let's leave it as `quote-style` for now... We may have to change it either way depending on what we learn (i.e., if we had `multiline-quote-style`, then we need to rename this; if we add `preserve`, then we'd probably need to rename away from `inline-quote-style`), but if we can get away with it, `quote-style` is simpler and more obvious.


---

_Merged by @charliermarsh on 2023-09-28 19:11_

---

_Closed by @charliermarsh on 2023-09-28 19:11_

---

_Branch deleted on 2023-09-28 19:11_

---
