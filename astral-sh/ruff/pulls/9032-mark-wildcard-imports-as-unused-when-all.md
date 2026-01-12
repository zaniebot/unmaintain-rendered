```yaml
number: 9032
title: Mark wildcard imports as unused when all references are resolved
type: pull_request
state: closed
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
base: main
head: charlie/wildcard
created_at: 2023-12-07T01:58:33Z
updated_at: 2023-12-08T14:38:37Z
url: https://github.com/astral-sh/ruff/pull/9032
synced_at: 2026-01-12T15:55:27Z
```

# Mark wildcard imports as unused when all references are resolved

---

_@charliermarsh_

## Summary

If a file contains, e.g., `from foo import *`, but all references are resolved, we should mark the import as unused (similar to Flake8).

This change is gated behind preview for now.

Closes https://github.com/astral-sh/ruff/issues/9028.

---

_Label `rule` added by @charliermarsh on 2023-12-07 01:58_

---

_Label `preview` added by @charliermarsh on 2023-12-07 01:58_

---

_Marked ready for review by @charliermarsh on 2023-12-07 01:58_

---

_Comment by @MichaReiser on 2023-12-07 02:22_

Can you explain what it means that all imports are resolved? Does it mean we know what `foo` exports or is it that there are no unknown references in the file? We may want to be more explicit about the meaning in the documentation as well.

---

_Comment by @charliermarsh on 2023-12-07 02:26_

It's the latter -- just that there are no unknown references in the file. So it's relatively conservative.

---

_Comment by @MichaReiser on 2023-12-07 02:49_

> It's the latter -- just that there are no unknown references in the file. So it's relatively conservative.

Is this assumption safe if some imports are conditional or if there are code blocks between the imports?

```python
from "a" import b, c

print(b)

from foo import *

print(a) # we can resolve `a` but we don't know if `foo` rebound `a` 
```


---

_Comment by @charliermarsh on 2023-12-07 02:52_

It would definitely get the case you posted right, yes. Given our semantic model (where we basically assume all branches are traversed), I think it's more likely to have false negatives than false positives.


---

_Comment by @github-actions[bot] on 2023-12-07 02:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+23 -2 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/model/test_model.py#L23'>tests/unit/bokeh/model/test_model.py:23:1:</a> F401 [*] `bokeh.models.*` imported but unused
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/model/test_model.py#L25'>tests/unit/bokeh/model/test_model.py:25:1:</a> F401 [*] `bokeh.plotting.*` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/f03637b41549cfa891d867c0c6d91bc0622add15/mlflow/pytorch/pickle_module.py#L26'>mlflow/pytorch/pickle_module.py:26:1:</a> F401 [*] `cloudpickle.*` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/dd5f15a600a071ba00859e84fa497a9d1a25f521/pkg_resources/_vendor/more_itertools/__init__.pyi#L1'>pkg_resources/_vendor/more_itertools/__init__.pyi:1:1:</a> F401 [*] `.more.*` imported but unused
+ <a href='https://github.com/pypa/setuptools/blob/dd5f15a600a071ba00859e84fa497a9d1a25f521/pkg_resources/_vendor/more_itertools/__init__.pyi#L2'>pkg_resources/_vendor/more_itertools/__init__.pyi:2:1:</a> F401 [*] `.recipes.*` imported but unused
+ <a href='https://github.com/pypa/setuptools/blob/dd5f15a600a071ba00859e84fa497a9d1a25f521/setuptools/_vendor/more_itertools/__init__.pyi#L1'>setuptools/_vendor/more_itertools/__init__.pyi:1:1:</a> F401 [*] `.more.*` imported but unused
+ <a href='https://github.com/pypa/setuptools/blob/dd5f15a600a071ba00859e84fa497a9d1a25f521/setuptools/_vendor/more_itertools/__init__.pyi#L2'>setuptools/_vendor/more_itertools/__init__.pyi:2:1:</a> F401 [*] `.recipes.*` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/d6374ca3f0c1edb1090fd8bf5c16e998b39e1102/reflex/.templates/apps/sidebar/code/sidebar.py#L6'>reflex/.templates/apps/sidebar/code/sidebar.py:6:1:</a> F401 [*] `code.pages.*` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/1ed48ec932dfccb931fc26ae8c9a018339a3fb35/rotkehlchen/tests/conftest.py#L50'>rotkehlchen/tests/conftest.py:50:1:</a> F401 [*] `rotkehlchen.tests.fixtures.*` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+14 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/scripts/lib/pythonrc.py#L4'>scripts/lib/pythonrc.py:4:5:</a> F401 [*] `analytics.models.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/scripts/lib/pythonrc.py#L5'>scripts/lib/pythonrc.py:5:5:</a> F401 [*] `zerver.models.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/configured_settings.py#L10'>zproject/configured_settings.py:10:1:</a> F401 [*] `.default_settings.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/configured_settings.py#L20'>zproject/configured_settings.py:20:5:</a> F401 [*] `.prod_settings.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/configured_settings.py#L24'>zproject/configured_settings.py:24:5:</a> F401 [*] `.prod_settings_template.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/configured_settings.py#L25'>zproject/configured_settings.py:25:5:</a> F401 [*] `.dev_settings.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/configured_settings.py#L34'>zproject/configured_settings.py:34:13:</a> F401 [*] `zproject.custom_dev_settings.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/prod_settings.pyi#L1'>zproject/prod_settings.pyi:1:1:</a> F401 [*] `.prod_settings_template.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/settings.py#L1'>zproject/settings.py:1:1:</a> CPY001 Missing copyright notice at top of file
- <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/settings.py#L1'>zproject/settings.py:1:1:</a> CPY001 Missing copyright notice at top of file
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/settings.py#L26'>zproject/settings.py:26:1:</a> F401 [*] `.configured_settings.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/settings.py#L27'>zproject/settings.py:27:1:</a> F401 [*] `.computed_settings.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/test_settings.py#L1'>zproject/test_settings.py:1:1:</a> CPY001 Missing copyright notice at top of file
- <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/test_settings.py#L1'>zproject/test_settings.py:1:1:</a> CPY001 Missing copyright notice at top of file
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/test_settings.py#L21'>zproject/test_settings.py:21:1:</a> F401 [*] `.settings.*` imported but unused
+ <a href='https://github.com/zulip/zulip/blob/bbfcb2dcb3bbc05ee04707f892520be1b05c7451/zproject/test_settings.py#L22'>zproject/test_settings.py:22:1:</a> F401 [*] `.test_extra_settings.*` imported but unused
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 21 | 21 | 0 | 0 | 0 |
| CPY001 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2023-12-07 02:57_

