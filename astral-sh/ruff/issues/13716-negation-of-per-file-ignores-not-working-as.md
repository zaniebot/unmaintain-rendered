```yaml
number: 13716
title: Negation of per-file-ignores not working as expected
type: issue
state: closed
author: jmspereira
labels:
  - question
  - needs-info
assignees: []
created_at: 2024-10-11T09:44:33Z
updated_at: 2024-11-20T12:02:38Z
url: https://github.com/astral-sh/ruff/issues/13716
synced_at: 2026-01-10T11:09:55Z
```

# Negation of per-file-ignores not working as expected

---

_Issue opened by @jmspereira on 2024-10-11 09:44_

Hey everyone, 

I have a monorepo where I am using the banned-api from flake8-tidy-imports to block the usage of an import of a certain package. However, I only want this to work inside the projects that are inside a folder called abc. I am trying to use the per-file-ignores
configuration, in a config file that I use globally across the repo, with the following:

```toml
[lint.per-file-ignores]
"!abc/**.py" = ["TID251"] 
```

However, this appears not to be working. Am I doing something wrong?



---

_Comment by @MichaReiser on 2024-10-11 14:32_

Can you tell me a bit more about what exactly isn't working? For which files is `TID0251` not enabled or disabled where you would expect it to be enabled/disabled. 

You might want to consider using `!abc/**/*.py` to match files at an arbitrary deep nesting level (e.g. even if they're in sub directories of `abc`.

---

_Comment by @jmspereira on 2024-10-15 16:28_

Hey @MichaReiser,

Sorry for the delay. I want TID0251 to be enabled in every project inside abc folder and disable otherwise. I'm not sure what ruff does about the path that is used for those exclusions when a config file is passed, but it appears to be using the relative path instead of the absolute path.

For instance, I have a project inside abc, let's call it xpto, that has a src folder inside. 
If I pass as parameter src/**.py the per-file-ingores configuration seems to be doing what I want.
   

---

_Comment by @MichaReiser on 2024-10-16 06:48_

That makes sense. Did you try using `!abc/**/*.py`? Also see https://www.atlassian.com/git/tutorials/saving-changes/gitignore as a reference


---

_Comment by @jmspereira on 2024-10-16 07:37_

Yes, I tried `!abc/**/*.py`, and still does not work. 

---

_Label `question` added by @MichaReiser on 2024-10-16 07:39_

---

_Comment by @MichaReiser on 2024-10-16 07:44_

Can you share an example project? 

I created a project with:

```
- abc
  - test.py
  - subfolder/test2.py
- src
	- test.py
root.py
pyproject.toml
```

```toml
[tool.ruff.lint.per-file-ignores]
"!abc/**/*.py" = ["TID251"]


[tool.ruff.lint.flake8-tidy-imports.banned-api]
"os".msg = "We don't want to rely on any platform specific code"
```

And ruff correctly flags `import os` in `abc/test.py` and `abc/subfolder/test2.py` but it doesn't flag the import in `src/test` or `root.py`

---

_Label `needs-info` added by @MichaReiser on 2024-10-16 07:44_

---

_Comment by @jmspereira on 2024-10-16 10:13_

Let's assume that the project is somewhat like this:

```
- abc
  - xpto1 
       - src 
           - test.py
           - subfolder/test2.py
           - ...
       - pyproject.toml
  - xpto2
       - src 
           - test.py
           - subfolder/test2.py
           - ...
       - pyproject.toml
    ....
- def
    - xpto3
       - src 
           - test.py
           - subfolder/test2.py
           - ...
       - pyproject.toml
  - xpto4
       - src 
           - test.py
           - subfolder/test2.py
           - ...
       - pyproject.toml
    .... 
- <tool-that-wraps-ruff>
     - resources/
         - config.toml
     - src
         - ... 
```

And what is specified in the config.toml is (I do not have the ruff configurations on a pyproject.toml by project):
```
[tool.ruff.lint.per-file-ignores]
"!abc/**/*.py" = ["TID251"]

[tool.ruff.lint.flake8-tidy-imports.banned-api]
"os".msg = "We don't want to rely on any platform-specific code"
```

And the tool that calls ruff is calling it:

```
ruff check <absolute_path_to_xpto1_src> --config <absolute_path_to_config.toml> --fix 
ruff check <absolute_path_to_xpto2_src> --config <absolute_path_to_config.toml> --fix 
ruff check <absolute_path_to_xpto3_src> --config <absolute_path_to_config.toml> --fix 
ruff check <absolute_path_to_xpto4_src> --config <absolute_path_to_config.toml> --fix 
```

---

_Comment by @MichaReiser on 2024-10-16 10:40_

I'm not sure if `--config` works with ignore. I would have to look into this in more detail. I'm also not sure if the config passed to `--config` MUST use `lint.per-file-ignores` without the `tool.ruff` prefix because it isn't a pyproject.toml.

The easiest here might actually be to make use of ruff's configuration inheritance feature where:

* You have a `config.toml` at the root
* You add a `ruff.toml` inside of `abc` and it sets `lint.extend-select = ["TID251"]` and `extend = "../config.toml"`
* You set `extend="../config"` in every `pyproject.toml` **unless** they contain no ruff configuration. If so, you're good to go.

That should also remove the need to call ruff with an explicit config parameter because it will automatically pick up the closest `ruff.toml`

---

_Closed by @MichaReiser on 2024-11-20 12:02_

---
