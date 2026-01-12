```yaml
number: 7469
title: "Consider presence on the `PATH` an opt-in to Python pre-releases"
type: issue
state: closed
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2024-09-17T15:57:07Z
updated_at: 2024-09-18T15:19:11Z
url: https://github.com/astral-sh/uv/issues/7469
synced_at: 2026-01-12T15:59:14Z
```

# Consider presence on the `PATH` an opt-in to Python pre-releases

---

_@zanieb_

From @henryiii in https://github.com/astral-sh/uv/issues/7300#issuecomment-2356049477

> Yes, the issue wasn't VIRTUAL_ENV, but rather that we were making the venv assuming it would use the Python on the top of the PATH. cibuildwheel 2.21.1 fixes this.
> 
> It's really weird, though, that it no longer takes the Python at the top of the PATH when `--system` is used. If someone goes to the trouble to put Python in the PATH, intentionally skipping over it is really surprising; and if you type `python`, you don't get the one you just installed to. For example:
>
>
> ```yaml
> - uses: actions/setup-python@v5
>   with:
>     python-version: ${{ matrix.python-version }}
>     allow-prereleases: true
> - uses: astral-sh/setup-uv@v3
> - run: uv pip install --system some_lib
> - run: python -c "import some_lib"
> ```
> 
> now fails only on 3.13 because uv installed to the 3.12 instead of the 3.13 that's been prepared by setup-python. The `python` above is 3.13, while uv is skipping it and installing into 3.12. (I've seen this failure somewhere but have forgotten which repos it affected).
> 
> I understand this makes sense with managed Python (where you have a bunch of Pythons and uv's just picking out the best one), but it is very surprising to have it do this to the Python on PATH.


We may want to consider allowing `--system` to opt-in here, but I'm a little hesitant to have different discovery behaviors.

cc @carljm / @AlexWaygood who suggested to me that we should not select Python pre-releases by default.
            

---

_Label `needs-decision` added by @zanieb on 2024-09-17 15:58_

---

_Comment by @henryiii on 2024-09-17 16:03_

I don't like handling pre-releases differently from full releases in general - if someone is helping test pre-releases, making it harder for them isn't very helpful. We want to encourage pre-release to be easy to test and use like regular releases (especially RC's). The only exception I know of is the original motivation: if you have installed several _managed_ Pythons, defaulting to the latest non-prerelease is a good default. But these aren't on the PATH. What about only having special handling for commands that pick up a managed release automatically, and letting PATH work normally? Is that possible?

```yaml
- uses: actions/setup-python@v5
  with:
    python-version: "3.13"
    allow-prereleases: true
- run: python --version  # 3.13
- run: uv venv # I would kind of expect Python 3.13 here
```
```yaml
- run: uv python install 3.11
- run: uv python install 3.12
- run: uv python install 3.13
- run: python --version # This is whatever's already present, or nothing
- run: uv venv  # I would not expect Python 3.13 here, either system or 3.12
```



---

_Comment by @zanieb on 2024-09-17 16:11_

It's definitely possible — we just need to align on the behavior.

I believe https://github.com/astral-sh/uv/pull/7470 would achieve the behavior here.

---

_Renamed from "Consider `--system` an opt-in to Python pre-releases" to "Consider `PATH` an opt-in to Python pre-releases" by @zanieb on 2024-09-17 16:13_

---

_Renamed from "Consider `PATH` an opt-in to Python pre-releases" to "Consider presence on the `PATH` an opt-in to Python pre-releases" by @zanieb on 2024-09-17 16:13_

---

_Closed by @zanieb on 2024-09-18 15:19_

---

_Closed by @zanieb on 2024-09-18 15:19_

---
