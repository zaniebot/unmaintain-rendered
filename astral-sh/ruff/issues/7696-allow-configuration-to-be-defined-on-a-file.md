```yaml
number: 7696
title: Allow configuration to be defined on a file-scoped basis
type: issue
state: open
author: Czaki
labels:
  - configuration
assignees: []
created_at: 2023-09-28T11:45:56Z
updated_at: 2025-02-27T17:07:29Z
url: https://github.com/astral-sh/ruff/issues/7696
synced_at: 2026-01-10T11:09:50Z
```

# Allow configuration to be defined on a file-scoped basis

---

_Issue opened by @Czaki on 2023-09-28 11:45_

It will be nice to allow define `banned-api` rules per file pattern. 

In general, there are API that may be useful in tests but should not be used in the library itself:
For example, `warnings.catch_warnings` because of https://github.com/python/cpython/issues/73858.

On the other hand, I would like to block using `numpy.random` in tests but allow in the library and/or examples (as it leads to random test failure).

If you think it is useful, I could try to contribute (learning rust).

---

_Comment by @MichaReiser on 2023-09-28 12:23_

That makes sense. Have you tried using a [hierarchical configuration](https://docs.astral.sh/ruff/configuration/#pyprojecttoml-discovery) to achieve your goal? The way this works is that you create a new `ruff.toml` in the test directory that `extend`s your default configuration. The test-specific configuration can override individual settings.

---

_Comment by @Czaki on 2023-09-28 12:51_

Unfortuantelly in this project (napari), test are not stored in a separate directory but in `_test` modules. And I'm only one of the maintainers. 
(and this simplifies searching for a test of a given function). 

---

_Comment by @charliermarsh on 2023-09-29 01:29_

Really wishing we had ESLint-style file overrides, as in:

```json
{
  "rules": {
    "quotes": ["error", "double"]
  },

  "overrides": [
    {
      "files": ["bin/*.js", "lib/*.js"],
      "excludedFiles": "*.test.js",
      "rules": {
        "quotes": ["error", "single"]
      }
    }
  ]
}
```


---

_Label `configuration` added by @charliermarsh on 2023-09-29 01:29_

---

_Comment by @Czaki on 2023-09-29 07:34_

cibuildwheel project use toml tables to implement overwrite rules inside pyproject.toml https://github.com/pypa/cibuildwheel/
https://cibuildwheel.readthedocs.io/en/stable/options/#overrides

Maybe it could be an inspiration? 

---

_Comment by @MichaReiser on 2023-09-29 09:01_

> Really wishing we had ESLint-style file overrides, as in:
> 
> ```json
> {
>   "rules": {
>     "quotes": ["error", "double"]
>   },
> 
>   "overrides": [
>     {
>       "files": ["bin/*.js", "lib/*.js"],
>       "excludedFiles": "*.test.js",
>       "rules": {
>         "quotes": ["error", "single"]
>       }
>     }
>   ]
> }
> ```

I've mixed feelings about `overrides`. It is powerful but can become confusing if you mix it with extend. E.g. what happens if you have:


```json
{
   "rules": {
     "quotes": ["error", "double"]
   },
 
   "overrides": [
     {
       "files": ["bin/*.js", "lib/*.js"],
       "excludedFiles": "*.test.js",
       "rules": {
         "quotes": ["error", "single"]
       }
     }
   ]
}
```

and

```json
{
	"extend": "config-above.json",
   "rules": {
     "quotes": ["error", "double"]
   }
}
```

What does now bind stronger, the parent configuration because it's in the `overrides` section or the `extend`? I think we would need to change our configuration more drastically to make "overrides" feel intuitive, similar to what [eslint does with the new flat configuration](https://eslint.org/blog/2022/08/new-config-system-part-2/#glob-based-configs-everywhere)

---

_Renamed from "Allow define banned-api per file" to "Allow configuration to be defined on a file-scoped basis" by @charliermarsh on 2023-10-11 13:40_

---

_Comment by @charliermarsh on 2023-10-11 13:40_

(Making this issue more general -- it would be nice to support something like ESLint's flat configuration, in which you can define configuration that only applies to a subset of files.)

---

_Comment by @DetachHead on 2023-10-26 06:17_

i'm fine with hierarchical configuration, but the only problem is that i wish it would inherit the `flake8-tidy-imports.banned-api` options from parent directories:
```toml
# ./pyproject.toml
[tool.ruff.flake8-tidy-imports.banned-api]
"foo".msg = "banned project-wide"
```
```toml
# ./tests/ruff.toml
extend = "../pyproject.toml"

[flake8-tidy-imports.banned-api]
"bar".msg = "only banned in tests"
```

```py
# tests/test_foo.py
import foo # no error, because the `banned-api` the `tests` directory doesn't inherit from the global one
```

---

_Comment by @BT-rmartin on 2024-01-11 17:05_

In my case #7890 we want to configure isort on file-scope basis, and in our particular need the hierarchical solution is not feasible, as we want it for every __init__.py file. Is there any progress on this topic @charliermarsh ?

---

_Comment by @BT-rmartin on 2024-10-23 20:16_

@charliermarsh Is there any progress on configure isort on file-scope basis?

---

_Comment by @jorenham on 2025-02-10 14:14_

see https://github.com/astral-sh/ruff/issues/16057 for another unique use-case of this

---

_Comment by @irdkwmnsb on 2025-02-11 18:19_

I'm not sure if that's the feature I want, but my usecase is as follows:  
Coming from a big legacy project I have multiple files with long variable names where it's ok to have wider lines.  
And the formatter wants to split those lines into multiple lines, which ends up being not so pretty.

One specific example of this is such code:
```python3
if (
    (something1) is not None
    and (something2) is not None
    and (something3) is not None
):
```
With the default value of 88 the formatter will split up those lines to have `and` and `is not None` to be sometimes on one line, and sometimes on multiple, since the length of `something1` expression can be different.
If I set the `line-length` parameter to some large number the code looks neat and every `is not None` statement is placed on a separate line. But I don't want such large `line-length` on the whole project, so having a feature to be able to change settings on a per-file basis would be nice.

---

_Comment by @MichaReiser on 2025-02-11 18:23_

@irdkwmnsb you can already achieve that *if* all legacy files have a common ancestor directory by adding a `ruff.toml` in that ancestor directory.

You need this feature if the files are side by side to non-legacy files.

---

_Comment by @arogozhnikov on 2025-02-27 17:07_

I have two use-cases for this feature, line-length in notebooks vs plain python files (they are side by side), and an additional 'type' of python file, which also should have a different line-length (also side by side with rest of codebase)

---
