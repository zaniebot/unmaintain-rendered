```yaml
number: 9751
title: "Range formatting: Fix invalid syntax after parenthesizing expression"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: range-formatting-parentheses
created_at: 2024-02-01T12:35:50Z
updated_at: 2024-02-02T16:56:26Z
url: https://github.com/astral-sh/ruff/pull/9751
synced_at: 2026-01-10T22:57:09Z
```

# Range formatting: Fix invalid syntax after parenthesizing expression

---

_Pull request opened by @MichaReiser on 2024-02-01 12:35_

## Summary

This PR fixes an issue where range formatting omitted the closing parentheses when formatting a range that end's at an expression that gets parenthesized. 

The issue is there's no unambiguous mapping for the position after `"automatic"` in the following example:

```python
def needs_parentheses( ) -> bool:
    return item.sizing_mode is None and item.width_policy == "auto" and item.height_policy == "automatic"
```

This gets reformatted to 

```python
def needs_parentheses() -> bool:
    return (
        item.sizing_mode is None
        and item.width_policy == "auto"
        and item.height_policy == "automatic"
    )

```

The generated IR ending after "automatic" roughly is (where 139 is the position after the closing quote of `"automatic"`)

```
source_position(139), // end position from the expression statement
soft_line_break(),
if_group_breaks(&text(")"))
hard_line_break(), 
source_position(139)
```

The printer will generate multiple source map entries for this

```
139 => 164 // Right after "automatic"
139 => 169 // The closing parentheses
139 => 170 // After the closing parentheses
139 => 171 // After the hard line break
```

Generating multiple mappings for the same position is correct because, depending on the use case, you either want to know where position 139 starts (you pick the first) or where it ends (you pick the last) in the formatted code. That's why the proper fix to this problem would be to include whether we look for the end or start of a node for both offsets passed to `Printed::slice_range`. This way, the source map lookup logic could pick the correct entry if multiple entries point to the same source location... However, this doesn't work for us because the suite ranges in the AST are off. 

Looking at the example from above again: Both the expression and the expression statement end at position 139. If the statement is the last in the body (which is the case in the example above), then the body, and with it, potentially the enclosing node, also ends at position 139 in our AST because the body's end position is equal to the last statement's end position. However, this is incorrect. The end offset of the body should be the position of the `Dedent` token (after the hard line break). We might fix this with the new parser, but it is as it is now. 

The problem with the body ending at offset 139 is that we incorrectly include the `hard_line_break` that ends the body even when formatting the expression statement alone because it's impossible to disambiguate the end of the expression statement from the end of the body. Including the `hard_line_break` results in inserting a new line every time we format the expression statement. 

I'm not too fond of the solution taken by this PR but it works :shrug: 

First, we reduce the granularity of the source map to only generate positions we care about. We no longer generate source map entries for:

* Text elements
* or anything below logical line level (expression, with items, etc)

This solves the problem that we can't disambiguate between the end of the expression and the expression statement. 
This also solves the problem where we can't disambiguate between the expression statement and the body because we'll now always take the first source mapping position. This can produce incorrect* results if we start inserting tokens around the statement, similar to the problem with parenthesizing expressions. However, I'm unaware of a case where we do this today or why this should be needed in the future. If it's needed in the future, than let's hope that we have our new parser by then and can fix the ranges of compound statements.

The first commit removes the automatic source map generation for text elements from the Printer. It now always requires explicit `source_position` element. 

The second commit changes the python formatter to only emit source positions for logical-line like nodes. It also fixes an issue where we didn't fix a comment's indent.


## Test Plan

Added snapshot tests.


---

_Comment by @github-actions[bot] on 2024-02-01 12:53_

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

_Label `internal` added by @MichaReiser on 2024-02-02 08:19_

---

_Label `formatter` added by @MichaReiser on 2024-02-02 08:19_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-02 08:19_

---

_Review request for @charliermarsh removed by @MichaReiser on 2024-02-02 08:19_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-02 08:19_

---

_Marked ready for review by @MichaReiser on 2024-02-02 08:31_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-02 08:41_

---

_@charliermarsh approved on 2024-02-02 14:49_

---

_Comment by @charliermarsh on 2024-02-02 14:50_

> This can produce incorrect* results if we start inserting tokens around the statement, similar to the problem with parenthesizing expressions. However, I'm unaware of a case where we do this today or why this should be needed in the future. If it's needed in the future, than let's hope that we have our new parser by then and can fix the ranges of compound statements.

Are there any other downsides to removing these source map entries? Would it mean that we lose granularity in some cases (that we don't have a use for anyway right now)?

---

_Review comment by @BurntSushi on `crates/ruff_formatter/src/builders.rs`:342 on 2024-02-02 16:16_

It looks like this function doesn't allocate any more?

---

_@BurntSushi approved on 2024-02-02 16:18_

LGTM!

---

_@MichaReiser reviewed on 2024-02-02 16:26_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/builders.rs`:342 on 2024-02-02 16:26_

This function doesn't, but it does when calling `Format`. I can improve the documentation.

---

_Comment by @MichaReiser on 2024-02-02 16:27_

@charliermarsh Yeah, it's mainly if we want to have a full fidelity source maps where we want to map exact positions from source to dest and the other way round. However, that would still have the problem I'm facing today. So that needs fixing before it can become useful anyway. 

---

_Merged by @MichaReiser on 2024-02-02 16:56_

---

_Closed by @MichaReiser on 2024-02-02 16:56_

---

_Branch deleted on 2024-02-02 16:56_

---
