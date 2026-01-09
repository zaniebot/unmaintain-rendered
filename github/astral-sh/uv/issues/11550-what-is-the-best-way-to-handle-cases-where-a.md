---
number: 11550
title: What is the best way to handle cases where a Python package internally assumes that pip is the installer?
type: issue
state: closed
author: devon-research
labels:
  - question
assignees: []
created_at: 2025-02-16T05:31:52Z
updated_at: 2025-03-10T00:25:33Z
url: https://github.com/astral-sh/uv/issues/11550
synced_at: 2026-01-07T13:12:18-06:00
---

# What is the best way to handle cases where a Python package internally assumes that pip is the installer?

---

_Issue opened by @devon-research on 2025-02-16 05:31_

### TL;DR Version

What is the best way to handle cases where a Python package internally assumes that pip is the installer (specifically for installing packages during testing)?

### Context

The relevant context is that the situation brought up in [this comment](https://github.com/astral-sh/uv/issues/1331#issuecomment-1947528004)[^1] arises in the [Inspect AI](https://github.com/UKGovernmentBEIS/inspect_ai) package [here](https://github.com/UKGovernmentBEIS/inspect_ai/blob/011d6da45094f1e420565408eee06d80f70507c4/tests/test_helpers/utils.py#L228)  and [here](https://github.com/UKGovernmentBEIS/inspect_ai/blob/011d6da45094f1e420565408eee06d80f70507c4/tests/conftest.py#L97). In the former case:

```python
def ensure_test_package_installed():
    try:
        import inspect_package  # type: ignore # noqa: F401
    except ImportError:
        subprocess.check_call(
            [sys.executable, "-m", "pip", "install", "--no-deps", "tests/test_package"]
        )
```

Here `inspect_package` is a test extension for `inspect_ai` that is just used for testing purposes, and the package authors decided to install it in the process of running the tests and then automatically uninstall it once `pytest` concludes.

When running `pytest` with uv, all the tests that rely on the installation of `inspect_package` fail because the above assumes you are using pip. More specifically, running this yields unexpected failing tests:
```bash
git clone https://github.com/UKGovernmentBEIS/inspect_ai.git
cd inspect_ai
uv sync --extra dev
uv run pytest
```
This is in contrast to the non-uv workflow which works fine:
```bash
git clone https://github.com/UKGovernmentBEIS/inspect_ai.git
cd inspect_ai
pip install -e ".[dev]"
pytest
```

### Solutions [?]

Possible solutions for dealing with this might include:

1. Adding `skip_if` decorators on relevant tests for when running under uv
2. Detect when the tests are being run under uv and use uv in the [install](https://github.com/UKGovernmentBEIS/inspect_ai/blob/011d6da45094f1e420565408eee06d80f70507c4/tests/test_helpers/utils.py#L233) and [uninstall](https://github.com/UKGovernmentBEIS/inspect_ai/blob/011d6da45094f1e420565408eee06d80f70507c4/tests/conftest.py#L101) steps instead
3. Add `inspect_package` / `tests/test_package` to the `dev` dependencies
4. Advise uv users to do `uv venv --seed` before running `uv sync --extra dev` and `uv run pytest` (as mentioned e.g. [here](https://github.com/astral-sh/uv/issues/7534#issuecomment-2360539972))
5. Add `pip` to the `dev` dependencies

Putting aside worries that installing and using pip in the uv venv will cause unexpected problems, approach 5 seemed reasonable to me. In practice, this might look like adding these two lines to the `pyproject.toml` file:
```toml
[dependency-groups]
dev = ["inspect_ai[dev]", "pip"]
```
This requires no uv-specific changes to the code and has the nice benefit of allowing uv users to just run `uv run pytest` with no additional setup steps (integrating with the existing `project.optional-dependencies` in a similar way as discussed [here](https://github.com/astral-sh/uv/issues/11093)).

However, I am not sure if this is a bad idea because I'm uncertain if using pip within the uv-managed venv as in the code snippet above (or just having pip installed more generally) might cause unexpected problems.

### Questions

Would approach 5 likely cause problems? What solution (possibly one not listed) seems ideal here?

[^1]: Specifically, the part "it's expected that you can always run an installer via `subprocess.run([sys.executable, "-m", "pip", ...])`"

---

_Label `question` added by @devon-research on 2025-02-16 05:31_

---

_Comment by @konstin on 2025-02-18 20:04_

Both pip and uv follow the same standards for installing and uninstalling packages. It's possible to use both together in the same venv just as you described.

Depending on how your tests are structured, another option may be requiring `tests/test_package` with the same development group that installs pytest.

---

_Closed by @devon-research on 2025-03-10 00:25_

---
