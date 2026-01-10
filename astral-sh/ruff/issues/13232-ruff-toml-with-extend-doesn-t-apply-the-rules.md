---
number: 13232
title: "Ruff toml with `extend` doesn't apply the rules"
type: issue
state: closed
author: rogier-stegeman
labels:
  - question
  - configuration
assignees: []
created_at: 2024-09-03T15:23:11Z
updated_at: 2024-09-05T09:31:23Z
url: https://github.com/astral-sh/ruff/issues/13232
synced_at: 2026-01-10T01:22:53Z
---

# Ruff toml with `extend` doesn't apply the rules

---

_Issue opened by @rogier-stegeman on 2024-09-03 15:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I looked for: "extend", "extend configuration".

Version: 0.6.3

**Situation**
```py
#test.py
def f(a):
    return a
```

And 
```toml
#ruff.toml
extend  = "./ruff.2.toml"
[lint]
select = []
ignore = []
```

And finally
```toml
#ruff.2.toml
[lint]
select = []
ignore = []
```

And command: `ruff check test.py`.

**Result**

No warnings, nothing. As expected.

**Change situation**

Add a random line like `foo=[]` to `ruff.2.toml`. Or moving the `select` outside of the `[lint]` block.

**Result**

Errors on running the command, like unknown rules. This is great, so it can find the file listed in `extend` and evaluates it properly.

**Change situation**

```toml
#ruff.toml
extend  = "./ruff.2.toml"
[lint]
select = ["ANN"]
ignore = []
```

```toml
#ruff.2.toml
[lint]
select = ["D"]
ignore = ["ANN001"]
```

**Result**

Expected: ANN001 to be supressed, ANN201 to be raised, D103 to be raised.

Actual: ANN001 is raised, ANN 201 is raised.

This is equivalent to only `ruff.toml` being used, in other words, `ruff.2.toml` is completely ignored, even though it is evaluated for correct syntax.


---

_Comment by @charliermarsh on 2024-09-03 16:03_

If the `pyproject.toml` that contains an `extend` defines its own `select`, it effectively resets the selection chain. In your situation, you want `ruff.toml` to use `extend-select = ["ANN"]` instead of `select = ["ANN"]`.

---

_Label `question` added by @charliermarsh on 2024-09-03 16:03_

---

_Label `configuration` added by @AlexWaygood on 2024-09-03 16:15_

---

_Comment by @rogier-stegeman on 2024-09-04 14:47_

My `pyproject.toml` doesn't specify any ruff configuration, I assume you meant `ruff.toml` in my scenario, and to put the `extend-select` in my `ruff.2.toml`, the file which is extending.

How would this work with `ignore`? The docs tell me `extend-ignore` is deprecated. A normal `ignore` is itself, ignored.

<strike>
Second, my issue is not specifically with the `select` options not merging, the entire file found and validated by `extend`, is not actually used. Rules like `line-length` and ~~

```toml
[lint.pyupgrade]
keep-runtime-typing = true
```
Are also ignored in my extended file (`ruff.2.toml`). This while the file is correctly found by the `extend` rule because any syntax errors in the `ruff.2.toml` cause an error when running any ruff command.
</strike>

---

_Comment by @MichaReiser on 2024-09-04 14:55_

How did you determine that the settings were ignored? 

Can you try running `ruff check --show-settings`?

---

_Comment by @rogier-stegeman on 2024-09-04 15:20_

Mhm my VSCode seems to have been acting up yesterday, the main way I tested it. Striked from previous comment. It works fine now for most configuration.

Still my main issue, and why I created this post, is that I can't disable specific rules. My use case is to have a generic ruff.toml used in all projects and one for the specific project.

---

_Comment by @MichaReiser on 2024-09-05 07:22_

The problem you're facing here is that Ruff resolves the selectors of the base configuration first, before merging them with the sub-configuration. 

What happens when resolving the rules of `ruff.toml` is:

* Select all `D` rules
* Ignore `ANN001` but the rule is already disabled. So this is a no-op
* Select `Ann`, this does include `ANN001`. 

That's why you end up with `ANN001` selected. 

You would have to move the `select=["ANN"]` to `ruff.2.toml` if you want that the `ignore = ["ANN001"]` to have any effect or move the ignore to `ruff.toml`.

---

_Comment by @rogier-stegeman on 2024-09-05 09:31_

Aah I think I'm doing this the wrong way around. `ruff.toml` should be the specific one. Ignores here work even if they don't select anything. If I want anything extra I use `extend-select`, but I don't need to just to ignore more settings. Then Instead of a `ruff.python38.toml` I would create a `ruff.base.toml` and share that one with all projects. If I still want drop-in support for specific configuration files like a ruff file for a specific python version, I'll either have to do `pyproject` extend `ruff specific` extend `ruff base`. Or `pyroject` extend `ruff.toml` extend `ruff specific` extend `ruff base`. Sounds extremely overkill but would allow for very standardised files we can keep synced through dozens of projects.
![image](https://github.com/user-attachments/assets/6aa196cb-1eb2-467a-99b4-b0fef9f0a1c5)


---

_Closed by @rogier-stegeman on 2024-09-05 09:31_

---
