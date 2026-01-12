```yaml
number: 8503
title: "ARG: Add option to enforce granularity"
type: issue
state: open
author: ofek
labels:
  - suppression
assignees: []
created_at: 2023-11-06T04:07:53Z
updated_at: 2023-11-07T15:30:40Z
url: https://github.com/astral-sh/ruff/issues/8503
synced_at: 2026-01-12T15:54:48Z
```

# ARG: Add option to enforce granularity

---

_@ofek_

It would be nice if there was [an option](https://docs.astral.sh/ruff/settings/#flake8-unused-arguments) for the ARG rules that would force ignore comments to apply just to the particular unused arguments. Often I want to take note in the code that one or two are intentionally ignored but the others should definitely not be. When this option exists there should be another rule added that would allow you to actually ignore the entire function/method.

Example from https://github.com/pypa/hatch/pull/1031, an implementation of [PEP 517](https://peps.python.org/pep-0517/#build-wheel):

```
def build_wheel(
    wheel_directory: str,
    config_settings: dict[str, Any] | None = None,  # noqa: ARG001
    metadata_directory: str | None = None,  # noqa: ARG001
) -> str:
```

---

_Comment by @charliermarsh on 2023-11-07 04:46_

Can you expand on this a bit, or give an example of what you mean? (Do the ignore comments in your example above not work as expected?)

---

_Label `waiting-on-author` added by @charliermarsh on 2023-11-07 04:46_

---

_Comment by @ofek on 2023-11-07 05:03_

No I mean I want to enforce the example from above rather than allowing the comment to apply to the entire function

---

_Comment by @charliermarsh on 2023-11-07 05:08_

If I plug that into the [playground](https://play.ruff.rs/f7660caa-b0db-46ba-bba7-6b5bfc3572e3), I see that the second two arguments _are_ ignored, while the first is flagged with `ARG001`. I'm really sorry because I suspect I'm still just misunderstanding the ask!

---

_Comment by @ofek on 2023-11-07 13:46_

I want this to produce an error:

```
def build_wheel(wheel_directory, config_settings, metadata_directory) -> str:  # noqa: ARG001
```

whereas this is allowed:

```
def build_wheel(
    wheel_directory: str,
    config_settings,  # noqa: ARG001
    metadata_directory,  # noqa: ARG001
) -> str:
```

---

_Comment by @charliermarsh on 2023-11-07 15:30_

Ahh got it, thanks!

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-11-07 15:30_

---

_Label `noqa` added by @charliermarsh on 2023-11-07 15:30_

---
