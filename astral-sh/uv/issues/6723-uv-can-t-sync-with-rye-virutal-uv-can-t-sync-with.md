```yaml
number: 6723
title: "uv can't sync with rye --virutal / uv can't sync with data-style layouts"
type: issue
state: closed
author: dream-dasher
labels: []
assignees: []
created_at: 2024-08-27T21:28:38Z
updated_at: 2024-08-28T01:39:27Z
url: https://github.com/astral-sh/uv/issues/6723
synced_at: 2026-01-12T15:59:06Z
```

# uv can't sync with rye --virutal / uv can't sync with data-style layouts

---

_@dream-dasher_

`uv sync` does not seem to support a layout like the following:
- root
    - data
        - input
        - intermediate
        - output
     - notebooks
         - nb_1.py
         - nb_2.py
         - nb_3.py
    - pyproject.toml

e.g., image:
![uv-nosync-layout](https://github.com/user-attachments/assets/3a7f3184-eb60-4522-b948-0b85d100bd7c)


I made the above repo with `rye init --virtual`
However, uv has been unable to work with it.  
It seems to have decided that both **data** and **notebooks_python** are packages.  (Note: there are no python files in the data directory at any depth)

`❯ uv sync`
> Resolved 40 packages in 12ms
> error: Failed to prepare distributions
>   Caused by: Failed to fetch wheel: redacted @ file:///Users/redacted/redacted/redacted/redacted
>   Caused by: Build backend failed to determine extra requires with `build_editable()` with exit status: 1
> --- stdout:
> 
> --- stderr:
> error: Multiple top-level packages discovered in a flat-layout: ['data', 'notebooks_python'].
> 
> To avoid accidental inclusion of unwanted files or directories,
> setuptools will not proceed with this build.
> 
> If you are trying to create a single distribution with multiple packages
> on purpose, you should not rely on automatic discovery.
> Instead, consider the following options:
> 
> 1. set up custom discovery (`find` directive with `include` or `exclude`)
> 2. use a `src-layout`
> 3. explicitly set `py_modules` or `packages` with a list of names
> 
>
> To find more information, look for "package discovery" on setuptools docs.
> ---

I use src layouts for apps and scripts, but they add a considerable level of indirection and complexity in data oriented notebooks.  (Not least because files run with different relative paths to the data depending on if you run them as a notebook or a script.  And absolute paths obviously don't work across collaborators.)

Am I missing a solution?
If not, I'd appreciate support for this from uv.



---

_Comment by @charliermarsh on 2024-08-27 21:30_

This is solved on main by #6585 but not yet released.

---

_Comment by @charliermarsh on 2024-08-28 01:39_

Closed by https://github.com/astral-sh/uv/pull/6585 which will be released tomorrow morning :)

---

_Closed by @charliermarsh on 2024-08-28 01:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-28 01:39_

---
