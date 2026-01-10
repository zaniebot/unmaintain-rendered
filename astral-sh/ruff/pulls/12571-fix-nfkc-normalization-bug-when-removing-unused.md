```yaml
number: 12571
title: Fix NFKC normalization bug when removing unused imports
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
  - linter
assignees: []
merged: true
base: main
head: alex/f401-bug-2
created_at: 2024-07-29T18:35:34Z
updated_at: 2024-08-02T17:21:14Z
url: https://github.com/astral-sh/ruff/pull/12571
synced_at: 2026-01-10T21:47:02Z
```

# Fix NFKC normalization bug when removing unused imports

---

_Pull request opened by @AlexWaygood on 2024-07-29 18:35_

Fixes #12570.

## Summary

Our lexer eagerly applies NFKC normalization to NKFC-confusable unicode characters; this is done to match Python's semantics at runtime, where the interpreter views these characters as the same. When removing unused imports, however, we use `libCST`, which doesn't apply the same normalization (it would be incorrect for `libCST` to do so, since it's a CST rather than an AST). This can lead to a `QualifiedName` constructed from a `libCST` node not comparing equal to a `QualifiedName` constructed from a `ruff_python_ast` node that represents the same sapn of source code as the `libCST` node. The difference here between the two parsers is the root cause of #12570.

This PR therefore takes care to apply NFKC normalization when constructing `QualifiedName`s from `libCST` nodes, so that these `QualifiedName`s are always comparable to `QualifiedName`s constructed from `ruff_python_ast` nodes.

## Test plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-07-29 18:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Marked ready for review by @AlexWaygood on 2024-07-29 19:23_

---

_Comment by @MichaReiser on 2024-07-29 19:38_

Thanks for looking into this bug. This looks interesting. 

It would help me review if you could update the summary and include a short summary of what the issue was/how this PR fixes the issue.

---

_Comment by @AlexWaygood on 2024-07-29 19:43_

> Thanks for looking into this bug. This looks interesting.
> 
> It would help me review if you could update the summary and include a short summary of what the issue was/how this PR fixes the issue.

Sorry, my bad. I provided an analysis of the bug in the issue, but forgot to copy it over in the PR summary. I'll do so first thing tomorrow.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/codemods.rs`:201 on 2024-07-30 06:20_

What's the motivation for nesting these functions? Seems a fairly unrelated change and it took me quiet a bit to figure out **what** changed.

---

_@MichaReiser approved on 2024-07-30 06:21_

If I understand the change correctly, the issue is that libCST doesn't perform nfkc normalization. Is that correct?

> Sorry, my bad. I provided an analysis of the bug in the issue, but forgot to copy it over in the PR summary. I'll do so first thing tomorrow.

Linking to the analysis from the issue in the comment should be sufficient. I only want to avoid that we merge the PR with a stale summary (commit message)

---

_Comment by @AlexWaygood on 2024-07-30 09:22_

> If I understand the change correctly, the issue is that libCST doesn't perform nfkc normalization. Is that correct?

Yup, that's correct. Our lexer eagerly performs NFKC normalization to match Python's semantics, whereas libCST's lexer leaves NFKC confusables untouched. I don't think that's a bug in libCST, since it's a CST rather than an AST; but it is a difference that we need to account for when building `QualifiedName`s from `libCST` trees, since a `QualifiedName` represents semantic information, and a `QualifiedName` built from a `libCST` node should be comparable with a `QualifiedName` built from a `ruff_python_ast` node that's built from the same source.

---

_@AlexWaygood reviewed on 2024-07-30 09:29_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/codemods.rs`:201 on 2024-07-30 09:29_

Sorry again for taking this out of draft mode before I had written a PR summary. The motivation here is that the `UnqualifiedName` produced by `unqualified_name_from_expression` should also strictly-speaking be NFKC-normalized before it's safe to be used elsewhere and potentially compared to other `UnqualifiedName`s; the same applies for the segments collected by `collect_segments()`. Currently we only use these functions from the `qualified_name_from_name_or_attribute()`, so I think it's okay (and _probably_ more efficient? it allocates...) to just do a single NKFC normalization pass at the end of `qualified_name_from_name_or_attribute()`, _as long as_ we enforce that `unqualified_name_from_expression()` and `collect_segments()` can't be called from other functions.

Does that make sense? I suppose I should probably add a comment.

---

_@MichaReiser reviewed on 2024-07-30 09:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/codemods.rs`:201 on 2024-07-30 09:39_

Kind of. I'm not that concerned because the functions are private anyway. But I don't feel strongly about it.

---

_@AlexWaygood reviewed on 2024-07-30 09:40_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/codemods.rs`:201 on 2024-07-30 09:40_

Yeah, I was mostly concerned that other functions in this file might start using them.

---

_Label `fixes` added by @AlexWaygood on 2024-07-30 09:49_

---

_Label `linter` added by @AlexWaygood on 2024-07-30 09:49_

---

_Merged by @AlexWaygood on 2024-07-30 09:54_

---

_Closed by @AlexWaygood on 2024-07-30 09:54_

---

_Branch deleted on 2024-07-30 09:54_

---

_Comment by @codspeed-hq[bot] on 2024-07-30 09:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/f401-bug-2)

### Merging #12571 will **improve performances by ×2**

<sub>Comparing <code>alex/f401-bug-2</code> (ffb7fd1) with <code>alex/f401-bug-2</code> (ed8e5ed)</sub>



### Summary

`⚡ 1` improvements
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `alex/f401-bug-2` | `alex/f401-bug-2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[incremental]` | 534 µs | 264.4 µs | ×2 |


---
