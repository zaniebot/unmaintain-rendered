```yaml
number: 15244
title: Unable to ignore UP006, UP035 after upgrade to 0.8.0 or later (python3.10)
type: issue
state: closed
author: jhovell
labels:
  - question
assignees: []
created_at: 2025-01-03T21:27:56Z
updated_at: 2025-01-05T08:15:19Z
url: https://github.com/astral-sh/ruff/issues/15244
synced_at: 2026-01-12T15:54:54Z
```

# Unable to ignore UP006, UP035 after upgrade to 0.8.0 or later (python3.10)

---

_@jhovell_

Bug not present in 0.7.4

After upgrading to 0.8.0-0.8.5 my project has a large number of `UP` rule violations, but the issue is there seems to be no way to suppress these violations.

```
poetry run ruff check . --statistics
1046	UP006 	[*] non-pep585-annotation
 385	UP035 	[ ] deprecated-import
[*] fixable with `ruff check --fix`
```

Adding

```
[tool.ruff.lint]
...
ignore = [
    "UP006", # non-pep585-annotation
    "UP035", # deprecated import
]
```
... has no effect and also interestingly `"UP"` or any variant is not present in my `select = [ .. ]` section, and error even persists if i remove my `select` block entirely (presumably default rules) or explicitly `select = [ ] `

I have experimented with different `target-version` settings and also tried various settings for 
```
[tool.ruff.lint.pyupgrade]
keep-runtime-typing = true or false
```

```
[tool.ruff] # any of the following

target-version = "py38"
target-version = "py39"
target-version = "py310"
```

I have also tried with preview mode turned on and off, same result. 
 
```
poetry run python --version
Python 3.10.13
```

Is some other rule or setting triggering these rules? Is there some way to work around these violations temporarily so I can upgrade ruff? 


---

_Comment by @oprypin on 2025-01-03 23:54_

Does a file `ruff.toml` exist in your directory, or do you have global overrides?

---

_Comment by @MichaReiser on 2025-01-04 12:03_

Can you run ruff with `ruff check --show-settings` to see what rules are selecting. Or consider using `ruff check --verbose` to see which configuration files Ruff picks up

---

_Label `question` added by @MichaReiser on 2025-01-04 12:03_

---

_Comment by @jhovell on 2025-01-04 19:41_

> Does a file `ruff.toml` exist in your directory, or do you have global overrides?

It's a poetry project and everything is configured through pyproject.toml. There's no ruff.toml. Not sure where else I'd look at for global overrides? 

Relevant section for pyproject.toml
```

[tool.ruff]
line-length = 120
target-version = "py310"

[tool.ruff.lint]
ignore = [
    "UP006",
    "UP035",
]

[tool.ruff.lint.pyupgrade]
keep-runtime-typing = true
```

---

_Comment by @MichaReiser on 2025-01-04 19:45_

> Can you run ruff with `ruff check --show-settings` to see what rules are selecting. Or consider using `ruff check --verbose` to see which configuration files Ruff picks up

Can you try the commands that I listed above?

---

_Comment by @jhovell on 2025-01-04 19:51_

@MichaReiser ah, thanks! that uncovered the issue. Per your suggestion, I ran `ruff check --verbose` and saw that there were two other pyproject.toml files in sub projects I did not know about and did not know they were being processed. not sure if this is recommended or not. 

Feel free to close this issue. Really appreciate it!

---

_Closed by @MichaReiser on 2025-01-05 08:15_

---
