```yaml
number: 21844
title: Print Python version and Python platform in the fuzzer output when fuzzing fails
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/fuzzer-output
created_at: 2025-12-08T13:45:37Z
updated_at: 2025-12-08T14:42:31Z
url: https://github.com/astral-sh/ruff/pull/21844
synced_at: 2026-01-10T16:42:11Z
```

# Print Python version and Python platform in the fuzzer output when fuzzing fails

---

_Pull request opened by @AlexWaygood on 2025-12-08 13:45_

## Summary

There was some confusion in https://github.com/astral-sh/ruff/pull/21839, because ty as executed by the fuzzer was picking up the `requires-python = ">=3.7"` setting in Ruff's pyproject.toml file. But that's different from ty's default behaviour (if executed in a directory that has no pyproject.toml file, and no pyproject.toml file in any of its ancestor directories), which is to assume Python 3.14.

I think running the fuzzer with `--python-version={OLDEST_SUPPORTED_PYTHON}` is actually a pretty good idea -- it caught a bug in #21839! But we should do that explicitly rather than having ty implicitly pick up the Python version from Ruff's pyproject.toml file. And we should print out the Python version and Python platform to the terminal when the fuzzer finds a bug.

<details>
<summary>Screenshot showing the new output when a bug is found</summary>

<img width="2642" height="1692" alt="image" src="https://github.com/user-attachments/assets/c748c7df-a7cc-49df-abd6-0a9ec939e626" />

</details> 

## Test Plan

- See above screenshot
- CI on this PR


---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-12-08 13:45_

---

_Label `testing` added by @AlexWaygood on 2025-12-08 13:45_

---

_Label `ty` added by @AlexWaygood on 2025-12-08 13:45_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/pyproject.toml`:22 on 2025-12-08 13:46_

listing ty as an explicit dependency makes it easier to run `uv run ty check` locally

---

_Review comment by @AlexWaygood on `python/py-fuzzer/pyproject.toml`:37 on 2025-12-08 13:48_

I couldn't get the ty server to select the correct virtual environment without this in VSCode, even if I opened the `./python/py-fuzzer` folder as its own workspace in VSCode. Not sure why.

---

_@AlexWaygood reviewed on 2025-12-08 13:48_

---

_Review comment by @MichaReiser on `python/py-fuzzer/pyproject.toml`:37 on 2025-12-08 13:55_

This should not be necessary, given that the ruff `pyproject.toml` doesn't contain a `ty` section. 

What's more likely the case is that you selected (or VS Code did it for you) a Python interpreter in VS Code, which we use as preferred fallback. You can select a Python Interpreter using `Cmd+P, Select Interpreter`. Make sure it points to your venv.

---

_@MichaReiser reviewed on 2025-12-08 13:55_

---

_Review comment by @MichaReiser on `python/py-fuzzer/pyproject.toml`:37 on 2025-12-08 13:55_

Related: https://github.com/astral-sh/ty/issues/1185

---

_@MichaReiser reviewed on 2025-12-08 13:55_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 13:56_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@dhruvmanila reviewed on 2025-12-08 13:57_

---

_Review comment by @dhruvmanila on `python/py-fuzzer/fuzz.py`:70 on 2025-12-08 13:57_

Does this mean that the fuzzer will now always run the code assuming the oldest Python version and `linux`?

When I was debugging 21839, I ran the fuzzer with some print statements to get the panic message and it printed the message multiple times. This suggests that it must be running the same code multiple times (?) but I also noticed that it didn't print the message for every run which suggests that the Python version picked up could've been different.

Let me know if my assumptions are incorrect because I don't know how py-fuzzer works, you might know better! What I'm trying to ask is that would this make us loose coverage on testing the same seed with different Python version?

---

_Review comment by @MichaReiser on `python/py-fuzzer/fuzz.py`:51 on 2025-12-08 13:57_

The `parent.parent.parent` is rather funny here. I would probably write this as `Path(__file__) / "../../../pyproject.toml"`

Is this loading Ruff's `pyproject.toml`? Do we want that, given that it's a very old Python version? Should we instead fuzz for multiple Python versions (using sharding) or hard-code a reasonable and more recent Python version?

---

_@AlexWaygood reviewed on 2025-12-08 14:00_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/pyproject.toml`:37 on 2025-12-08 14:00_

