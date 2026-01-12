```yaml
number: 9808
title: Add fast-path for comment detection
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/eradicate
created_at: 2024-02-03T16:22:26Z
updated_at: 2024-02-05T16:09:21Z
url: https://github.com/astral-sh/ruff/pull/9808
synced_at: 2026-01-12T15:55:30Z
```

# Add fast-path for comment detection

---

_@charliermarsh_

## Summary

When we fall through to parsing, the comment-detection rule is a significant portion of lint time. This PR adds an additional fast heuristic whereby we abort if a comment contains two consecutive name tokens (via the zero-allocation lexer). For the `ctypeslib.py`, which has a few cases that are now caught by this, it's a 2.5x speedup for the rule (and a 20% speedup for token-based rules).

---

_Comment by @codspeed-hq[bot] on 2024-02-03 16:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/eradicate)

### Merging #9808 will **improve performances by 4.84%**

<sub>Comparing <code>charlie/eradicate</code> (c63bc5e) with <code>main</code> (b47f85e)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/eradicate` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 24.4 ms | 23.2 ms | +4.84% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 21.7 ms | 20.7 ms | +4.44% |


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/eradicate/detection.rs`:66 on 2024-02-04 11:12_

The comment mentions that this tests if the comment starts with two consecutive identifiers but the implementation consumes all tokens and tests if there are any consecutive identifiers. 

If it's only the first pair, than I would probably use (I'm not a huge can of `tuple_windows` because it's a performance footgun if the values are expensive to `Clone`. This is not the case here but it's the reason why I try to avoid it in general).

```rust
if let (Some(a), Some(b)) = (tokenizer.next(), tokenizer.next()) {
	if a.kind == SimpleTokenKind::Name && b.kind == SimpleTokenKind::Name {
	  return false;
	}
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/eradicate/detection.rs`:66 on 2024-02-04 11:13_

What if we have 

```python
# a
# b
```

Or does this only ever run over a single comment

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:185 on 2024-02-04 11:14_

How can we avoid returning a `Name` for a string prefix? 

---

_@MichaReiser reviewed on 2024-02-04 11:14_

---

_@charliermarsh reviewed on 2024-02-04 12:01_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:66 on 2024-02-04 12:01_

It’s in draft for this reason — haven’t finished the PR!

(This only ever runs over a single own-line comment.)

---

_Marked ready for review by @charliermarsh on 2024-02-04 21:37_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-04 21:37_

---

_Label `performance` added by @charliermarsh on 2024-02-04 21:37_

---

_Comment by @github-actions[bot] on 2024-02-04 21:51_

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

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:185 on 2024-02-05 13:33_

I don't feel comfortable with this. It violates the constraint outlined in the `SimpleTokenizer` lexer where it guarantees to generate valid results for as long as you don't point it in the middle of a token, string, or comment.

It's also not a constraint easily guarded against because it yields incorrect results whenever you start lexing at the start of ANY expression. 

I recommend exploring how expensive it is to handle string prefixes. 

---

_@MichaReiser requested changes on 2024-02-05 13:35_

I don't feel comfortable having such an important constraint in an inline comment that violates the basic properties of `SimpleTokenizer`. We should explore if we can support proper name lexing in `SimpleTokenizer` without degrading performance. 


---

_@charliermarsh reviewed on 2024-02-05 13:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:185 on 2024-02-05 13:42_

I’m confused - this is an internal method? The SimpleTokenizer handles this internally.

---

_@charliermarsh reviewed on 2024-02-05 13:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:185 on 2024-02-05 13:44_

We do properly handle string prefixes in the simple tokenizer - see below (and the test cases). In what case would this yield incorrect results?

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-05 13:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:185 on 2024-02-05 15:09_

I see now. You already do what I recommended and I misunderstood the comment. I assumed that the "caller" referred to in the comment is the caller of `SimpleTokenizer::next`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:573 on 2024-02-05 15:13_

This is an over-approximation and might be fine... but it will return a different output than `Lexer` gives for 

```python
abc"test"
```

Our regular lexer returns `Name` and `String` but this implementation returns `Other`, `Other`. 

It would be nice if we could align the behavior by either matching on all possible valid prefixes OR do the prefix check before lexing the identifier, similar to what `Lexer` does

---

_@MichaReiser approved on 2024-02-05 15:13_

---

_@charliermarsh reviewed on 2024-02-05 15:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:573 on 2024-02-05 15:50_

I changed it to be more defensive.

---

_@charliermarsh reviewed on 2024-02-05 15:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:185 on 2024-02-05 15:50_

I clarified in the comment -- sorry!

---

_Merged by @charliermarsh on 2024-02-05 16:00_

---

_Closed by @charliermarsh on 2024-02-05 16:00_

---

_Branch deleted on 2024-02-05 16:00_

---
