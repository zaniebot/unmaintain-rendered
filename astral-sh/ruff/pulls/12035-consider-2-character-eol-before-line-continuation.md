```yaml
number: 12035
title: Consider 2-character EOL before line continuation
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/line-continuation-eol
created_at: 2024-06-26T03:17:29Z
updated_at: 2024-06-26T08:30:48Z
url: https://github.com/astral-sh/ruff/pull/12035
synced_at: 2026-01-10T21:56:00Z
```

# Consider 2-character EOL before line continuation

---

_Pull request opened by @dhruvmanila on 2024-06-26 03:17_

## Summary

This PR fixes a bug introduced in https://github.com/astral-sh/ruff/pull/12008 which didn't consider the two character newline after the line continuation character.

For example, consider the following code highlighted with whitespaces:
```py
call(foo # comment \\r\n
\r\n
def bar():\r\n
....pass\r\n
```
The lexer is at `def` when it's running the re-lexing logic and trying to move back to a newline character. It encounters `\n` and it's being escaped (incorrect) but `\r` is being escaped, so it moves the lexer to `\n` character. This creates an overlap in token ranges which causes the panic.

```
Name 0..4
Lpar 4..5
Name 5..8
Comment 9..20
NonLogicalNewline 20..22 <-- overlap between
Newline 21..22           <-- these two tokens
NonLogicalNewline 22..23
Def 23..26
...
```

fixes: #12028 

## Test Plan

Add a test case with line continuation and windows style newline character.


---

_Label `bug` added by @dhruvmanila on 2024-06-26 03:17_

---

_Label `parser` added by @dhruvmanila on 2024-06-26 03:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-26 03:17_

---

_Comment by @github-actions[bot] on 2024-06-26 03:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila reviewed on 2024-06-26 04:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1408 on 2024-06-26 04:42_

The main change is to split the match with `\r` and `\n` into it's own branch and then a common check for line continuation character.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1408 on 2024-06-26 06:35_

I think we can simplify this logic a bit. It doesn't seem to matter how many new line characters there are, we just want to skip all of them


```rust
if matches!(c, '\n' | '\r') {
	while Some(newline) = reverse_chars.next_if(|c| matches!(c, '\n' | '\r')) {
		current_position -= newline.text_len();
	}

 // after any new line, handle line continuation
}
```

---

_@MichaReiser approved on 2024-06-26 06:39_

Looks good to me. But I agree with you that this is all backwards lexing again. Could we use the previous token kind (e.g. by storing it on the lexer) and test if it is a `NonLogicalNewline` to avoid all the complexity of guessing if something's a newline or not?

Or is this not sufficient if there are multiple newlines (in which case storing just one previous token isn't enough?)

I guess there's also:

```python
[a, 
	# comment
	# more comment

def foo
```

where there's more in between `def` and `,`

---

_Comment by @dhruvmanila on 2024-06-26 07:23_

> Looks good to me. But I agree with you that this is all backwards lexing again. Could we use the previous token kind (e.g. by storing it on the lexer) and test if it is a `NonLogicalNewline` to avoid all the complexity of guessing if something's a newline or not?
> 
> Or is this not sufficient if there are multiple newlines (in which case storing just one previous token isn't enough?)
> 
> I guess there's also:
> 
> ```python
> [a, 
> 	# comment
> 	# more comment
> 
> def foo
> ```
> 
> where there's more in between `def` and `,`

Yeah, that's correct. There can be multiple trivia tokens in between which makes it hard to just look at the last token.

Although we could split the logic and move "finding the non-logical newline" part to the `TokenSource` who has access to all the tokens and then it'll ask the lexer to move back to the provided location. I'm not sure if this is a good way to model this behavior.

---

_@dhruvmanila reviewed on 2024-06-26 08:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1408 on 2024-06-26 08:30_

I don't think this will work because we want the position of the last unescaped newline character. We would still need to keep track of that position, so we might as well be explicit about this.

---

_Merged by @dhruvmanila on 2024-06-26 08:30_

---

_Closed by @dhruvmanila on 2024-06-26 08:30_

---

_Branch deleted on 2024-06-26 08:30_

---
