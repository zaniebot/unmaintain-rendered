```yaml
number: 9429
title: Allow Boolean positionals in setters
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/fbt
created_at: 2024-01-08T04:25:17Z
updated_at: 2024-01-08T18:02:18Z
url: https://github.com/astral-sh/ruff/pull/9429
synced_at: 2026-01-12T15:55:28Z
```

# Allow Boolean positionals in setters

---

_@charliermarsh_

## Summary

Ignores Boolean trap enforcement for methods that appear to be setters (as in the Qt and pygame APIs).

Closes https://github.com/astral-sh/ruff/issues/9287.
Closes https://github.com/astral-sh/ruff/issues/8923.


---

_Label `rule` added by @charliermarsh on 2024-01-08 04:25_

---

_Comment by @github-actions[bot] on 2024-01-08 04:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/server/app/spectrogram/app_hooks.py#L8'>examples/server/app/spectrogram/app_hooks.py:8:17:</a> FBT003 Boolean positional value in function call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT003 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/server/app/spectrogram/app_hooks.py#L8'>examples/server/app/spectrogram/app_hooks.py:8:17:</a> FBT003 Boolean positional value in function call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT003 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_boolean_trap/helpers.rs`:68 on 2024-01-08 07:36_

Can we add an explanation why we do this? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_boolean_trap/helpers.rs`:73 on 2024-01-08 07:41_

I'm unsure I understand how this works (or the comment is misleading). 

* It strips `set_`
* It then tests if the next character is either `_` or an ascii uppercase character. 

What I understand is that this should match

* `set_A` 
* `set__` 

But it won't match `setV`


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_boolean_trap/helpers.rs`:73 on 2024-01-08 07:41_

I think this can be simplified to 
```suggestion
            .and_then(|suffix| suffix.starts_with(|c: char| c == '_' || c.is_ascii_uppercase()))
```

---

_@MichaReiser reviewed on 2024-01-08 07:43_

What are the downsides of allowing this calls. Does it increase the rate of false negatives? 

---

_@MichaReiser reviewed on 2024-01-08 07:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_boolean_trap/FBT.py`:75 on 2024-01-08 07:44_

Is it intentional to use the identifier `true` here instead of the singleton keyword value `True`?

---

_@charliermarsh reviewed on 2024-01-08 14:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_boolean_trap/helpers.rs`:73 on 2024-01-08 14:27_

Sorry, it should be `strip_prefix("set")` to match `set_` and `setA`.

---

_@charliermarsh reviewed on 2024-01-08 14:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_boolean_trap/FBT.py`:75 on 2024-01-08 14:28_

Uhh no

---

_Comment by @charliermarsh on 2024-01-08 14:37_

@MichaReiser - Yeah, that's right -- there will be some false negatives. But based on the issues, I think this is the right tradeoff -- it's almost always a false positive in these cases, with no workaround.

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-08 14:37_

---

_Merged by @charliermarsh on 2024-01-08 18:02_

---

_Closed by @charliermarsh on 2024-01-08 18:02_

---

_Branch deleted on 2024-01-08 18:02_

---
