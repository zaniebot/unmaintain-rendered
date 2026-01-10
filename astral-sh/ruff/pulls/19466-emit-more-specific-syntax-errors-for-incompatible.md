```yaml
number: 19466
title: Emit more specific syntax errors for incompatible string prefixes
type: pull_request
state: closed
author: dylwil3
labels:
  - parser
assignees: []
draft: true
base: main
head: prefix-syntax-errors
created_at: 2025-07-21T15:08:35Z
updated_at: 2025-12-10T18:02:51Z
url: https://github.com/astral-sh/ruff/pull/19466
synced_at: 2026-01-10T16:42:11Z
```

# Emit more specific syntax errors for incompatible string prefixes

---

_Pull request opened by @dylwil3 on 2025-07-21 15:08_

Following the CPython changes here:

- https://github.com/python/cpython/issues/133197
- https://github.com/python/cpython/pull/133202
- https://github.com/python/cpython/pull/133242

we modify the lexing of double character string prefixes to emit a more useful syntax error in the case of invalid double character prefixes. Our implementation differs slightly from that of CPython in that our error message retains the casing of the prefixes and we emit a separate error for repeated prefixes (like `uu"hello"`).

## Example

```python
def foo():
    x = uf"a more complicated {example}"
    return x
```

Before:

```console
❯ ruff check --no-cache --isolated ex.py
ex.py:2:11: SyntaxError: Simple statements must be separated by newlines or semicolons
  |
1 | def foo():
2 |     x = uf"a more complicated {example}"
  |           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
3 |     return x
  |

Found 1 error.
```

After:

```console
❯ cargo run -p ruff -- check --no-cache --isolated ex.py
ex.py:2:9: SyntaxError: "u" and "f" prefixes are incompatible
  |
1 | def foo():
2 |     x = uf"a more complicated {example}"
  |         ^^
3 |     return x
  |

Found 1 error.
```

Closes #19342


---

_Review requested from @MichaReiser by @dylwil3 on 2025-07-21 15:08_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-07-21 15:08_

---

_Label `parser` added by @dylwil3 on 2025-07-21 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:741 on 2025-07-21 15:48_

The comment needs updating

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__repeated_string_prefix_error.snap`:5 on 2025-07-21 15:49_

Let's also add a parser test. I'm curious to see how the parser recovers overall.

---

_Comment by @github-actions[bot] on 2025-07-21 15:52_

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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:630 on 2025-07-21 15:53_

It would be helpful to include an explaination why it's okay to drop the unknown token (`push_token` should probably be marked with `#[must_use]`).

Is there a risk that any downstream user of the tokens or AST will drip over the fact that the token/ast prefix doesn't match the source prefix?

---

_@MichaReiser reviewed on 2025-07-21 15:53_

Nice. 

I'm curious to see how this looks in a parsed AST to better understand the impact of this change.

---

_@MichaReiser reviewed on 2025-07-21 16:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_string_prefix.py.snap`:55 on 2025-07-21 16:45_

`Empty` seems suboptimal. I think we should try to preserve the most important prefix (e.g. if `f` is present, favor it over `u`)

---

_@dylwil3 reviewed on 2025-07-21 16:48_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__repeated_string_prefix_error.snap`:5 on 2025-07-21 16:48_

Added. 

What happens is that the parser will behave as though the string had no prefixes and parse it as an ordinary string literal. What may be potentially misleading is that the range of this string will include the skipped prefix. So, for example, if you added up the lengths for the content of the string, the quote (single or triple), and the prefix (apparently 0), you would come up short.

I don't think this trips up the parser, and we don't do anything downstream with source that has invalid syntax - i.e. no lint rules are run - so I don't think we have to worry.

But I could try to arrange for the range to exclude the prefix? Then the AST would behave as if there was nothing there - like it does when we skip trivia.

---

_@dylwil3 reviewed on 2025-07-21 16:48_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_string_prefix.py.snap`:55 on 2025-07-21 16:48_

What is the most important prefix for `ft"hello {there}"`? or `bf"hey"`?

---

_@MichaReiser reviewed on 2025-07-21 16:51_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__repeated_string_prefix_error.snap`:5 on 2025-07-21 16:51_

> I don't think this trips up the parser, and we don't do anything downstream with source that has invalid syntax - i.e. no lint rules are run - so I don't think we have to worry.

I think we do run all token based rules. 

> But I could try to arrange for the range to exclude the prefix? Then the AST would behave as if there was nothing there - like it does when we skip trivia.

We would need to emit the prefix as an `Unknown` token because the lexer isn't allowed to skip over **any** content. I suspect that this will undo much of the improvements you made (as the `Unknown` token will confuse the parser)

---

_@MichaReiser reviewed on 2025-07-21 16:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_string_prefix.py.snap`:55 on 2025-07-21 16:52_

Not sure for `ft` but I would ignore the binary prefix for the second case. 

My concern with the current solution is that ignoring both prefixes is the least correct. Ignoring just one would be closer to what the user intended. 

---

_@dylwil3 reviewed on 2025-07-21 16:56_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__repeated_string_prefix_error.snap`:5 on 2025-07-21 16:56_

Hmm... what if instead we parsed the thing as a string, like we're currently doing, but then expanded the possible prefixes to include invalid ones. So instead of the AST showing `prefix: Empty` it was like `prefix: InvalidPair(first,second)`? Then all the characters would be accounted for.

---

_@dylwil3 reviewed on 2025-07-21 17:00_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_string_prefix.py.snap`:55 on 2025-07-21 17:00_

fwiw in the current state of affairs we are parsing the full prefix as a `Name` and then the string literal as an ordinary string. So I feel like even if we parsed this as an ordinary string with an invalid prefix it would be an improvement.

but I'll try to think of a way to implement what you're suggesting!

---

_@dylwil3 reviewed on 2025-07-21 17:01_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__repeated_string_prefix_error.snap`:5 on 2025-07-21 17:01_

> I think we do run all token based rules.

oh interesting! I'll add a test fixture for some of those then

---

_@MichaReiser reviewed on 2025-07-21 17:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__repeated_string_prefix_error.snap`:5 on 2025-07-21 17:02_

That's certainly worth exploring. One challenge is that the data needs to fit into `TokenFlags`, which is a `u8`. The other, that you still need to pick a `TokenKind`. 

The last challenge is that it requires designing how the different `StringFlags` represent this new state (most of those currently assert that the flags are valid). 

I'm not sure I can help you much with this right now as it seems fairly low priority (while a nice improvement). 

For now, I suggest making a local improvement (which might be to go with `Unknown`)

---

_Comment by @MichaReiser on 2025-08-18 07:38_

@dylwil3 what's the status of this PR?

---

_Converted to draft by @dylwil3 on 2025-08-18 12:26_

---

_Comment by @dylwil3 on 2025-08-18 12:26_

I'll revert to draft and re-open when I get a chance to visit it again - apologies for cluttering the open PRs!

---

_Comment by @dylwil3 on 2025-12-10 18:02_

Gonna close this since I won't be able to get back to it until higher priority items are cleared - which may not be soon. Others are welcome to pick it up!

---

_Closed by @dylwil3 on 2025-12-10 18:02_

---
