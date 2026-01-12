```yaml
number: 3046
title: "`E731` autofix broken for class vars"
type: issue
state: closed
author: JoshKarpel
labels:
  - bug
assignees: []
created_at: 2023-02-20T01:44:10Z
updated_at: 2023-02-20T17:38:43Z
url: https://github.com/astral-sh/ruff/issues/3046
synced_at: 2026-01-12T15:54:43Z
```

# `E731` autofix broken for class vars

---

_@JoshKarpel_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

(`E731` is "do not assign a `lambda` expression, use a `def`")

Consider this example where a `dataclass` has a `Callable` attribute with a default value:

```python
from collections.abc import Callable
from dataclasses import dataclass

@dataclass
class Foo:
    bar: Callable[[], str] = lambda: "foo"
```

Ruff identifies the class variable assignment as a violation of `E731`:

```console
$ ruff check test.py --isolated
test.py:7:5: E731 [*] Do not assign a `lambda` expression, use a `def`
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

But the autofix transforms it into:

```python
from dataclasses import dataclass

@dataclass
class Foo:
    def bar():
        return "foo"
```

which is not equivalent. 

Looking at https://github.com/charliermarsh/ruff/blob/9168a12679976ece5e94dd9cf6abe3c8886e31cc/crates/ruff/src/rules/pycodestyle/rules/lambda_assignment.rs#L29-L37 it seems like the problem is that there's no check on whether it makes sense to do this transformation in the surrounding context (in this case, the autofix doesn't apply because we're in a `@dataclass` body, but maybe there are other contexts too?). 

I think the desired autofix in this case would be more like:

```python
from collections.abc import Callable
from dataclasses import dataclass

def bar():
    return "foo"

@dataclass
class Foo:
    bar: Callable[[], str] = bar
```

but that is only correct for dataclasses because we care about the type-annotated class variable! If this wasn't a dataclass, the original autofix would be fine and produces equivalent behavior at runtime. I'd suggest that as a first pass, there's just no autofix for `E731` if it occurs in a class body, because it depends on this question of whether the type-annotated class variable needs to exist for some other code to use, which is probably impossible to determine (since it's not just `@dataclass` that might use those annotations).

Happy to take a look at a fix for this issue myself, but I'm a first-time contributor so it might take some time :)

---

_Comment by @charliermarsh on 2023-02-20 02:54_

Yeah this makes sense. We could look at `checker.current_scope` to see if it's of `ScopeKind::Class`. I'll get to it eventually but you're more than welcome and I can answer questions as they come up :)

---

_Label `bug` added by @charliermarsh on 2023-02-20 02:54_

---

_Comment by @JoshKarpel on 2023-02-20 03:45_

Oh nice, that sounds relatively straightforward! I will give it a shot.

---

_Closed by @charliermarsh on 2023-02-20 17:38_

---
