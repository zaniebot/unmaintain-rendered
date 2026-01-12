```yaml
number: 1611
title: Treat convention as setting ignore, rather than select
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/convention
created_at: 2023-01-04T01:34:59Z
updated_at: 2023-01-04T08:38:27Z
url: https://github.com/astral-sh/ruff/pull/1611
synced_at: 2026-01-12T15:55:06Z
```

# Treat convention as setting ignore, rather than select

---

_@charliermarsh_

This PR treats the `convention` setting as an "ignore" directive, rather than a "select" directive, which leads to confusing settings and behavior.

The new semantics are such that setting a convention force-disables any rules that are incompatible with that convention. (It's effectively as-if those rules don't exist.) You can then disable further rules via `ignore` directive.

For example, this would now be a correct and reasonable configuration:

```toml
[tool.ruff]
select = [
    "D",
]
ignore = [
    "D1",
]

[tool.ruff.pydocstyle]
convention = "google"
```


The biggest limitation here is that you _can't_ enable rules that aren't compatible with the convention, as long as the convention is set. It's impossible. So for example, if you used `pep257` but wanted to enable a few of the checks that `pep257` excludes by default, you'd be out of luck. This _is_ a limitation, and I considered a few other models (like letting `extend-select` override this behavior)... but honestly, it felt like an edge case, and all the other models had their own confusing behaviors. If you find yourself in that position, you can always avoid setting `convention` and select codes manually.

See the discussion here: #1569. This approach also would've prevented this issue: #1603.


---

_Comment by @charliermarsh on 2023-01-04 01:35_

\cc @stinodego - how does this look?

---

_Merged by @charliermarsh on 2023-01-04 02:27_

---

_Closed by @charliermarsh on 2023-01-04 02:27_

---

_Branch deleted on 2023-01-04 02:27_

---

_Comment by @stinodego on 2023-01-04 04:33_

Thanks @charliermarsh ! Exactly what I had in mind.

Regarding the limitation of this approach: I believe this now matches how `flake8-docstrings` is normally configured. So even though it is a limitation, it will at least be familiar to people coming from `flake8`. And it's easy to work around, by creating your 'own' convention as a set of ignores. This is what we previous did (and still do now with Ruff) in Polars.

---

_Comment by @ManonMarchand on 2023-01-04 08:38_

Perfect, thanks!

---
