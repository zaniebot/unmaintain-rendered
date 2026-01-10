```yaml
number: 9802
title: Add our own ignored-names abstractions
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/ignore-names
created_at: 2024-02-02T21:15:35Z
updated_at: 2024-02-03T14:50:41Z
url: https://github.com/astral-sh/ruff/pull/9802
synced_at: 2026-01-10T22:57:09Z
```

# Add our own ignored-names abstractions

---

_Pull request opened by @charliermarsh on 2024-02-02 21:15_

## Summary

These run over nearly every identifier. It's rare to override them, so when not provided, we can just use a match against the hardcoded default set.


---

_Comment by @codspeed-hq[bot] on 2024-02-02 21:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/ignore-names)

### Merging #9802 will **not alter performance**

<sub>Comparing <code>charlie/ignore-names</code> (6a239a7) with <code>main</code> (e0a6034)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-02 21:40_

---

_Label `performance` added by @charliermarsh on 2024-02-02 21:40_

---

_Marked ready for review by @charliermarsh on 2024-02-02 21:40_

---

_@charliermarsh reviewed on 2024-02-02 21:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pep8_naming/settings.rs`:156 on 2024-02-02 21:40_

I don't know if this is preferable to using AhoCorasick or similar. Seems like a big speed-up as-is though.

---

_Comment by @github-actions[bot] on 2024-02-02 21:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pep8_naming/settings.rs`:156 on 2024-02-02 22:13_

Yeah this one is less clear to me. This time it's an equality comparison with a smaller number of strings. `AhoCorasick` might still be faster, but it'd surprise me if it was by a lot. And it wouldn't surprise me if it were slower. So I'm happy to see the improvement land. :-)

---

_@BurntSushi approved on 2024-02-02 22:14_

Nice! I also like the encapsulation introduced here. Looks like the code is simpler to me. :)

---

_@charliermarsh reviewed on 2024-02-02 22:47_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pep8_naming/settings.rs`:212 on 2024-02-02 22:47_

\cc @snowsignal - in case you had ideas on this. I created a wrapper type with an enum variant that _can_ contain a vector, so had to (sorta) reimplement our settings-array formatting in the display.

---

_Merged by @charliermarsh on 2024-02-03 14:48_

---

_Closed by @charliermarsh on 2024-02-03 14:48_

---

_Branch deleted on 2024-02-03 14:48_

---
