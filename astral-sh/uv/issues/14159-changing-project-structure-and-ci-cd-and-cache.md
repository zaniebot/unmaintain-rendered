---
number: 14159
title: changing project structure and CI/CD and cache?
type: issue
state: open
author: gsal
labels:
  - question
assignees: []
created_at: 2025-06-20T15:26:11Z
updated_at: 2025-06-22T19:36:14Z
url: https://github.com/astral-sh/uv/issues/14159
synced_at: 2026-01-10T01:25:43Z
---

# changing project structure and CI/CD and cache?

---

_Issue opened by @gsal on 2025-06-20 15:26_

### Question

Hi, new user here....

A colleague started a uv project and put it through CI/CD; then, I joined in and started to work on the project, too, and refactored a bit...but uv continues to install the one file that existed in the original location before the refactoring. 

After a couple of tests, the concept of cache came to mind and sure enough, uv keeps cache around. Went to the docs, found `uv cache clean`, run it in my repo and now uv picks the correct file.  

Since this cache thing is on a per-user basis; in other words, I cleaning my own cache in ~/.cache/uv has not effect in the gitlab-runner.....What's the best practice to ensure that the CI/CD job does not get bitten by the same cache problem?  



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @gsal on 2025-06-20 15:26_

---

_Comment by @konstin on 2025-06-22 15:08_

Can you be more specific about how you included the file, and during which change you would like to see it invalidated? uv tries invalidates the cache for a package on all significant changes, so manual intervention should not be necessary.

---

_Comment by @gsal on 2025-06-22 18:56_

```
#Original project structure: 
./pyproject.toml    # [project.scripts] spin = spin:main
./uv.lock
./spin.py
```

```
#Next structure ./src style; 
#moved spin.py from top directory to ./src/spin/
./pyproject.toml    # [project.scripts] spin = spin.spin:main
./uv.lock
./src/spin/spin.py
```

When building or synching the virtual environment, uv kept bringing into the venv the `spin.py` that used to be in the top directory even though that one under `./src/spin` had aleady been modified  ( and added/committed/pushed to gitlab ).

Locally, it was only after a `uv clean cache`, that uv picked up `spin.py` from its new/only location. 

For the CI/CD pipeline, I started to test for the existence of the `GITLAB_CI` variable and adding the option `--no-cache` to the uv commands so that it does everything anew.


---

_Comment by @konstin on 2025-06-22 19:36_

uv refreshes a package whenever `pyproject.toml` changes, so I'm surprised the file keeps reappearing. What build backend are you using? What commands are you using, and what logs do they output (https://github.com/astral-sh/uv/issues/9452)?

---
