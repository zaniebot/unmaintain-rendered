```yaml
number: 9744
title: Removing trailing whitespace inside multiline strings is unsafe
type: pull_request
state: merged
author: sanxiyn
labels:
  - bug
assignees: []
merged: true
base: main
head: trailing-whitespace-inside-multiline-string
created_at: 2024-01-31T15:52:08Z
updated_at: 2024-01-31T21:57:49Z
url: https://github.com/astral-sh/ruff/pull/9744
synced_at: 2026-01-12T15:55:30Z
```

# Removing trailing whitespace inside multiline strings is unsafe

---

_@sanxiyn_

Fix #8037.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs`:113 on 2024-01-31 15:56_

Slightly more ergonomic:

```rust
diagnostic.set_fix(Fix::applicable_edit(
    Edit::range_deletion(range),
    if safe {
        Applicability::Safe
    } else {
        Applicability::Unsafe
    }
));
```

---

_@charliermarsh reviewed on 2024-01-31 15:56_

---

_@charliermarsh reviewed on 2024-01-31 15:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pycodestyle/W291.py`:2 on 2024-01-31 15:57_

Do you mind adding a test for a multi-line f-string here?

---

_@charliermarsh reviewed on 2024-01-31 15:57_

Nice, thanks!

---

_Label `bug` added by @charliermarsh on 2024-01-31 15:57_

---

_Comment by @github-actions[bot] on 2024-01-31 16:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -20 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L2'>latch/resources/map_tasks.py:2:60:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L2'>latch/resources/map_tasks.py:2:60:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L3'>latch/resources/map_tasks.py:3:61:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L3'>latch/resources/map_tasks.py:3:61:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L4'>latch/resources/map_tasks.py:4:60:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L4'>latch/resources/map_tasks.py:4:60:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L5'>latch/resources/map_tasks.py:5:58:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L5'>latch/resources/map_tasks.py:5:58:</a> W291 [*] Trailing whitespace
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L204'>reflex/.templates/apps/demo/code/pages/datatable.py:204:140:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L204'>reflex/.templates/apps/demo/code/pages/datatable.py:204:140:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L205'>reflex/.templates/apps/demo/code/pages/datatable.py:205:156:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L205'>reflex/.templates/apps/demo/code/pages/datatable.py:205:156:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L206'>reflex/.templates/apps/demo/code/pages/datatable.py:206:128:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L206'>reflex/.templates/apps/demo/code/pages/datatable.py:206:128:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L207'>reflex/.templates/apps/demo/code/pages/datatable.py:207:100:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L207'>reflex/.templates/apps/demo/code/pages/datatable.py:207:100:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L21'>reflex/.templates/apps/demo/code/pages/forms.py:21:32:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L21'>reflex/.templates/apps/demo/code/pages/forms.py:21:32:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L22'>reflex/.templates/apps/demo/code/pages/forms.py:22:39:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L22'>reflex/.templates/apps/demo/code/pages/forms.py:22:39:</a> W291 [*] Trailing whitespace
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| W291 | 20 | 0 | 0 | 0 | 20 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -20 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L2'>latch/resources/map_tasks.py:2:60:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L2'>latch/resources/map_tasks.py:2:60:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L3'>latch/resources/map_tasks.py:3:61:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L3'>latch/resources/map_tasks.py:3:61:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L4'>latch/resources/map_tasks.py:4:60:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L4'>latch/resources/map_tasks.py:4:60:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L5'>latch/resources/map_tasks.py:5:58:</a> W291 Trailing whitespace
- <a href='https://github.com/latchbio/latch/blob/f8fcd3b7ecb7369c3b6a6f92580fc5abe7bf7073/latch/resources/map_tasks.py#L5'>latch/resources/map_tasks.py:5:58:</a> W291 [*] Trailing whitespace
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L204'>reflex/.templates/apps/demo/code/pages/datatable.py:204:140:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L204'>reflex/.templates/apps/demo/code/pages/datatable.py:204:140:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L205'>reflex/.templates/apps/demo/code/pages/datatable.py:205:156:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L205'>reflex/.templates/apps/demo/code/pages/datatable.py:205:156:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L206'>reflex/.templates/apps/demo/code/pages/datatable.py:206:128:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L206'>reflex/.templates/apps/demo/code/pages/datatable.py:206:128:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L207'>reflex/.templates/apps/demo/code/pages/datatable.py:207:100:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/datatable.py#L207'>reflex/.templates/apps/demo/code/pages/datatable.py:207:100:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L21'>reflex/.templates/apps/demo/code/pages/forms.py:21:32:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L21'>reflex/.templates/apps/demo/code/pages/forms.py:21:32:</a> W291 [*] Trailing whitespace
+ <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L22'>reflex/.templates/apps/demo/code/pages/forms.py:22:39:</a> W291 Trailing whitespace
- <a href='https://github.com/reflex-dev/reflex/blob/5176a7cb14c96807626e6ed4e7cdc87189910406/reflex/.templates/apps/demo/code/pages/forms.py#L22'>reflex/.templates/apps/demo/code/pages/forms.py:22:39:</a> W291 [*] Trailing whitespace
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| W291 | 20 | 0 | 0 | 0 | 20 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

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

_@sanxiyn reviewed on 2024-01-31 17:36_

---

_Review comment by @sanxiyn on `crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs`:113 on 2024-01-31 17:36_

Done!

---

_@sanxiyn reviewed on 2024-01-31 17:37_

---

_Review comment by @sanxiyn on `crates/ruff_linter/resources/test/fixtures/pycodestyle/W291.py`:2 on 2024-01-31 17:37_

I added a test and it failed! From a brief look, it didn't seem trivial, and I would prefer to do this as a separate PR.

---

_@charliermarsh reviewed on 2024-01-31 21:38_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pycodestyle/W291.py`:2 on 2024-01-31 21:38_

Sounds good!

---

_@charliermarsh approved on 2024-01-31 21:39_

Thanks!

---

_Merged by @charliermarsh on 2024-01-31 21:45_

---

_Closed by @charliermarsh on 2024-01-31 21:45_

---

_Branch deleted on 2024-01-31 21:47_

---