I think I found a problematic setting in my VSCode User settings that might be the cause...

---

_@MichaReiser reviewed on 2025-12-08 14:00_

---

_@AlexWaygood reviewed on 2025-12-08 14:02_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/fuzz.py`:51 on 2025-12-08 14:02_

> Is this loading Ruff's `pyproject.toml`? Do we want that, given that it's a very old Python version?

I explained why I think that it's useful to test with an old version in my PR description. It's better at catching bugs that way! It's more likely to throw up situations that we didn't think about when writing PRs, such as: "What happens if we want to infer a type as a `ParamSpec` instance but the `ParamSpec` class doesn't exist yet in the `typing` module?"

> Should we instead fuzz for multiple Python versions (using sharding) or hard-code a reasonable and more recent Python version?

we can consider making those changes, for sure. For now, this PR just makes explicit what the PR was always implicitly doing before. I'd prefer to consider those changes separately.

---

_@AlexWaygood reviewed on 2025-12-08 14:04_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/fuzz.py`:70 on 2025-12-08 14:04_

This makes explicit what it was always doing: it was always using the Python version that it picked up from Ruff's pyproject.toml file before.

The reason why you saw multiple messages from your `print()` statements isn't because it was running ty with a matrix of Python versions or Python platforms. Instead, it's that the fuzzer found a bug on a huge generated Python file and then systematically tried to minimize the AST in that file to produce the smallest possible snippet on which the bug would reproduce. That involves executing ty repeatedly.

---

_Review comment by @MichaReiser on `python/py-fuzzer/fuzz.py`:51 on 2025-12-08 14:05_

Fair enough, although it is somewhat confusing because ty doesn't officially support Python 3.7. I think we only support 3.8 or newer

---

_@MichaReiser reviewed on 2025-12-08 14:05_

---

_@MichaReiser approved on 2025-12-08 14:06_

Let's revert the `[tool.ty.environment]` changes. I'd be inclined to hard code the Python version to 3.8 or shard the benchmark to check multiple versions (both seem equally likely to catch bugs IMO because we default to an old Python version in mdtests)

---

_@AlexWaygood reviewed on 2025-12-08 14:09_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/fuzz.py`:51 on 2025-12-08 14:09_

> Fair enough, although it is somewhat confusing because ty doesn't officially support Python 3.7. I think we only support 3.8 or newer

Hrm, it's listed as a supported version in our `--help` message?

<img width="3802" height="448" alt="image" src="https://github.com/user-attachments/assets/6bb3cf30-e38b-4c94-8eb8-2825a855f20f" />


---

_@MichaReiser reviewed on 2025-12-08 14:12_

---

_Review comment by @MichaReiser on `python/py-fuzzer/fuzz.py`:51 on 2025-12-08 14:12_

https://github.com/astral-sh/ty/issues/878

ty's `pyproject.toml` specifies `requires-python = ">=3.8"`


---

_@AlexWaygood reviewed on 2025-12-08 14:29_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/pyproject.toml`:37 on 2025-12-08 14:29_

Okay, I managed to get VSCode to select the correct Python interpreter.

When I use Pylance, a popup appears when I open a project for the first time, that invites me to select the Python interpreter from a drop-down. That never happened when opening the project with Pylance disabled and the ty-vscode extension running. Is that expected?

---

_Merged by @AlexWaygood on 2025-12-08 14:35_

---

_Closed by @AlexWaygood on 2025-12-08 14:35_

---

_Branch deleted on 2025-12-08 14:35_

---

_@MichaReiser reviewed on 2025-12-08 14:42_

---

_Review comment by @MichaReiser on `python/py-fuzzer/pyproject.toml`:37 on 2025-12-08 14:42_

My experience has been that VS Code sometimes automatically selects an interpreter the first time I open a project and then sticks to it. I'm not sure why or when it does that. I've found this confusing in the past, and this is why I considered changing how we should treat the Python environment from VS Code (but that also sort of feels bad)

---
