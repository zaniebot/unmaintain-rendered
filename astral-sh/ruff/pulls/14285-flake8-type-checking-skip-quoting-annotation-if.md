```yaml
number: 14285
title: "[`flake8-type-checking`] Skip quoting annotation if it becomes invalid syntax (`TCH001`)"
type: pull_request
state: merged
author: Glyphack
labels:
  - bug
assignees: []
merged: true
base: main
head: linter-tch-autofix-quotations
created_at: 2024-11-11T18:48:05Z
updated_at: 2024-11-15T13:28:54Z
url: https://github.com/astral-sh/ruff/pull/14285
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-type-checking`] Skip quoting annotation if it becomes invalid syntax (`TCH001`)

---

_Pull request opened by @Glyphack on 2024-11-11 18:48_

Fix: #13934 

## Summary

Current implementation has a bug when the current annotation contains a string with single and double quotes.

TL;DR: I think these cases happen less than other use cases of Literal. So instead of fixing them we skip the fix in those cases.

One of the problematic cases:

```
from typing import Literal
from third_party import Type

def error(self, type1: Type[Literal["'"]]):
    pass
```

The outcome is:

```
- def error(self, type1: Type[Literal["'"]]):
+ def error(self, type1: "Type[Literal[''']]"):
```

While it should be:

```
"Type[Literal['\'']"
```

The solution in this case is that we check if there’s any quotes same as the quote style we want to use for this Literal parameter then escape that same quote used in the string.

Also this case is not uncommon to have: <https://grep.app/search?current=2&q=Literal["'>

But this can get more complicated for example in case of:

```
- def error(self, type1: Type[Literal["\'"]]):
+ def error(self, type1: "Type[Literal[''']]"):
```

Here we escaped the inner quote but in the generated annotation it gets removed. Then we flip the quote style of the Literal paramter and the formatting is wrong.

In this case the solution is more complicated.
1. When generating the string of the source code preserve the backslash.
2. After we have the annotation check if there isn’t any escaped quote of the same type we want to use for the Literal parameter. In this case check if we have any `’` without `\` before them. This can get more complicated since there can be multiple backslashes so checking for only `\’` won’t be enough.

Another problem is when the string contains `\n`. In case of `Type[Literal["\n"]]` we generate `'Type[Literal["\n"]]'` and both pyright and mypy reject this annotation.
https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUAMaXkDOjUAKoiQNqsC6AXFAB0w6tQAmJYLBKMYAfQCOAVzCk5tMChjlUjOQCNytANaMGjABYAKRiUrAANLA4BGAQHJ2CLkVIVKnABEADoogTw87gCUfNRQ8VAITIyiElKksooqahpaOih6hiZmTNa29k7w3m5sHJy%2BZFRBoeE8MXEJScxAA

## Test Plan

I added test cases for the original code in the reported issue and two more cases for backslash and new line.

---

_Converted to draft by @Glyphack on 2024-11-11 18:49_

---

_Comment by @github-actions[bot] on 2024-11-11 19:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Glyphack on 2024-11-12 20:54_

Hi @dhruvmanila I spent some time on finding the issue and the fix and wrote it down below. I'm not sure if it's okay to skip fixing these tricky cases
but would be happy to find a better one with your guidance.
Let me know what you think.

---

_Comment by @dhruvmanila on 2024-11-14 05:32_

Hey, thanks for looking into this and opening this PR.

I think the solution for now would be to avoid providing a fix if the annotation contains the preferred quote (the one used in to surround the annotation in the fix) or if it contains any escape characters like a newline character. Although Pyright supports them, the initial implementation for string annotations in red knot will reject them (https://github.com/astral-sh/ruff/pull/14151). There's also this discussion about the different string kinds [here](https://discuss.python.org/t/exotic-kinds-of-string-annotations/50527).

I think that's what this PR is doing right now based on looking at the snapshot changes, right?

---

_Comment by @Glyphack on 2024-11-14 17:03_

Yes @dhruvmanila this will skip the fix for those scenarios. sorry the point was not clear in the description.

---

_Marked ready for review by @Glyphack on 2024-11-14 17:16_

---

_@dhruvmanila reviewed on 2024-11-15 05:43_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:403 on 2024-11-15 05:43_

Should we avoid traversing if we know that this is not possible? We could use `enter_node`:

https://github.com/astral-sh/ruff/blob/3210f1a23bfe42c5d58c609b602861062eeed2ad/crates/ruff_python_ast/src/visitor/source_order.rs#L14-L17

---

_@dhruvmanila reviewed on 2024-11-15 05:45_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:279 on 2024-11-15 05:45_

Can we update the function docs to state that this will return an `Err` in this case?

Additionally, it seems that the function docs is out of date with your previous PR adding support for the cases where it says "(This is currently unsupported.)". We can update that as well.

---

_@dhruvmanila reviewed on 2024-11-15 05:46_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/quote3.py`:64 on 2024-11-15 05:46_

nit: we don't enforce formatting on the test fixtures but it's still useful to have consistency
```suggestion
    def test_string_contains_opposite_quote_do_not_fix(self, type1: Type[Literal["'"]], type2: Type[Literal["\'"]]):
        pass


def f():
```

---

_@dhruvmanila approved on 2024-11-15 05:47_

Thanks for looking into this.

---

_Label `bug` added by @dhruvmanila on 2024-11-15 05:47_

---

_Renamed from "Skip auto quoting when result is invalid" to "[`flake8-type-checking`] Skip quoting annotation if it becomes invalid syntax (`TCH001`)" by @dhruvmanila on 2024-11-15 11:08_

---

_Comment by @dhruvmanila on 2024-11-15 11:09_

(I updated the PR to get it into the next release.)

---

_Merged by @dhruvmanila on 2024-11-15 11:11_

---

_Closed by @dhruvmanila on 2024-11-15 11:11_

---

_Branch deleted on 2024-11-15 13:28_

---
