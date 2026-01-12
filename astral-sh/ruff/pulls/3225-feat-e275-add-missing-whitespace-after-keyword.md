```yaml
number: 3225
title: "feat(E275): add  Missing whitespace after keyword"
type: pull_request
state: merged
author: carlosmiei
labels:
  - rule
assignees: []
merged: true
base: main
head: e275
created_at: 2023-02-25T13:25:44Z
updated_at: 2023-02-26T21:36:05Z
url: https://github.com/astral-sh/ruff/pull/3225
synced_at: 2026-01-12T04:39:44Z
```

# feat(E275): add  Missing whitespace after keyword

---

_Pull request opened by @carlosmiei on 2023-02-25 13:25_

- add https://www.flake8rules.com/rules/E275.html

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/whitespace_around_keywords.rs`:84 on 2023-02-25 16:43_

Interestingly, `pycodestyle` does this a little differently -- can you help me understand how the implementation differs?

```
@register_check
def missing_whitespace_after_keyword(logical_line, tokens):
    r"""Keywords should be followed by whitespace.

    Okay: from foo import (bar, baz)
    E275: from foo import(bar, baz)
    E275: from importable.module import(bar, baz)
    E275: if(foo): bar
    """
    for tok0, tok1 in zip(tokens, tokens[1:]):
        # This must exclude the True/False/None singletons, which can
        # appear e.g. as "if x is None:", and async/await, which were
        # valid identifier names in old Python versions.
        if (tok0.end == tok1.start and
                keyword.iskeyword(tok0.string) and
                tok0.string not in SINGLETONS and
                tok0.string not in ('async', 'await') and
                not (tok0.string == 'except' and tok1.string == '*') and
                not (tok0.string == 'yield' and tok1.string == ')') and
                tok1.string not in ':\n'):
            yield tok0.end, "E275 missing whitespace after keyword"
```


---

_@charliermarsh reviewed on 2023-02-25 16:43_

---

_@charliermarsh reviewed on 2023-02-25 16:43_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/whitespace_around_keywords.rs`:84 on 2023-02-25 16:43_

I think the current version we have here would flag `if None:`, right?

---

_@carlosmiei reviewed on 2023-02-25 17:11_

---

_Review comment by @carlosmiei on `crates/ruff/src/rules/pycodestyle/rules/whitespace_around_keywords.rs`:84 on 2023-02-25 17:11_

@charliermarsh yes you're right, will try to improve this implementation. 

---

_Closed by @carlosmiei on 2023-02-25 17:11_

---

_@charliermarsh reviewed on 2023-02-25 17:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/whitespace_around_keywords.rs`:84 on 2023-02-25 17:23_

No prob! Most of the pycodestyle code thus far is nearly a 1:1 port of the original implementation, so you can probably check out their source for inspiration.

---

_Reopened by @carlosmiei on 2023-02-26 17:34_

---

_Comment by @carlosmiei on 2023-02-26 17:36_

@charliermarsh I have refactored it (following your suggestion), let me know if anything 🙂 

---

_@carlosmiei reviewed on 2023-02-26 17:38_

---

_Review comment by @carlosmiei on `crates/ruff/src/rules/pycodestyle/helpers.rs`:62 on 2023-02-26 17:38_

@charliermarsh I noticed that we have `KWLIST` but converting `Tok` to a string would require a hacky solution (they add single quotes to most tokens so we would need to remove them, eg `"')'"`) so I added these helper methods. 

---

_Review requested from @charliermarsh by @carlosmiei on 2023-02-26 17:55_

---

_Comment by @charliermarsh on 2023-02-26 19:16_

Thank you! Will review today.

---

_Label `rule` added by @charliermarsh on 2023-02-26 21:32_

---

_Merged by @charliermarsh on 2023-02-26 21:36_

---

_Closed by @charliermarsh on 2023-02-26 21:36_

---
