```yaml
number: 17191
title: Quadratic formatting cost for many comments between two lines
type: issue
state: open
author: hauntsaninja
labels:
  - performance
  - formatter
  - help wanted
assignees: []
created_at: 2025-04-03T23:56:37Z
updated_at: 2025-04-05T07:24:27Z
url: https://github.com/astral-sh/ruff/issues/17191
synced_at: 2026-01-12T15:54:55Z
```

# Quadratic formatting cost for many comments between two lines

---

_@hauntsaninja_

### Summary

See for example:

```
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 1000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.04s user 0.02s system 100% cpu 0.066 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  0.07s user 0.00s system 55% cpu 0.133 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 2000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.04s user 0.03s system 100% cpu 0.068 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  0.27s user 0.01s system 82% cpu 0.336 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 3000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.03s user 0.03s system 99% cpu 0.068 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  0.60s user 0.01s system 90% cpu 0.670 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 4000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.04s user 0.03s system 99% cpu 0.068 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  1.06s user 0.01s system 94% cpu 1.129 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 5000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.03s user 0.04s system 100% cpu 0.067 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  1.67s user 0.01s system 96% cpu 1.731 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 6000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.03s user 0.04s system 99% cpu 0.068 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  2.47s user 0.01s system 97% cpu 2.538 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 7000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.04s user 0.03s system 99% cpu 0.069 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  3.25s user 0.00s system 98% cpu 3.318 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 8000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.03s user 0.04s system 100% cpu 0.068 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  4.24s user 0.01s system 98% cpu 4.309 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 9000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.03s user 0.04s system 99% cpu 0.069 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  5.37s user 0.01s system 98% cpu 5.436 total
λ time uvx --from pypyp pyp '"apply(x)\n" + ("# comment comment comment\n") * 10000 + "apply(x)"' | ruff format --stdin-filename foo --line-length=100 > /dev/null
uvx --from pypyp pyp   0.04s user 0.03s system 99% cpu 0.068 total
ruff format --stdin-filename foo --line-length=100 > /dev/null  6.70s user 0.01s system 99% cpu 6.764 total
```

### Version

ruff 0.9.10

---

_Comment by @ntBre on 2025-04-04 01:20_

```shell
awk '
  /\* [0-9]+/ {printf "%s", $14}
  $1 ~ /ruff/ {printf " %s\n", $(NF-1)}' 17191.dat | 
gnuplot -e 'plot "-" with lines' -p
```

![Image](https://github.com/user-attachments/assets/1cae7b90-3d06-4f6d-9917-4e3b64e0f9be)

Yep looks quadratic. Thanks!

---

_Label `formatter` added by @ntBre on 2025-04-04 01:21_

---

_Label `performance` added by @ntBre on 2025-04-04 01:21_

---

_Comment by @MichaReiser on 2025-04-04 06:24_

Thanks for reporting. You must have very well documented code :) 

I recorded a profile; you can see it [here](https://share.firefox.dev/426IgrG), and it's entirely different from what I thought it would be. 

The problem is one of those two tokenization calls, which makes sense, because we end up relexing the same tokens over and over again. 



---

_Comment by @MichaReiser on 2025-04-04 06:43_

The main issue is that we keep re-lexing all preceding comments everywhere where we have `SimpleTokenizer::new(before_comment, comment).skip_trivia()` because we need to figure out if there's any non-trivia content between the two positions. I suspect we would have to make the comments visitor more stateful and track:

* the end offset to where we checked for non-trivia tokens
* what the kind of token is that we saw in this range
* reset that state after every node 



---

_Label `help wanted` added by @MichaReiser on 2025-04-04 06:44_

---

_Comment by @dhruvmanila on 2025-04-04 13:06_

Could we pass in the `parsed.tokens()` and use that when extracting the comments data or is the cost of doing multiple binary searches greater than just re-lexing it? We could even create a `CachedTokens` data structure that remembers each access point for the token methods `after`, `before` (similar to preceeding), etc.

https://github.com/astral-sh/ruff/blob/64e7e1aa648625359fdbdcaf5fe055c34ff5a783/crates/ruff_python_formatter/src/lib.rs#L127-L128

---

_Comment by @MichaReiser on 2025-04-04 13:46_

I don't think tokens helps. At least, it doesn't change the complexity to be linear. The problem is that we keep scanning `previous_statement.end()..current_comment.start()`. Using `tokens` might (or might not) be cheaper but it still performs the same scanning over and over again.

The solution reduces the window that we scan from `previous_statement.end()` to `previous_statement_or_comment.end()..current_comment.start()`, making it linear

---

_Comment by @MichaReiser on 2025-04-05 07:24_

Another, statement specific, solution is to skip some of those operations when the preceding and following nodes are both statements. This at least works at least for where we handle parenthesized nodes because two statements can never have parentheses in-between. This might even be possible when the enclosing node is parenthesized. But I'm not a 100%. 

The downside of this approach is that it doesn't fix the exponential runtime for a large number of comments between two expressions

---
