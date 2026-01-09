---
number: 25
title: Implement autofix
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-08-24T13:17:12Z
updated_at: 2022-09-06T14:15:18Z
url: https://github.com/astral-sh/ruff/issues/25
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement autofix

---

_Issue opened by @charliermarsh on 2022-08-24 13:17_

What's the right strategy for autofixing? Should we modify the AST? Should we use the generated rules, and modify via regex? (This is what [autoflake](https://github.com/PyCQA/autoflake) does, and there's a start on implementing autofix for F401 in #34.)

---

_Renamed from "Add autofix for f-string rule" to "Implement autofix" by @charliermarsh on 2022-08-28 13:55_

---

_Label `enhancement` added by @charliermarsh on 2022-08-28 13:55_

---

_Comment by @charliermarsh on 2022-08-28 14:01_

Should look at how [black](https://github.com/psf/black) works, and how [eslint](https://github.com/eslint/eslint) does autofixing.

---

_Comment by @Bobronium on 2022-08-31 00:39_

Here is a bunch of tools that I use for autofixing the code. Might be helpfull for both reference and ideas to carry into ruff.

[Fixit](https://github.com/Instagram/Fixit) — "writes better Python code for you"
[pyupgrade](https://github.com/asottile/pyupgrade) — "upgrades syntax for newer versions of the language"
[unimport](https://github.com/hakancelikdev/unimport) — "finding and removing unused import statements"
[pycln](https://github.com/hadialqattan/pycln) — "finding and removing unused import statements"
[flynt](https://pypi.org/project/flynt) — "string formatting converter"
[unify](https://github.com/myint/unify) — "modifies strings to all use the same quote where possible"

---

_Comment by @charliermarsh on 2022-08-31 00:54_

Woah, via Fixit I found [LibCST](https://github.com/Instagram/LibCST), which creates something closer to a concrete syntax tree -- and the parser is [written in Rust](https://github.com/Instagram/LibCST/tree/main/native/libcst)...

---

_Comment by @charliermarsh on 2022-08-31 02:07_

Started poking around at [`charlie/cst`](https://github.com/charliermarsh/ruff/tree/charlie/cst). The CST is much more complicated than the AST, so the visitor code will require more boilerplate.

---

_Comment by @charliermarsh on 2022-08-31 02:52_

Using CST as the primary parser (in lieu of RustPython) is probably too slow, though. The parsing alone is about 10x slower:

```
Benchmark 1: target/release/ruff ./resources/test/cpython/ --no-cache
  Time (mean ± σ):      2.368 s ±  0.947 s    [User: 15.231 s, System: 4.569 s]
  Range (min … max):    1.472 s …  3.896 s    10 runs
```

(This is benchmarked without iterating over the tree -- just the parser. It takes ~250ms with RustPython.)

Instead, perhaps would need to parse with CST only when attempting to autofix? Or maybe need to resort to simpler techniques, like regexes atop the AST.


---

_Comment by @charliermarsh on 2022-09-01 02:18_

Migrating to CST could be viewed as:

- Adding the `CSTTypedVisitorFunctions` interface
- Implement `_visit_and_replace_children` for all nodes, plus all the visitor helpers like `visit_iterable`

LibCST is very powerful so it's worth exploring. If we adhere closely to their API, we can also get all of the `fixit` lint rules out of the box _and_ trivially support autofix. The CST would be a super powerful foundation.


---

_Comment by @charliermarsh on 2022-09-01 02:22_

Interestingly, it looks like `Fixit` will lint code repeatedly, applying one autofix at a time, then re-linting, applying the next fix, and so on. I wonder if that's strictly necessary 🤔 

---

_Referenced in [astral-sh/ruff#78](../../astral-sh/ruff/issues/78.md) on 2022-09-01 16:54_

---

_Comment by @charliermarsh on 2022-09-03 16:58_

Here's my working summary of how other tools handle this behavior.

### Flynt

1. Iterate over the tokens in the source code, finding complete chunks.
2. Pass each complete chunk to a method that parses the AST and runs a custom `ast.NodeTransformer` on the node.
3. If the node is changed by the transformer, covert it back to code via [astor](https://pypi.org/project/astor/).
4. Build up the new representation of the code by concatenating the transformed and untransformed chunks as you go.

The things ruff are missing: `NodeTransformer` abstraction, AST-to-code generation (something like astor).

### Fixit

1. Run the Fixit linter over the file.
2. Look for "fixable" issues.
3. Apply the first fixable issue.
4. Repeat up to ~100 iterations.

To support the actual autofixing, rules can set a `replacement_node` property when they run. On the Python side, they also implement a codegen API for every CST node. (I'm a little confused because they implement effectively the same interface in `LibCST/native` on the rust side. I'm not sure if they're codegening the Python codegen code or just maintaining both implementations.)

### Autoflake

1. Run Pyflakes the file.
2. Extract the line numbers for the errors it can fix.
3. Iterate over the code line-by-line, fixing each error individually. (The fixing is all regex-based, with some additional bookkeeping to support multiline fixes. Whenever they remove code, they add a `pass` statement, and they do a second pass to remove unnecessary `pass` statements by iterating over the `tokenize.generate_tokens` representation for each line.)
4. Repeat until the file stops changing.

ruff could do this, it's just a matter of implementing the same logic.

### ESLint

- When rules report an error, they can provide a "fix", which is a function that takes a `fixer` and modifies the code in some way. The `fixer` has methods like `insertTextBeforeRange`, `remove(nodeOrToken)`, etc. The `fixer` really just takes those commands and generates a "fix", which is a source text range and a replacement snippet.
- When they go to apply the fixes, they re-build the source code by cutting the original code, adding in the next fix, and continuing where they left off when they move on to a new fix. They ignore any fixes that have conflicting ranges (so it's just "best effort").

ruff could do this... but our AST isn't as good. We don't always have accurate start and end ranges for our nodes, and we don't have a way to go from AST node to token.


---

_Comment by @charliermarsh on 2022-09-03 17:09_

I think the ESLint approach is the most likely to be performant, since we can continue to use the existing AST (faster parser than LibCST), and we can generate fixes at lint-time (so we don't need to re-lint the file or regenerate the AST or anything). It's also very tractable. It's not going to be as powerful as Fixit/LibCST though.

Separately, I think the Fixit API is a bit limiting, because you're required to do node replacement at visit-time. I don't think you can support things like "automatically removing unused imports" with this strategy, because you can't detect unused imports until you've traversed the entire AST. (So, when you see an "import" node, you can't _know_ that you have to delete it.)


---

_Comment by @charliermarsh on 2022-09-04 18:15_

Saving for later in case relevant / useful: logic from [pycodestyle](https://github.com/PyCQA/pycodestyle/blob/6cddabcb0a2f301441731fba23f655563ea0aba9/pycodestyle.py#L2190) to generate logical lines from tokens.

---

_Comment by @charliermarsh on 2022-09-06 14:15_

Initial autofix API is implemented for the refactoring rules (R001, R002).

---

_Closed by @charliermarsh on 2022-09-06 14:15_

---
