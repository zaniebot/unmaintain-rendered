---
number: 14813
title: "F821 not recognizing `requires-python` in `pyproject.toml`"
type: issue
state: closed
author: CNSeniorious000
labels:
  - breaking
  - configuration
assignees: []
created_at: 2024-12-06T11:50:00Z
updated_at: 2025-03-14T08:59:14Z
url: https://github.com/astral-sh/ruff/issues/14813
synced_at: 2026-01-10T01:22:55Z
---

# F821 not recognizing `requires-python` in `pyproject.toml`

---

_Issue opened by @CNSeniorious000 on 2024-12-06 11:50_

I tried to use `anext` and `aiter` in a project configured with `requires-python = "~=3.10"`, but Ruff still complaining

```
F821 Undefined name `anext`. Consider specifying `requires-python = ">= 3.10"` or `tool.ruff.target-version = "py310"` in your `pyproject.toml` file.
  |
8 | async def main():
9 |     print(anext(g()))
  |           ^^^^^ F821
  |
```

Here is a reproduction repository:

https://github.com/CNSeniorious000/ruff-F821-repro

See these workflow logs:

https://github.com/CNSeniorious000/ruff-F821-repro/actions/runs/12198261365/job/34029609880#step:5:13

---

_Comment by @charliermarsh on 2024-12-06 11:55_

If you add `[tool.ruff]` (even followed by nothing else) to your `pyproject.toml`, then we'll read the `requires-python`.

---

_Label `question` added by @charliermarsh on 2024-12-06 11:55_

---

_Comment by @CNSeniorious000 on 2024-12-06 12:06_

Thank you! I'm surprised this workaround is needed.


---

_Comment by @MichaReiser on 2024-12-06 12:09_

So am I! I'd expect Ruff to pick up the `pyproject.toml` as the root even if it has no `tool.ruff` section and there's no other configuration in any ancestor directory. However, that's probably a breaking change 

---

_Label `configuration` added by @AlexWaygood on 2024-12-06 13:59_

---

_Comment by @looztra on 2025-01-17 10:38_

Does not work for me.

```toml
# in pyproject.toml
[tool.ruff]
# DO NOT PUT ANYTHING HERE, use the ruff.toml file, this is a fix for https://github.com/astral-sh/ruff/issues/14813
```

+ a ruff.toml file with all the config

target-version is still py39

```bash
╰─»  uv run ruff check --show-settings | grep target
linter.target_version = Py39
formatter.target_version = Py39
analyze.target_version = Py39
```

using ruff 0.9.2

---

_Comment by @looztra on 2025-01-17 10:46_

when I run `uv run check --show-settings --verbose` there's multiple lines saying

