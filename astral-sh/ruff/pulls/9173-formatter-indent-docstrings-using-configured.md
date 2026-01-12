```yaml
number: 9173
title: "[formatter] Indent docstrings using configured indent_style"
type: pull_request
state: closed
author: sciyoshi
labels:
  - formatter
assignees: []
base: main
head: indent-docstrings-style
created_at: 2023-12-18T03:03:53Z
updated_at: 2024-02-08T14:58:44Z
url: https://github.com/astral-sh/ruff/pull/9173
synced_at: 2026-01-12T15:55:28Z
```

# [formatter] Indent docstrings using configured indent_style

---

_@sciyoshi_

## Summary

Ruff currently always formats docstrings using spaces for indentation. However, when `indent-style` is set to "tab", this leads to E101 (mixed indentation) linter errors.

There are a few approaches to improving the situation, some of which are discussed in https://github.com/astral-sh/ruff/issues/8430. One option which is likely the simplest is to recommend turning off E101 when using the formatter. If this is the preferred route, then Ruff should probably also respect the `indent-width` when expanding tabs in docstrings to avoid the issue [described here](https://github.com/astral-sh/ruff/issues/8430#issuecomment-1822529090).

This PR implements an alternative solution which indents docstrings using the configured `indent-style`. In cases where docstrings have indents that aren't a multiple of the `indent-width`, this may mean E101 is still triggered in the formatted code. However, this should be very uncommon. I did a cursory check of the few codebases across GitHub that use Ruff with tabs and could not find any cases where docstrings would be negatively affected by this change.

Note that this means that Ruff will format code with tabs differently than Black, which always expands tabs to 8 spaces. I think this is acceptable in this case, because Black doesn't support tabs to begin with (so there's no real compatibility issue).

There is also the question of how other tooling treats tabs in docstrings. As of Python 3.13, docstrings will be dedented to save memory. However, tabs will also unfortunately be expanded to 8 spaces by the interpreter, which will negate the memory usage savings, but this difference should also be mostly inconsequential (and this may potentially be fixed in CPython in the future.)

Closes #8430

---

_Comment by @github-actions[bot] on 2023-12-18 03:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
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

_Label `formatter` added by @MichaReiser on 2023-12-18 03:20_

---

_Label `needs-decision` added by @MichaReiser on 2023-12-18 03:24_

---

_Comment by @MichaReiser on 2023-12-18 03:30_

Thank you for this PR and pushing for another decison. 

I don't feel comfortable making this decision right now because I don't have a good enough understanding of docstring handling in Python and I want to avoid the situation where we change the definition only to later change it again. The PR also seems to change docstring formatting when using `indent-style=space`, meaning this PR reduces black compatibiliity. This doesn't mean we shouldn't move forward with this PR, it's just that I need to find time time to go through the issues and familiarize myself with Python's docstring handling. I'm happy to move forward with this PR if someone else from the team has enough context and feels comfortable making the decision. 

Note: We need to double check how this affects code block formatting inside docstrings (@BurntSushi)

---

_Label `needs-decision` removed by @MichaReiser on 2024-02-06 01:56_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-06 01:56_

---

_Comment by @MichaReiser on 2024-02-06 01:57_

I'm now looking into this and plan to make a decision tomorrow. 

---

_Comment by @MichaReiser on 2024-02-08 14:57_

Thanks, @sciyoshi, for putting the work into this and pushing for a decision. 

I understand that changing tabs in docstrings to spaces can be surprising if a specific documentation convention uses them. Unfortunately, changing spaces to a tab isn't safe when the spaces are used for alignment. 

I believe the proper fix is to add support for docstring formatting (that supports the different conventions). This way, we avoid the risk of breaking alignment and match the expectation of using tabs in these cases. 

See https://github.com/astral-sh/ruff/issues/8430#issuecomment-1934209625 for an in-depth explanation. 

---

_Closed by @MichaReiser on 2024-02-08 14:58_

---
