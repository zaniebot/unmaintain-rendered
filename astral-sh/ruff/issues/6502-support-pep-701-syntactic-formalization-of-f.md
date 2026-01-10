```yaml
number: 6502
title: "Support PEP 701: Syntactic formalization of f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - core
  - parser
  - python312
assignees: []
created_at: 2023-08-11T13:40:54Z
updated_at: 2023-09-29T02:55:40Z
url: https://github.com/astral-sh/ruff/issues/6502
synced_at: 2026-01-10T11:09:48Z
```

# Support PEP 701: Syntactic formalization of f-strings

---

_Issue opened by @dhruvmanila on 2023-08-11 13:40_

[PEP 701] formalizes the grammar for f-strings. This will require updates to both the lexer and parser while keeping the AST representation the same.

* Update the lexer to emit the new tokens (`FSTRING_START`, `FSTRING_MIDDLE`, `FSTRING_END`). This means that `f"foo {bar}"` wouldn't be emitted as a single `String` token but rather something like:
	
	```
	FSTRING_START('f"')
	FSTRING_MIDDLE("foo ")
	LBRACE
	NAME("bar")
	RBRACE
	FSTRING_END('f"')
	```

* Update the parser to account for any grammar changes required to recognize these tokens and parse it into an equivalent AST nodes. There are few things to note here:
	* We can remove the f-string parsing done in the string parser (`string.rs`)
	* Debug expressions (`f"foo {bar = }"`) preserves whitespaces using the [`debug_text` field](https://github.com/astral-sh/ruff/blob/f2939c678bf79d3c1061caaa638e51bead48b9d9/crates/ruff_python_ast/src/nodes.rs#L833). Newlines needs to be preserved as well.
	* Implicit string concatenation
   * `SimpleTokenizer` changes

The goal is to complete this ~in the current iteration (August)~ before the official release of Python 3.12.

## Tasks

- [x] #7042	
- [x] #7043
- [x] #6363
- [x] #7299
- [x] #7517

## Reference implementation:
* CPython: [Tokenizer](https://github.com/python/cpython/blob/23a6db98f21cba3af69a921f01613bd5f602bf6d/Parser/tokenizer.c)
* LibCST: [Tokenizer](https://github.com/Instagram/LibCST/blob/9eab2f037fa3680e0627d23038457b65dbb4078f/native/libcst/src/tokenizer/core/mod.rs) | [Parser](https://github.com/Instagram/LibCST/blob/9eab2f037fa3680e0627d23038457b65dbb4078f/native/libcst/src/parser/grammar.rs#L1399-L1432)

[PEP 701]: https://peps.python.org/pep-0701

---

_Label `core` added by @dhruvmanila on 2023-08-11 13:40_

---

_Label `parser` added by @dhruvmanila on 2023-08-11 13:40_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-08-11 13:40_

---

_Label `python312` added by @zanieb on 2023-08-24 20:03_

---

_Renamed from "Add support for PEP 701: Syntactic formalization of f-strings" to "Support PEP 701: Syntactic formalization of f-strings" by @dhruvmanila on 2023-09-01 13:35_

---

_Comment by @dhruvmanila on 2023-09-14 01:59_

The way the PRs will be merged is as follows:
1. All changes will be made in separate pull request for separation in logic and code reviews
2. All the pull requests will be merged in a new branch `dhruv/pep-701`
3. A final pull request will be opened from `dhruv/pep-701` to `main` once changes are done.
4. Merge 🥳 

---

_Closed by @dhruvmanila on 2023-09-29 02:55_

---