```text
[2025-01-17][11:45:48][ruff_workspace::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`
```

---

_Comment by @dhruvmanila on 2025-01-20 08:10_

@looztra Ruff has to pickup one configuration if there are multiple files present. The precedence is documented at https://docs.astral.sh/ruff/configuration/#config-file-discovery:

> If Ruff detects multiple configuration files in the same directory, the `.ruff.toml` file will take precedence over the `ruff.toml` file, and the `ruff.toml` file will take precedence over the `pyproject.toml` file.

Related https://github.com/astral-sh/ruff/pull/3470. Maybe we could use a separate logic for `target-version` specifically where the precedence is different i.e., `pyproject.toml`, `ruff.toml`, `.ruff.toml`.

---

_Label `question` removed by @dhruvmanila on 2025-01-20 08:10_

---

_Comment by @MichaReiser on 2025-01-20 08:48_

I think the solution here is to implement a configuration discovery similar to uv (and, in the future, red knot) where a `ruff.toml` takes precedence, but Ruff respects the projects `pyproject.toml`'s project table (if any). In Ruff's case, it would fall back to the `project.requires-python` if the `ruff.toml` doesn't specify a `target-version`. 

---

_Label `breaking` added by @dhruvmanila on 2025-01-20 09:00_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-02-11 21:51_

---

_Referenced in [astral-sh/ruff#16180](../../astral-sh/ruff/issues/16180.md) on 2025-02-16 10:28_

---

_Comment by @MichaReiser on 2025-02-16 10:33_

This has just come up again and seems to -- understandably -- be very confusing to users. This, unfortunately will be a breaking change and implementing it under preview is difficult because it happens during configuration loading (we don't know yet if the user has preview enabled). 

Either way, I think it would be good to have a fix ready for the next minor and, ideally, it would make the following two, but seperate, changes:

1. If we can't find any configuration but there's a `pyproject.toml` without a `tool.ruff` section, don't ignore it and instead use the `requires-python` from there
2. If we find a `ruff.toml` without a `target-version` fallback to the `pyproject.toml`'s `requires-python` at the same level (if there's any).


@dylwil3 @ntBre is this something either of you could look into? 

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-16 16:14_

---

_Comment by @dylwil3 on 2025-02-16 23:28_

@MichaReiser can you clarify the desired behavior of `ruff check foo/bar.py` in these situations? In each case, assume that `ruff.toml` is missing `target-version` and that `pyproject.toml` contains `requires-python` but is missing a `tool.ruff` section.

1.  
```
.
├── foo
│   ├── bar.py
│   └── ruff.toml
└── pyproject.toml
```

2. 
```
.
├── foo
│   ├── bar.py
│   └── pyproject.toml
└── ruff.toml
```

I realize that in your specification (2) you say that we should only look for a `pyproject.toml` "at the same level", but I just wanted to make sure since my guess is that users might expect `requires-python` to be respected as the `target-version` in both cases above.

---

_Comment by @MichaReiser on 2025-02-17 07:39_

That's a great call. We have to be careful that the change upholds the constraint that running ruff from the parent directory and from a child directory always results in the same results. 

1. No, the `pyproject.toml` here shouldn't apply because Ruff always uses the closest `ruff.toml` and it only inherits settings when explicitly using `extend`. I don't think we should change this behavior. 

2. This case is less clear to me but I think we should ignore the `pyproject.toml` in this case as well (because there's an ancestor `ruff.toml`). I could see this to be a common setup in mono repositories where you have multiple sub-projects but you want to have a single shared ruff configuration. Although, maybe this isn't that "clever" because it means that you have to list all project directories in the `src` setting or ruff won't resolve the first party imports correctly. But an example of this is airflow. They use a root [pyrpoject.toml](https://github.com/apache/airflow/blob/main/pyproject.toml) but also have multiple sub-projects (providers) where each has their own [pyproject.toml](https://github.com/apache/airflow/blob/e3aabea4c2d46599291e4dbcaad8efa83fd8a780/providers/microsoft/psrp/pyproject.toml#L4). We should make sure not to break this setup. 


---

_Referenced in [astral-sh/ruff#16319](../../astral-sh/ruff/pulls/16319.md) on 2025-02-22 19:14_

---

_Referenced in [astral-sh/ruff#16662](../../astral-sh/ruff/issues/16662.md) on 2025-03-12 05:11_

---

_Closed by @MichaReiser on 2025-03-13 11:51_

---

_Comment by @CNSeniorious000 on 2025-03-13 23:36_

Hey guys, it seems I'm still encountering the issue with ruff v0.10.0. What am I doing wrong?

https://github.com/CNSeniorious000/ruff-F821-repro/actions/runs/13846618684/job/38746272025

---

_Comment by @ntBre on 2025-03-13 23:42_

Hmm, I'm not sure. I cloned the repo (thanks for that, it's very helpful!) and ran ruff 0.10 with the `--verbose` flag, but I'm also not seeing anything helpful, as far as I can tell:

```
> ruff check -v
[2025-03-13][19:40:17][ruff::resolve][DEBUG] Using Ruff default settings
[2025-03-13][19:40:17][ignore::gitignore][DEBUG] opened gitignore file: /tmp/ruff-F821-repro/.gitignore
[2025-03-13][19:40:17][ignore::gitignore][DEBUG] opened gitignore file: /tmp/ruff-F821-repro/.git/info/exclude
[2025-03-13][19:40:17][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/tmp/ruff-F821-repro/.git"
[2025-03-13][19:40:17][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/ruff-F821-repro/hello.py"
[2025-03-13][19:40:17][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/ruff-F821-repro/pyproject.toml"
[2025-03-13][19:40:17][ignore::gitignore][DEBUG] opened gitignore file: /tmp/ruff-F821-repro/.ruff_cache/.gitignore
[2025-03-13][19:40:17][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/tmp/ruff-F821-repro/.ruff_cache"
[2025-03-13][19:40:17][ruff::commands::check][DEBUG] Identified files to lint in: 5.952225ms
[2025-03-13][19:40:17][ruff::diagnostics][DEBUG] Checking: /tmp/ruff-F821-repro/pyproject.toml
[2025-03-13][19:40:17][ruff::commands::check][DEBUG] Checked 2 files in: 143.454µs
hello.py:9:11: F821 Undefined name `anext`. Consider specifying `requires-python = ">= 3.10"` or `tool.ruff.target-version = "py310"` in your `pyproject.toml` file.
  |
8 | async def main():
9 |     print(anext(g()))
  |           ^^^^^ F821
  |

Found 1 error.
```

---

_Comment by @MichaReiser on 2025-03-14 08:13_

Yes, I'm sorry. It seems the relevant commit got dropped from the release branch. We'll follow up with a release later today. We only need to decide if it should be a patch or minor release

---

_Comment by @CNSeniorious000 on 2025-03-14 08:38_

Thanks for quick diagnosing! I personally suggest yanking v0.10.0 and publishing a patch release because this behavior is already documented in the 0.10.0 changelog.

---

_Comment by @MichaReiser on 2025-03-14 08:59_

I don't think yanking a release is justified here and we can easily update the changelog. See https://docs.pypi.org/project-management/yanking/ for PyPi's guidance on when to yank a release

---
