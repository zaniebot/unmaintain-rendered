```yaml
number: 21074
title: "Expose some methods of `ruff_python_parser::Lexer`"
type: pull_request
state: closed
author: ShaharNaveh
labels: []
assignees: []
base: main
head: expose-lexer-state
created_at: 2025-10-25T14:39:28Z
updated_at: 2025-10-26T10:09:42Z
url: https://github.com/astral-sh/ruff/pull/21074
synced_at: 2026-01-12T15:57:15Z
```

# Expose some methods of `ruff_python_parser::Lexer`

---

_@ShaharNaveh_

While trying to lex an incomplete source code that comes from a lazy iterator or a [`BufRead`](https://doc.rust-lang.org/std/io/trait.BufRead.html), I had trouble with:
- knowing what is the offset of the last good token
- Configure the Lexer to start from the offset of the last good token

Unfortunately
https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/crates/ruff_python_parser/src/lexer.rs#L1823-L1824

doesn't let you do that.

---

Feel free to close this PR if this is an unwanted change

---

_Review requested from @MichaReiser by @ShaharNaveh on 2025-10-25 14:39_

---

_Review requested from @dhruvmanila by @ShaharNaveh on 2025-10-25 14:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:85 on 2025-10-25 14:55_

I'd be okay to have a function next to `lex_tokens` that also takes an offset but I rather keep the constructor `pub(crate)`

---

_@MichaReiser reviewed on 2025-10-25 14:56_

Can you tell me more about your use case? 

The lexer is very much tied to our parser and not really intended to be public API.

---

_Comment by @github-actions[bot] on 2025-10-25 14:57_

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

_Comment by @ShaharNaveh on 2025-10-25 17:01_

> Can you tell me more about your use case?

ofc:)

I'm trying to implement a Rust iterator that behaves like CPython’s internal, undocumented `_tokenize.TokenizerIter` class.
This class operates on any object that provides a `.readline()` method.

On each `__next__` call, it:
- May call `.readline()` on the given object, consuming it lazily as needed.
- Yields a tuple that includes (among other elements) the line and column numbers where the token starts and ends, which is why I need access to the `TextRange`.

Because the input is consumed lazily, I need to keep track of my current position (offset) in the source. I don’t want to re-tokenize everything from the beginning every time a new line is read.

For example, imagine the first line is:

```py
def foo():
```
After processing the `:` token, a syntax error occurs. To continue, I call `.readline()` on the buffer, which gives me:

```py
    pass
```
Now, my `source` looks like this:
```py
def foo():
    pass
```

I should be able to get the next token starting from where the `:` token ended (in this case the next token would be `Indent`, then `Pass`).

Here’s a short Python snippet that illustrates the behavior:
```py
import io
import _tokenize

buf = io.StringIO(
"""
def func():
  pass

-)( ERROR $&-

for i in range(1):
  pass
""")

try:
  for tup in _tokenize.TokenizerIter(buf.readline, extra_tokens=False):
    # (token numeric value, token value, (char_offset_start, line_start), (char_offset_end, line_end), current_line)
    # Token numeric val from: https://github.com/python/cpython/blob/ebf955df7a89ed0c7968f79faec1de49f61ed7cb/Lib/token.py#L7-L79
    print(tup)
except BaseException as err:
  print(f"{err=}")

print(f"{buf.read()=}") # Remaining buffer that wasn't touched.
```



---

_@ShaharNaveh reviewed on 2025-10-25 17:09_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_parser/src/lexer.rs`:85 on 2025-10-25 17:09_

np.But it does feel a bit redundant as it would have the same signature and would call `Lexer::new` under the hood. so we end up with two identical methods only that one of them is `pub` and the other is `pub(crate)`

I'm not even sure how to call it :sweat_smile: 


---

_Comment by @MichaReiser on 2025-10-25 18:02_

Thanks for the explanation.

I don't think `Lexer::new` starting from a given offset is what you want in that case. The issue with constructing a new Lexer is that the Lexer tracks a lot of internal state (the number of open parentheses, the f-string nesting, ...) that you lose when you throw away the old lexer and create a new instance. 

So what you really want is a way to update the underlying `String` (which will append new content) and then call `next_token` again. But I'm not even sure if that will work because the `Lexer` e.g. returns a `String` token even if it is unterminated. In that case, you'd have to check that the unterminated flag is set, then rewind the lexer to *before the string*. 



---

_Comment by @ShaharNaveh on 2025-10-25 18:27_

> Thanks for the explanation.
> 
> I don't think `Lexer::new` starting from a given offset is what you want in that case. The issue with constructing a new Lexer is that the Lexer tracks a lot of internal state (the number of open parentheses, the f-string nesting, ...) that you lose when you throw away the old lexer and create a new instance.

> So what you really want is a way to update the underlying `String` (which will append new content) and then call `next_token` again. But I'm not even sure if that will work because the `Lexer` e.g. returns a `String` token even if it is unterminated. In that case, you'd have to check that the unterminated flag is set, then rewind the lexer to _before the string_.

Oh, good to know.

So, if I understand it correctly:
There's no benefit for me to start the Lexer from a different offset, I could re-lex the entire `source` and grab the first token that has a larger offset from what I previously had.
Unless there's an API to adjust the cursor location of the Lexer that I don't see (even if there was, it will feel criminally wrong).

And for this PR, I'd need to make the following adjustments:

Add `TokenFlags` to https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/crates/ruff_python_parser/src/lib.rs#L74

And make this `pub`:
https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/crates/ruff_python_parser/src/lexer.rs#L134


---

_@ShaharNaveh reviewed on 2025-10-25 18:28_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_parser/src/lexer.rs`:85 on 2025-10-25 18:28_

After your explanation here: https://github.com/astral-sh/ruff/pull/21074#issuecomment-3446985878

There's no need to have this one public. will revert

---

_Review requested from @MichaReiser by @ShaharNaveh on 2025-10-25 18:53_

---

_Comment by @MichaReiser on 2025-10-25 21:32_


It's not clear to me why you need the current_* methos over just calling next token?

Adjusting the cursor Location has the same problem as creating a new lexer: it doesn't account for the internal state. 



---

_Comment by @ShaharNaveh on 2025-10-26 10:06_

@MichaReiser After your explanation of:

> I don't think Lexer::new starting from a given offset is what you want in that case. The issue with constructing a new Lexer is that the Lexer tracks a lot of internal state (the number of open parentheses, the f-string nesting, ...) that you lose when you throw away the old lexer and create a new instance.

It seems like `Lexer` isn't exactly what I need. And if I intend to reparse the whole `source` after each new addition to `source` then `ruff_python_parser::Parser` already gives me what I need.

tysm for the replies and explanations!

---

_Closed by @ShaharNaveh on 2025-10-26 10:06_

---

_Branch deleted on 2025-10-26 10:09_

---