> It would definitely get the case you posted right, yes. Given our semantic model (where we basically assume all branches are traversed), I think it's more likely to have false negatives than false positives.

By right you mean it leaves the import because it can't be proven to be unused? I read through the code and my understanding is that it would remove the import .Would you mind adding it as a test case (and other test cases where it isn't safe to remove the imports)

---

_Comment by @MichaReiser on 2023-12-07 02:59_

Hmm, the boke warning migth be incorrect


https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/__init__.py#L66

Init files in general seem hard

---

_Comment by @charliermarsh on 2023-12-07 03:10_

It would _not_ remove the import. Given:

```python
from "a" import b, c

print(b)

from foo import *

print(a) # we can resolve `a` but we don't know if `foo` rebound `a` 
```

When we get to `print(a)`, we would be unable to resolve the symbol to anything defined in the file, and so we would assume that `a` comes from `from foo import *`, and _not_ remove it.

---

_Comment by @charliermarsh on 2023-12-07 03:11_

Maybe you were thinking of something like this?

```python
from bar import x, y
from foo import *

print(x, y)
```

Where `foo` contains a definition of `x`? In this case, we would incorrectly remove `from foo import *`, which matches Flake8. I still think it's fine though should likely be an unsafe fix.


---

_Comment by @MichaReiser on 2023-12-07 03:13_

> Maybe you were thinking of something like this?
> 
> ```python
> from bar import x, y
> from foo import *
> 
> print(x, y)
> ```
> 
> Where `foo` contains a definition of `x`? In this case, we would incorrectly remove `from foo import *`, which matches Flake8. I still think it's fine though should likely be an unsafe fix.

Lol yes, that's what I meant. I had to rewrite the import statements a couple of time until I got the python syntax right. I must then have switched the names.

I would prefer opting on the safe side and not remove these imports. 



---

_Comment by @charliermarsh on 2023-12-07 03:46_

Let's pull in some more opinions, I could honestly go either way. \cc @zanieb @konstin 

---

_Comment by @konstin on 2023-12-08 12:49_

I don't think we can mark them as unused when they might be used to override previous imports, and think we shouldn't because when i use star imports i use them as reexports (mainly in `__init__.py[i]`). Looking at the ecosystem checks all of the cases already have an `noqa: F403` annotation, being effectively reexports we wouldn't want to additionally lint with F401 too.

---

_Comment by @charliermarsh on 2023-12-08 14:38_

For what it's worth, this PR omits wildcard imports in `init.py` files. But I'm not convinced this is a good change and there are clear concerns.

---

_Closed by @charliermarsh on 2023-12-08 14:38_

---

_Branch deleted on 2023-12-08 14:38_

---
