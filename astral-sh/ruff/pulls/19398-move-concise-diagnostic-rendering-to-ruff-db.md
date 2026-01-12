```yaml
number: 19398
title: "Move concise diagnostic rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: brent/concise
created_at: 2025-07-17T13:13:02Z
updated_at: 2025-07-23T15:43:35Z
url: https://github.com/astral-sh/ruff/pull/19398
synced_at: 2026-01-12T15:56:38Z
```

# Move concise diagnostic rendering to `ruff_db`

---

_@ntBre_

## Summary

This PR moves most of the work of rendering concise diagnostics in Ruff into `ruff_db`, where the code is shared with ty. To accomplish this without breaking backwards compatibility in Ruff, there are two main changes on the `ruff_db`/ty side:
- Added the logic from Ruff for remapping notebook line numbers to cells
- Reordered the fields in the diagnostic to match Ruff and rustc
  ```text
  # old
  error[invalid-assignment] try.py:3:1: Object of type `Literal[1]` is not assignable to `str`
  # new
  try.py:3:1: error[invalid-assignment]: Object of type `Literal[1]` is not assignable to `str`
  ```

I don't think the notebook change failed any tests on its own, and only a handful of snaphots changed in ty after reordering the fields, but this will obviously affect any other uses of the concise format, outside of tests, too.

The other big change should only affect Ruff:

- Added three new `DisplayDiagnosticConfig` options
  Micha and I hoped that we could get by with one option (`hide_severity`), but Ruff also toggles `show_fix_status` itself, independently (there are cases where we want neither severity nor the fix status), and during the implementation I realized we also needed access to an `Applicability`. The main goal here is to suppress the severity (`error` above) because ruff only uses the `error` severity and to use the secondary/noqa code instead of the line name (`invalid-assignment` above). 
  ```text
  # ty - same as "new" above
  try.py:3:1: error[invalid-assignment]: Object of type `Literal[1]` is not assignable to `str`
  # ruff
  try.py:3:1: RUF123 [*] Object of type `Literal[1]` is not assignable to `str`
  ```

This part of the concise diagnostic is actually shared with the `full` output format in Ruff, but with the settings above, there are no snapshot changes to either format.

## Test Plan

Existing tests with the handful of updates mentioned above, as well as some new tests in the `concise` module.

Also this PR. Swapping the fields might have broken mypy_primer, unless it occasionally times out on its own.

I also ran this script in the root of my Ruff checkout, which also has CPython in it:

```shell
flags=(--isolated --no-cache --no-respect-gitignore --output-format concise .)
diff <(target/release/ruff check ${flags[@]} 2> /dev/null) \
     <(ruff check ${flags[@]} 2> /dev/null)
```

This yielded an expected diff due to some t-string error changes on main since 0.12.4:
```diff
33622c33622
< crates/ruff_python_parser/resources/inline/err/f_string_lambda_without_parentheses.py:1:15: SyntaxError: Expected an element of or the end of the f-string
---
> crates/ruff_python_parser/resources/inline/err/f_string_lambda_without_parentheses.py:1:15: SyntaxError: Expected an f-string or t-string element or the end of the f-string or t-string
33742c33742
< crates/ruff_python_parser/resources/inline/err/implicitly_concatenated_unterminated_string_multiline.py:4:1: SyntaxError: Expected an element of or the end of the f-string
---
> crates/ruff_python_parser/resources/inline/err/implicitly_concatenated_unterminated_string_multiline.py:4:1: SyntaxError: Expected an f-string or t-string element or the end of the f-string or t-string
34131c34131
< crates/ruff_python_parser/resources/inline/err/t_string_lambda_without_parentheses.py:2:15: SyntaxError: Expected an element of or the end of the t-string
---
> crates/ruff_python_parser/resources/inline/err/t_string_lambda_without_parentheses.py:2:15: SyntaxError: Expected an f-string or t-string element or the end of the f-string or t-string
```

So modulo color, the results are identical on 38,186 errors in our test suite and CPython 3.10.

---

_Label `internal` added by @ntBre on 2025-07-17 13:13_

