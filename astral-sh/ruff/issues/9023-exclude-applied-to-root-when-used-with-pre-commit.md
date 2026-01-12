```yaml
number: 9023
title: Exclude applied to root, when used with pre-commit
type: issue
state: open
author: 99njaggi
labels:
  - question
assignees: []
created_at: 2023-12-06T12:31:37Z
updated_at: 2023-12-08T08:52:18Z
url: https://github.com/astral-sh/ruff/issues/9023
synced_at: 2026-01-12T15:54:48Z
```

# Exclude applied to root, when used with pre-commit

---

_@99njaggi_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.
I'm not sure if this is an issue of Ruff or pre-commit.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Files are not checked with Ruff running in pre-commit, if the git repository is inside a build directory. 

### Steps to reproduce

The setup is very basic.
Create a directory `build`.
Initialize git inside this directory. 
Create a Python file, which is violating Ruff rules.
Install pre-commit hook (with the pre-commit Python package).
Run pre-commit on all files -> the Python file will not be checked.


### Project structure
```
build
    |- .git
    |- .pre-commit-config.yaml
    |- test.py
```

### Executed commands: 
``` shell
> mkdir build
> cd build
> git init
# Add .pre-commit-config.yaml and test.py to git
> python -m pre_commit install
> python -m pre_commit run --all-files --verbose
ruff.....................................................................Passed
- hook id: ruff
- duration: 0s

warning: No Python files found under the given path(s)
> python -m ruff check .
test.py:3:8: F401 [*] `ruff` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```
 
### Files
test.py
``` python
#!/usr/bin/env python3

import ruff

def main():
    pass

if __name__ == '__main__':
    main()
```

.pre-commit-config.yaml 
``` yaml
repos:
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.1.6
  hooks:
  - id: ruff
    types: [python]
```

### Versions
pre-commit==3.5.0
ruff==0.1.7



---

_Comment by @dhruvmanila on 2023-12-06 16:27_

Can you try it with the official pre-commit hook as mentioned in the [documentation](https://docs.astral.sh/ruff/integrations/#pre-commit)?

So, your config would be:
```yaml
repos:
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.1.6
  hooks:
  - id: ruff
    types: [python]
```

Also, can you provide the output for the command `python -m pre_commit run --all-files --verbose`?

---

_Label `question` added by @dhruvmanila on 2023-12-06 16:28_

---

_Comment by @99njaggi on 2023-12-07 07:52_

> Can you try it with the official pre-commit hook as mentioned in the [documentation](https://docs.astral.sh/ruff/integrations/#pre-commit)?

Same result with the official pre-commit hook. Files are not checked and the violation is not detected.

> Also, can you provide the output for the command python -m pre_commit run --all-files --verbose?

```
> python -m pre_commit run --all-files --verbose
ruff.....................................................................Passed
- hook id: ruff
- duration: 0.01s

warning: No Python files found under the given path(s)
```


---

_Comment by @zeshuaro on 2023-12-07 09:44_

I'm pretty sure this is the same problem that I brought up here https://github.com/astral-sh/ruff/discussions/6191

---

_Comment by @charliermarsh on 2023-12-07 14:23_

Are the files correctly checked if you add a `ruff.toml` to the root of the `build` directory?

---

_Comment by @99njaggi on 2023-12-07 15:44_

> Are the files correctly checked if you add a `ruff.toml` to the root of the `build` directory?

No. Same result if a `ruff.toml` exists in the root.

The files are checked if ruff is called directly:
```
> python -m ruff check .
test.py:3:8: F401 [*] `ruff` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

With the pre-commit hook, they are not checked:
```
> python -m pre_commit run --all-files --verbose
ruff.....................................................................Passed
- hook id: ruff
- duration: 0s

warning: No Python files found under the given path(s)
```

Directory structure:
```
build
    |- .git
    |- .pre-commit-config.yaml
    |- test.py
    |- ruff.toml
```

---

_Comment by @charliermarsh on 2023-12-07 17:02_

Oh sorry, I missed that this is specifically when using `--all-files` (at least, I assume?). That I can at least explain. Presumedly, pre-commit is passing the root directory to Ruff, like `ruff check .` (from the directory containing `build`). In that case, when we start to iterate over files in that root directory, we use Ruff's default settings, and see the `build` directory, which is excluded by default, and skip it. (If we pass paths to files within `build` directly, Ruff will correctly use the settings within `build`.)

This is confusing in this case, but changing the behavior would also be confusing. Imagine that instead of `build`, your directory was `.venv`. It would be strange if we linted files within `.venv` in such a case, right?


---

_Comment by @zeshuaro on 2023-12-08 06:17_

If pre-commit provides the files in relative paths, would the problem be resolved?

---

_Comment by @99njaggi on 2023-12-08 08:52_

> Oh sorry, I missed that this is specifically when using `--all-files` (at least, I assume?). That I can at least explain. Presumedly, pre-commit is passing the root directory to Ruff, like `ruff check .` (from the directory containing `build`). In that case, when we start to iterate over files in that root directory, we use Ruff's default settings, and see the `build` directory, which is excluded by default, and skip it. (If we pass paths to files within `build` directly, Ruff will correctly use the settings within `build`.)

I think there is a misunderstanding between us. 
There is no directory named `build` in the project. The root directory is named `build`.
If we are using strictly relative paths, no path will contain `build`.

The current behavior is not consistent between pre-commit and directly using Ruff. 
Ruff is working as expected when called directly:
```
> cd /home/user/build
> ruff .
test.py:3:8: F401 [*] `ruff` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
> ruff /home/user/build/test.py
test.py:3:8: F401 [*] `ruff` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Running it with pre-commit has a different behavior:
``` 
> python -m pre_commit run --all-files --verbose
ruff.....................................................................Passed
- hook id: ruff
- duration: 0.05s

warning: No Python files found under the given path(s)
```

----

If we check the documentation here. https://docs.astral.sh/ruff/settings/#exclude

> Single-path patterns, like .mypy_cache (to exclude any directory named .mypy_cache in the tree), foo.py (to exclude any file named foo.py), or foo_*.py (to exclude any file matching foo_*.py ).

Ruff will exclude any directory named `build` in the tree. 
I expect it to check only the tree starting from root (the `build` directory in our case).
Therefore the tree would look like this: 
```
./
├─ .git/
├─ .pre-commit-config.yaml
├─ test.py
├─ ruff.toml
```





---
