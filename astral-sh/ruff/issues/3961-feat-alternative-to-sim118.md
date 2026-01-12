```yaml
number: 3961
title: "feat: alternative to SIM118"
type: issue
state: closed
author: upstartjohnvandivier
labels: []
assignees: []
created_at: 2023-04-13T18:10:07Z
updated_at: 2023-10-19T21:28:33Z
url: https://github.com/astral-sh/ruff/issues/3961
synced_at: 2026-01-12T15:54:44Z
```

# feat: alternative to SIM118

---

_@upstartjohnvandivier_

as a developer, I would like an alternative to `SIM118` so that I can enforce `.keys()` is used consistently, instead of having to choose between enabling or disabling SIM118. When `SIM118` is disabled, developers can use `.keys()` or not use it as they please, leading to inconsistent code which is not ideal.

---

_Comment by @markis on 2023-04-13 19:39_

I believe the alternative, using the `.keys()` method, would come at a performance cost. My simple test had these results.
```python
DATA: dict[str, int | None] = {"value": 1, "value2": 1}


def default_method() -> None:
    for key in DATA:
        assert DATA[key] == 1


def keys_method() -> None:
    for key in DATA.keys():
        assert DATA[key] == 1


"""
Python 3.10 on an M1
(smaller is better)
default_method ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 49.0
keys_method    ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 61.0
"""

"""
Python 3.10 on an intel xeon based server
(smaller is better)
default_method ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 18.0
keys_method    ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 21.0
"""
```

---

_Comment by @charliermarsh on 2023-05-08 22:37_

I think it's gonna be hard for us to reasonably enforce the _opposite_ right now, since we don't have a good way of detecting that something is a `dict` and thus needs `.keys()`. So, I'm gonna err on the side of closing.

---

_Closed by @charliermarsh on 2023-05-08 22:37_

---

_Comment by @jpivarski on 2023-10-19 21:18_

Because you can't detect whether something is a dict, SIM118 has false positives. I just ran into one—automatically removing the `.keys()` caused a bug that took me a while to solve.

I think it's fine for SIM118 to raise an error, but not for it to auto-fix. When it's raised as an error, that gives me the chance to choose whether to remove `.keys()` or to add `# noqa: SIM118`, depending on the specific context that ruff doesn't know.

Should this be a new issue?

---

_Comment by @charliermarsh on 2023-10-19 21:20_

@jpivarski - In the most recent release ([v0.1.0](https://astral.sh/blog/ruff-v0.1.0)), we introduced the concept of fix safety, and the fix here is marked as "unsafe" -- so you now have to provide the `--unsafe-fixes` flag in order for it to be applied (though the CLI will tell you that it's available). I think that should roughly match what you're describing!

---

_Comment by @jpivarski on 2023-10-19 21:28_

Perfect, thanks!

Yes, my .pre-commit-config.yaml is at ruff 0.0.291, but it will auto-update soon. Looking forward to it!

---