---

_Label `diagnostics` added by @ntBre on 2025-07-17 13:13_

---

_Comment by @github-actions[bot] on 2025-07-17 13:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-07-21 16:54_

I think this is ready for a first review. The preview changes in the ecosystem are expected, but maybe undesirable. A couple of them look useful, but I'm definitely open to suppressing this for now if we want to try to make some of the fix suggestions less redundant with the main message before exposing them to users in this format.

---

_Marked ready for review by @ntBre on 2025-07-21 16:56_

---

_Review requested from @carljm by @ntBre on 2025-07-21 16:56_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-21 16:56_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-21 16:56_

---

_Review requested from @sharkdp by @ntBre on 2025-07-21 16:56_

---

_Review requested from @dcreager by @ntBre on 2025-07-21 16:56_

---

_Renamed from "[prototype] Move concise diagnostic rendering to `ruff_db`" to "Move concise diagnostic rendering to `ruff_db`" by @ntBre on 2025-07-21 16:56_

---

_Review request for @dcreager removed by @ntBre on 2025-07-21 16:56_

---

_Review request for @carljm removed by @ntBre on 2025-07-21 16:56_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-21 16:56_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-21 16:56_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-21 16:56_

---

_Comment by @MichaReiser on 2025-07-21 17:03_

I'll take a closer look tomorrow and try to come up with a suggestion. I don't think we can keep the rendering as is. The messages contain too much redundant information now

---

_Comment by @MichaReiser on 2025-07-21 21:07_

Isn't your other PR addressing the difference in rendering (by moving the help text?)

---

_Comment by @ntBre on 2025-07-21 21:30_

> Isn't your other PR addressing the difference in rendering (by moving the help text?)

Oh yeah, I think you're right. I was only thinking about the `full` diagnostic, but that should resolve this as well. We should just be able to use `concise_message` here again.

---

_Comment by @MichaReiser on 2025-07-22 06:14_

I'll review this after rebasing ontop of your ohter PR. That will make it easier for me to understand the scope of the changes. 

---

_@MichaReiser approved on 2025-07-22 14:34_

---

_Comment by @ntBre on 2025-07-22 15:37_

As discussed on Discord, we should now match Ruff's concise styling exactly. I was hoping to get a clean diff from this command:

```shell
diff <(CLICOLOR_FORCE=1 ruff check . --output-format concise --no-cache) \
     <(CLICOLOR_FORCE=1 ~/astral/ruff/target/debug/ruff check . --output-format concise --no-cache)
```

but I think `colored` and `anstyle` output slightly different escape codes. This is the remaining diff, but I think both of these should render the same way:

<img width="312" height="99" alt="image" src="https://github.com/user-attachments/assets/df4808b6-06b1-4620-8054-0bc92b03ddff" />

<img width="565" height="104" alt="image" src="https://github.com/user-attachments/assets/e8380cb4-411b-4ffc-b9f3-aa8b7d54a9d4" />

I did have to add some new `Style`s to the `Stylesheet` since Ruff was using non-bold cyan and bold red instead of bold bright red.

---

_Label `ecosystem-analyzer` added by @ntBre on 2025-07-22 16:05_

---

_@BurntSushi approved on 2025-07-22 16:09_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-22 18:56_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-22 18:56_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-22 19:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-22 19:43_

---

_Comment by @github-actions[bot] on 2025-07-22 19:52_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-import` | 134 | 134 | 0 |
| `invalid-argument-type` | 5 | 5 | 0 |
| `invalid-assignment` | 4 | 4 | 0 |
| `possibly-unresolved-reference` | 4 | 4 | 0 |
| `possibly-unbound-attribute` | 1 | 1 | 0 |
| `unresolved-reference` | 1 | 1 | 0 |
| **Total** | **149** | **149** | **0** |

**[Full report with detailed diff](https://brent-concise.ecosystem-663.pages.dev/diff)**


---

_Merged by @ntBre on 2025-07-23 15:43_

---

_Closed by @ntBre on 2025-07-23 15:43_

---

_Branch deleted on 2025-07-23 15:43_

---
