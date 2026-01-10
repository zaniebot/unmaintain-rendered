---
number: 15403
title: Overwrite ignore values from config from cli command
type: issue
state: closed
author: benedikt-mue
labels:
  - question
assignees: []
created_at: 2025-01-10T15:12:24Z
updated_at: 2025-06-24T09:09:10Z
url: https://github.com/astral-sh/ruff/issues/15403
synced_at: 2026-01-10T01:22:56Z
---

# Overwrite ignore values from config from cli command

---

_Issue opened by @benedikt-mue on 2025-01-10 15:12_

Hope, I can address my question in here, if not feel free to delete.
__

I have the following setup where I disable the F401 (delete unused imports) rule for development so it is not picked up by the auto-formatting.
However, before committing the code, F401 should be re-introduced. I run ruff in my pre-commit config, but I cannot see that there is any way to overwrite an existing ignore value from the cli, am I correct?

```
# pyroject.toml

...
[tool.ruff.lint]
select = [ "E4", "E7", "E9", "F",]
ignore = ["F401"]
fixable = [ "ALL"]
unfixable = []
...

```

```
# pre-commit.yaml
repo:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.8.4
    hooks:
      # Run the linter.
      - id: ruff
        name: Ruff-Linter
        types_or: [ python, pyi, jupyter ]
        args: [ --fix, --fixable]
      # Run the formatter.
      - id: ruff-format
        name: Ruff-Formatter
        types_or: [ python, pyi, jupyter ]

```

Any workarounds on how to achieve the intended behavior (without the need of two separate files) are also welcomed!
Cheers in advance.

---

_Label `question` added by @MichaReiser on 2025-01-10 15:18_

---

_Comment by @MichaReiser on 2025-01-10 15:19_

You should be able to use `--extend-select=F401` to re-enable a rule that was ignored in the configuration.

---

_Comment by @benedikt-mue on 2025-01-10 15:26_

Damn, you right.. I literally tried every flag except this one for some unknown reason.. thanks for the quick reply!

```
      - id: ruff
        name: Ruff-Linter
        types_or: [ python, pyi, jupyter ]
        args: [ --fix, --extend-select=F401]
```

---

_Closed by @benedikt-mue on 2025-01-10 15:26_

---

_Comment by @MichaReiser on 2025-01-10 16:14_

No worries. I'm glad it works :) Have a great weekend

---

_Comment by @DanielYang59 on 2025-06-24 08:56_

Hi @MichaReiser thanks for your reply on this, I have a very similar question: it seems `--extend-select` cannot re-enable rules ignored by `extend-per-file-ignores`? Is this intended behaviour?

---

More details: I have the following in `pyproject.toml` to ignore I001 for `__init__.py`, but `ruff check __init__.py --extend-select=I001` doesn't seem to detect issues (when there is):

```toml
[tool.ruff.lint]
select = [
    "I001", 
]

[tool.ruff.lint.extend-per-file-ignores]
"__init__.py" = ["I001"]
```





---

_Comment by @MichaReiser on 2025-06-24 09:02_

@DanielYang59 

`extend-per-file-ignores` takes precedence over `--extend-select` because it's the more specific option (you disabled that rule for `__init__.py`). You could try using `--config 'lint.extend-per-file-ignores.'__init__.py' = []'` but I'm not sure if that's supported.

---

_Comment by @DanielYang59 on 2025-06-24 09:09_

Hi thanks for the quick reply and input again

> extend-per-file-ignores takes precedence over --extend-select because it's the more specific option (you disabled that rule for __init__.py).

Great to know, this explains.

> You could try using --config 'lint.extend-per-file-ignores.'__init__.py' = []' but I'm not sure if that's supported.

I tried it seems to ignore the file still, so I guess I would just comment out the `extend-per-file-ignores` rule for now ;)



---
