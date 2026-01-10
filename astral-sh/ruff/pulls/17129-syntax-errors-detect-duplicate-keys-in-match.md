```yaml
number: 17129
title: "[syntax-errors] Detect duplicate keys in `match` mapping patterns"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-duplicate-match-key
created_at: 2025-04-01T18:06:24Z
updated_at: 2025-04-03T14:26:10Z
url: https://github.com/astral-sh/ruff/pull/17129
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Detect duplicate keys in `match` mapping patterns

---

_Pull request opened by @ntBre on 2025-04-01 18:06_

Summary
--

Detects duplicate literals in `match` mapping keys.

This PR also adds a `source` method to `SemanticSyntaxContext` to display the duplicated key in the error message by slicing out its range.

Test Plan
--

New inline tests.

---

_Label `rule` added by @ntBre on 2025-04-01 18:06_

---

_Label `preview` added by @ntBre on 2025-04-01 18:06_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-01 18:06_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-01 18:06_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:288 on 2025-04-01 18:08_

We could be a bit stricter here if we want (checking that the `BinOp` is an `Add` and the arguments are numbers), but we already report syntax errors for non-complex literals like `1 + 2`, so I thought this might be sufficient.

---

_@ntBre reviewed on 2025-04-01 18:08_

---

_Comment by @github-actions[bot] on 2025-04-01 18:15_

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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:289 on 2025-04-01 20:06_

I think we should avoid breaking here and report all duplicate keys within a single mapping pattern. Or, do you see any limitation or challenges in that approach? What do you think?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:721 on 2025-04-01 20:07_

Interesting!

Also, I think you meant "python" or "py" and not "pycon" xD

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@duplicate_match_key.py.snap`:665 on 2025-04-01 20:11_

What happens when there are multiple different duplicate keys? Like `case {0: 1, "x": 1, 0: 2, "x": 2}: ...`.

I think we should highlight all the subsequent duplicate keys i.e., in the above case we should highlight the second `0` and the second `"x"` instance.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:288 on 2025-04-01 20:12_

Yeah, I think that's fine, the parser should catch invalid complex literals.

---

_@dhruvmanila reviewed on 2025-04-01 20:12_

Looks good, I'd suggest that we improve the diagnostic range and expand the check for all keys unless it creates an issue.

---

_@ntBre reviewed on 2025-04-01 20:14_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:289 on 2025-04-01 20:14_

I was just copying CPython, but that sounds reasonable too!

```pycon
>>> match x:
...     case {"x": 1, "x": 2}: ...
...
  File "<python-input-223>", line 2
    case {"x": 1, "x": 2}: ...
         ^^^^^^^^^^^^^^^^
SyntaxError: mapping pattern checks duplicate key ('x')
```

---

_@ntBre reviewed on 2025-04-01 20:17_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:721 on 2025-04-01 20:17_

Oh I thought `pycon` was for the Python console [[1]](https://github.com/github-linguist/linguist/blob/c27ac0c1daf3865e2b45ee3908d06b5825161d17/lib/linguist/languages.yml#L5892). That's what I use here on GitHub, although it doesn't usually do much syntax highlighting anyway.

---

_@ntBre reviewed on 2025-04-01 20:19_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@duplicate_match_key.py.snap`:665 on 2025-04-01 20:19_

Yeah CPython just notes the first one (also inline with the `break` above), but I think this is a good idea.

```pycon
>>> match x:
...     case {"x": 1, "x": 2, 0: 3, 0: 4}: ...
...
  File "<python-input-224>", line 2
    case {"x": 1, "x": 2, 0: 3, 0: 4}: ...
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: mapping pattern checks duplicate key ('x')
```

---

_@dhruvmanila reviewed on 2025-04-01 20:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:289 on 2025-04-01 20:25_

Yeah, in general, we should prefer to surface all errors whenever possible which is one of the main motivation to have an error resilient parser :)

---

_@dhruvmanila reviewed on 2025-04-01 20:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:721 on 2025-04-01 20:27_

Oh lol, I had no idea about that. I don't think that matters as those are going to be rendered in an editor or crates.io.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:288 on 2025-04-02 07:29_

We also have to allow f-string expressions. They're allowed for as long as they contain no placeholders and they should compare equal to their string equivalent (I think this is already handled by `ComparableExpr`). 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@duplicate_match_key.py.snap`:912 on 2025-04-02 07:32_

Another use case for multi-span diagostics :)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@duplicate_match_key.py.snap`:988 on 2025-04-02 07:34_

This will break our concise rendering where each message should only be a single line long. I don't have a good recommendation but it's a general concern with including source text as is in diagnostic messages. You can either replace new lines, truncate before the new line (and replace with `...`), or not include the name if it is multiline.

---

_@MichaReiser approved on 2025-04-02 07:36_

---

_@ntBre reviewed on 2025-04-02 14:31_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@duplicate_match_key.py.snap`:988 on 2025-04-02 14:31_

I went with escaping the newlines to match CPython (as we discussed on Discord):

```pycon
>>> match x:
...     case {
...     """x
...     y
...     z
...     """: 1,
...     """x
...     y
...     z
...     """: 2}: ...
...
  File "<python-input-0>", line 2
    case {
         ^
SyntaxError: mapping pattern checks duplicate key ('x\n    y\n    z\n    ')
```

I added a modified version of [`std::str::EscapeDefault`](https://doc.rust-lang.org/std/str/struct.EscapeDefault.html) from the Rust standard library. It's a little more heavily modified than I wanted because many of the functions in the real implementation are private, but it gets the job done and without too much code.

---

_@ntBre reviewed on 2025-04-02 14:34_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:288 on 2025-04-02 14:34_

I think f-strings are not allowed here by CPython, even without placeholders:

```pycon
>>> match x:
...     case {f"x": 1}: ...
...
  File "<python-input-2>", line 2
    case {f"x": 1}: ...
         ^^^^^^^^^
SyntaxError: mapping pattern keys may only match literals and attribute lookups
```

In that case, I think our `is_literal_expr` is doing the right thing.

---

_@dhruvmanila approved on 2025-04-02 14:37_

---

_Merged by @ntBre on 2025-04-03 14:22_

---

_Closed by @ntBre on 2025-04-03 14:22_

---

_Branch deleted on 2025-04-03 14:22_

---

_@dhruvmanila reviewed on 2025-04-03 14:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:288 on 2025-04-03 14:26_

Yeah, f-strings are not allowed as literal patterns.

---
