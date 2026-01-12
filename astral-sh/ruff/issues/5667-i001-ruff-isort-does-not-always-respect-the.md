```yaml
number: 5667
title: "I001: ruff isort does not always respect the global src option"
type: issue
state: open
author: saippuakauppias
labels:
  - isort
  - needs-decision
assignees: []
created_at: 2023-07-10T23:08:38Z
updated_at: 2024-10-08T11:55:22Z
url: https://github.com/astral-sh/ruff/issues/5667
synced_at: 2026-01-12T15:54:45Z
```

# I001: ruff isort does not always respect the global src option

---

_@saippuakauppias_

I have a slightly odd bug that I don't understand how to fix yet. In a large project (django-service), there is a `statistics` application that overrides an builtin module with the same name.

When I run linter with rule `I001` - it offers to fix the imports so that imports from `statistics` go to the top (imports from the standard library). That is, the rule settings do not always take into account what is specified in `[tool.ruff].src`. And on the project this is the only case with incorrect processing, everything else works properly (I do not have separate settings for the linter isort in `pyproject.toml`).

What it looks like:
<img width="235" alt="image" src="https://github.com/astral-sh/ruff/assets/945306/3521edb2-817c-4e91-88f3-6cc504fce1ae">

I use ruff 0.0.270.

### File contents:
*pyproject.toml*
```toml
[tool.ruff]
target-version = "py311"
src = ["src"]
```

*src/foobar/bar.py*
```python
class B:
    ...
```

*src/foobar/baz.py*
```python
from logging import getLogger

from foobar.baz import B
from statistics.some import A

logger = getLogger('stream.logger')


a = A()
b = B()
logger.info('%s %s', a, b)
```

*src/statistics/some.py*
```python
class A:
    ...
```

### If I run linter, it will
```shell
ruff --config=pyproject.toml src --select=I001
src/foobar/baz.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

### If I run linter with autocorrect, it will change the file `src/foobar/baz.py` as follows
```python
from logging import getLogger
from statistics.some import A

from foobar.baz import B

logger = getLogger('stream.logger')


a = A()
b = B()
logger.info('%s %s', a, b)
```

---

Changing the application name is very problematic as it would entail a lot of refactoring. I would like the linter to take into account the settings from `[tool.ruff].src` (taking the list of packages from the directories that are listed there) and handle this case correctly. Or I would be grateful for your advice on how to fix it in the most painless way. 

PS: yes, I realize that such an application name makes it inadmissible to import functions from the standard library, but specifically in my case - it seems that it can be neglected.

---

_Comment by @charliermarsh on 2023-07-10 23:12_

I suspect we’re just checking if a module is in the standard library prior to checking if it’s available locally. I need to look at how other projects treat this case.

---

_Comment by @saippuakauppias on 2023-07-10 23:21_

If anything - I've noticed this incorrect behavior in PyCharm. 

In PyCharm I use imports sorting when applying the "Optimize imports" action. In the IDE I have
- "Source folders" are set correctly (Preferences -> Project -> Project Structure) - the "src" directory from my example is selected as "Source folders".
- I have these imports settings (Preferences -> Editor -> Code Style -> Python -> Imports):
<img width="679" alt="image" src="https://github.com/astral-sh/ruff/assets/945306/d51400eb-fc1d-40bf-83fb-9705967cdfe4">

- And I have these settings (Preferences -> Editor -> Code Style -> Python -> Wrapping and Braces):
<img width="679" alt="image" src="https://github.com/astral-sh/ruff/assets/945306/3c776e37-8550-4437-b72a-b433b879da8d">


I would be very grateful if you could help figure out how to solve this or fix the behavior in linter (thank you for such a quick response, once again I'm amazed how you always keep things under control!).

---

_Comment by @saippuakauppias on 2023-07-11 08:22_

I meant that in PyCharm imports are sorted correctly (package `statistics` is not in the imports of the standard library, but in the imports block of the project).

Simply if I use "Optimize Imports" in PyCharm - after that ruff breaks, sorting of imports between them is not compatible in terms of the location of this package.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-11 15:16_

---

_Label `isort` added by @charliermarsh on 2023-07-11 15:16_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-11 15:16_

---

_Comment by @Stealthii on 2023-11-02 23:18_

This can be easily demonstrated with a namespaced package where the name masks a globally available package:

```python
### mycompany/myproject/httpx/__init__.py

import httpx  # Treated as an internal import with ruff isort rules
```

The upstream `isort` tool does not exhibit this behaviour.

---

_Comment by @charliermarsh on 2023-11-02 23:28_

Whether or not that example is correct depends on your configuration and the overall structure of the project. Can you share your configuration? Why would httpx not be considered a first-party import there?

---

_Comment by @Stealthii on 2023-11-03 00:15_

You're right. In that example, under default configuration, ruff is considering the package as "httpx", as namespace packages haven't been configured or defined in ruff config.

There's two options for avoiding httpx being treated as a first-party import in this case:
```toml
[tool.ruff.isort]
# stop automatically marking imports from within the same package as first-party (masking the issue)
detect-same-package = false
```
```toml
[tool.ruff]
# Mark the directory as a namespace package
namespace-packages = ["mycompany/myproject/httpx"]
```

---

_Comment by @souliane on 2024-10-08 11:54_

I had a similar issue where my `tests` module was considered third party when running on GitLab CI [1].
`tests` is already listed in the `src` option.

I didn't manage to fix it with `detect-same-package` or `namespace-packages`, but using [known-first-party](https://docs.astral.sh/ruff/settings/#lint_isort_known-first-party).


[1] On my local machine, with the same ruff version and a clean env, I was not able to reproduce... I didn't find any explanation, but this is more likely not a ruff issue.

---
