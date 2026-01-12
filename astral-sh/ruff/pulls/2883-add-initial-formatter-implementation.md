```yaml
number: 2883
title: Add initial formatter implementation
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/fork
created_at: 2023-02-14T03:17:57Z
updated_at: 2023-02-15T07:58:14Z
url: https://github.com/astral-sh/ruff/pull/2883
synced_at: 2026-01-12T15:55:11Z
```

# Add initial formatter implementation

---

_@charliermarsh_

# Summary

This PR contains the code for the autoformatter proof-of-concept.

## Crate structure

The primary formatting hook is the `fmt` function in `crates/ruff_python_formatter/src/lib.rs`.

The current formatter approach is outlined in `crates/ruff_python_formatter/src/lib.rs`, and is structured as follows:

- Tokenize the code using the RustPython lexer.
- In `crates/ruff_python_formatter/src/trivia.rs`, extract a variety of trivia tokens from the token stream. These include comments, trailing commas, and empty lines.
- Generate the AST via the RustPython parser.
- In `crates/ruff_python_formatter/src/cst.rs`, convert the AST to a CST structure. As of now, the CST is nearly identical to the AST, except that every node gets a `trivia` vector. But we might want to modify it further.
- In `crates/ruff_python_formatter/src/attachment.rs`, attach each trivia token to the corresponding CST node. The logic for this is mostly in `decorate_trivia` and is ported almost directly from Prettier (given each token, find its preceding, following, and enclosing nodes, then attach the token to the appropriate node in a second pass).
- In `crates/ruff_python_formatter/src/newlines.rs`, normalize newlines to match Black’s preferences. This involves traversing the CST and inserting or removing `TriviaToken` values as we go.
- Call `format!` on the CST, which delegates to type-specific formatter implementations (e.g., `crates/ruff_python_formatter/src/format/stmt.rs` for `Stmt` nodes, and similar for `Expr` nodes; the others are trivial). Those type-specific implementations delegate to kind-specific functions (e.g., `format_func_def`).

## Testing and iteration

The formatter is being developed against the Black test suite, which was copied over in-full to `crates/ruff_python_formatter/resources/test/fixtures/black`.

The Black fixtures had to be modified to create `[insta](https://github.com/mitsuhiko/insta)`-compatible snapshots, which now exist in the repo.

My approach thus far has been to try and improve coverage by tackling fixtures one-by-one.

## What works, and what doesn’t

- *Most* nodes are supported at a basic level (though there are a few stragglers at time of writing, like `StmtKind::Try`).
- Newlines are properly preserved in most cases.
- Magic trailing commas are properly preserved in some (but not all) cases.
- Trivial leading and trailing standalone comments mostly work (although maybe not at the end of a file).
- Inline comments, and comments within expressions, often don’t work -- they work in a few cases, but it’s one-off right now. (We’re probably associating them with the “right” nodes more often than we are actually rendering them in the right place.)
- We don’t properly normalize string quotes. (At present, we just repeat any constants verbatim.)
- We’re mishandling a bunch of wrapping cases (if we treat Black as the reference implementation). Here are a few examples (demonstrating Black's stable behavior):

```py
# In some cases, if the end expression is "self-closing" (functions,
# lists, dictionaries, sets, subscript accesses, and any length-two
# boolean operations that end in these elments), Black
# will wrap like this...
if some_expression and f(
    b,
    c,
    d,
):
    pass

# ...whereas we do this:
if (
    some_expression
    and f(
        b,
        c,
        d,
    )
):
    pass

# If function arguments can fit on a single line, then Black will
# format them like this, rather than exploding them vertically.
if f(
    a, b, c, d, e, f, g, ...
):
    pass
```

- We don’t properly preserve parentheses in all cases. Black preserves parentheses in some but not all cases.

---

_@charliermarsh reviewed on 2023-02-14 03:21_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/black/jupyter/non_python_notebook.ipynb`:1 on 2023-02-14 03:21_

Meant to remove most of these -- dropped one commit. Hold on...

---

_Label `autoformatter` added by @charliermarsh on 2023-02-14 03:27_

---

_Comment by @charliermarsh on 2023-02-14 03:34_

\cc @MichaReiser 

---

_Comment by @charliermarsh on 2023-02-14 04:33_

Need to find a way to ignore the "unused" snapshot files, which are intentionally unused.

---

_Comment by @not-my-profile on 2023-02-14 04:46_

Why are they intentionally unused? I assume you could change the file extension.

---

_Comment by @charliermarsh on 2023-02-14 22:15_

> Why are they intentionally unused? I assume you could change the file extension.

They're copied over from Black, so they represent the "desired" output (essentially, the spec). The formatter fails on a bunch of them right now, so only those that it knowingly passes are used, and we'll increase that set over time.


---

_Merged by @charliermarsh on 2023-02-15 04:06_

---

_Closed by @charliermarsh on 2023-02-15 04:06_

---

_Branch deleted on 2023-02-15 04:06_

---

_Comment by @not-my-profile on 2023-02-15 04:27_

> ruff_formatter and ruff_python_formatter

I find these crate names to be very confusing ... what is the difference?

---

_Comment by @charliermarsh on 2023-02-15 04:45_

The former is language-agnostic formatting infrastructure. The latter is a Python source code formatter.

---

_Comment by @not-my-profile on 2023-02-15 04:58_

Ah right looking at the [rome crates](https://github.com/rome/tools/tree/main/crates) I see that there are several language-specific `_formatter` crates, all depending on `rome_formatter`. Do you think there is another language ruff could end up being able to format?

---

_Comment by @MichaReiser on 2023-02-15 07:58_

> Ah right looking at the [rome crates](https://github.com/rome/tools/tree/main/crates?rgh-link-date=2023-02-15T04%3A58%3A04Z) I see that there are several language-specific `_formatter` crates, all depending on `rome_formatter`. Do you think there is another language ruff could end up being able to format?

I would find it interesting if ruff could format toml and JSON files, raw SQL statements in backend code, graphql queries, etc. to have the same experience as with Prettier. It doesn't just format your JavaScript code, but also other languages that are commonly embedded into JavaScript

---
