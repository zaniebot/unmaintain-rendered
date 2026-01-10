---
number: 12078
title: "Creating `Package Application` with different target subfolder"
type: issue
state: open
author: Anselmoo
labels:
  - question
assignees: []
created_at: 2025-03-09T20:33:04Z
updated_at: 2025-03-10T06:14:17Z
url: https://github.com/astral-sh/uv/issues/12078
synced_at: 2026-01-10T01:25:15Z
---

# Creating `Package Application` with different target subfolder

---

_Issue opened by @Anselmoo on 2025-03-09 20:33_

### Question

If I want to specify a different target subfolder instead of `example_pkg` for `example-pkg`, I'd like to use `ep`. 

```shell
example-pkg
├── .python-version
├── README.md
├── pyproject.toml
└── src
    └── ep
        └── __init__.py
```


The Doctest and CI runs work fine. However, when I try using `uv run` or run it via the terminal and VSCode (`Run Python File in Dedicated Terminal`), I encounter this error:

```shell
ModuleNotFoundError: No module named 'ep'
```

Does the `pyproject.toml` need to be configured after this? 
Thanks for the feedback.


### GitHub-Example

<details><summary>Details</summary>
<p>

Previously, I used `poetry` and could locally run some of the modules in the terminal, but that is not possible anymore with `uv`. I am a little bit lost.

Ref: [useful-math-functions](https://github.com/Anselmoo/useful-math-functions)

<img width="270" alt="Image" src="https://github.com/user-attachments/assets/b46d5ed6-12e4-47a7-addf-9e8c8609df99" />
</p>
</details> 



### Platform

macOS 15.3.1

### Version

uv 0.6.5 (Homebrew 2025-03-06)

---

_Label `question` added by @Anselmoo on 2025-03-09 20:33_

---

_Comment by @zanieb on 2025-03-09 21:46_

I presume you're using the default backend (Hatchling)?

In which case, you may need to configure the target if it differs from the project name? https://hatch.pypa.io/1.9/config/build/#packages

(We'll have better answers for this once we have our own build backend — #3957)

---

_Comment by @Anselmoo on 2025-03-10 06:13_

@zanieb  thx for fast response

Correct, I used basig backend `Hatchling`. The main reason is that  [useful-math-functions](https://github.com/Anselmoo/useful-math-functions) will be come very long `useful_math_functions`, so I want to shorten it to `umf`. A little bit like [scikit-learn](https://github.com/scikit-learn/scikit-learn/tree/main/sklearn) becomes `sklearn`.

I hope it makes sense ...

---
