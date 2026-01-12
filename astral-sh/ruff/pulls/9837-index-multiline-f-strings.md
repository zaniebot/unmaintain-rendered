```yaml
number: 9837
title: Index multiline f-strings
type: pull_request
state: merged
author: sanxiyn
labels:
  - linter
assignees: []
merged: true
base: main
head: multiline-fstring
created_at: 2024-02-05T16:28:53Z
updated_at: 2024-02-06T02:25:42Z
url: https://github.com/astral-sh/ruff/pull/9837
synced_at: 2026-01-12T15:55:30Z
```

# Index multiline f-strings

---

_@sanxiyn_

Fix #9777.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-05 16:46_

---

_Comment by @github-actions[bot] on 2024-02-05 16:47_

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

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
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
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_@dhruvmanila reviewed on 2024-02-05 19:11_

This looks good although it's unfortunate that it requires adding another field to the token. 

I think there would be more required for f-strings. If there's a whitespace after the opening curly brace, then it should be a safe fix:

```py
# trailing whitespace vvv
f"""literal element {    
    expression
} end"""
```

But, then if there's a debug expression, then it's actually an unsafe fix because whitespaces are preserved for such case:

```py
# trailing whitespace vvv
f"""literal element {    
    expression =
} end"""
```

Printing the above f-string would preserve the whitespace after the opening curly brace.

---

_Label `linter` added by @dhruvmanila on 2024-02-05 19:14_

---

_@charliermarsh reviewed on 2024-02-05 19:33_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pycodestyle/W291.py`:5 on 2024-02-05 19:33_

Can we add tests for:

```python
# Trailing whitespace after `{`
print(f"abc {  
    1 + 2
}")
```

And:

```python
# Trailing whitespace after `2`
print(f"abc {
    1 + 2  
}")
```


---

_@sanxiyn reviewed on 2024-02-06 01:52_

---

_Review comment by @sanxiyn on `crates/ruff_linter/resources/test/fixtures/pycodestyle/W291.py`:5 on 2024-02-06 01:52_

Added!

---

_@charliermarsh reviewed on 2024-02-06 02:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pycodestyle/W291.py`:5 on 2024-02-06 02:00_

Thank you :)

---

_@charliermarsh approved on 2024-02-06 02:24_

---

_Merged by @charliermarsh on 2024-02-06 02:25_

---

_Closed by @charliermarsh on 2024-02-06 02:25_

---

_Branch deleted on 2024-02-06 02:25_

---
