```yaml
number: 11557
title: "`uv build` with `hatchling` build backend doesn't include nested package in wheel"
type: issue
state: closed
author: Kayrnt
labels:
  - question
assignees: []
created_at: 2025-02-16T16:30:52Z
updated_at: 2025-02-16T18:41:06Z
url: https://github.com/astral-sh/uv/issues/11557
synced_at: 2026-01-12T16:00:40Z
```

# `uv build` with `hatchling` build backend doesn't include nested package in wheel

---

_@Kayrnt_

### Summary

# High level problem

I'm building a package using `uv build` and the resulting file doesn't contain the package source files when it's unzipped.
I noticed that it happened when I moved my python package from root directory into `src` directory and had to change the configuration to 
```
[tool.hatch.build]
packages = ["src/<my_package>"]
sources = ["src"]
```

# How to reproduce

Let's consider following project:
https://github.com/dbt-labs/dbt-adapters/tree/main/dbt-bigquery

Let's prepare it with `uv`:
```bash
uv sync
uv build
```

Resulting `dbt_bigquery-1.10.0a1-py3-none-any.whl` in `/dist` doesn't contain any source file ❌

Then if I try to use hatch directly:
```
uv add --dev hatch
uv run hatch build
```

Resulting `dbt_bigquery-1.10.0a1-py3-none-any.whl` in `/dist` **contains** source files as expected ✅

# Workaround

As shown previously, using `hatch` directly through `uv` works:
`uv run hatch build`

## Note

it wasn't working either with an older uv version so I suspect it's not a regression.

### Platform

MacOS 15.3 ARM64

### Version

uv 0.6.0 (591f38c25 2025-02-14)

### Python version

Python 3.11.1

---

_Label `bug` added by @Kayrnt on 2025-02-16 16:30_

---

_Comment by @zanieb on 2025-02-16 16:35_

Does this reproduce with the standard build frontend? `uvx --from build pypyproject-build`

Hatch special-cases its own build-backend and it is not uncommon for the behavior to be different.


---

_Comment by @Kayrnt on 2025-02-16 16:38_

Using `uvx --from build pyproject-build`, the produced artifact doesn't contain the expected source files either ❌

---

_Comment by @zanieb on 2025-02-16 16:39_

Yeah sorry, we're just acting as a standards-complaint build frontend here (same as the official `build` package).

---

_Comment by @zanieb on 2025-02-16 16:40_

Similar to https://github.com/astral-sh/uv/issues/10066

cc @ofek

---

_Label `bug` removed by @zanieb on 2025-02-16 16:40_

---

_Label `question` added by @zanieb on 2025-02-16 16:40_

---

_Comment by @ofek on 2025-02-16 16:52_

Same resolution as here: https://github.com/astral-sh/uv/issues/10391#issuecomment-2588165513

Try to never use the global `[tool.hatch.build]` table and prefer always the target-specific config like `[tool.hatch.build.targets.sdist]`. Defining `packages` or `sources` for the source distribution means the top-level directory is collapsed and therefore won't maintain the structure. You want:

```toml
[tool.hatch.build.targets.wheel]
packages = ["src/<my_package>"]

[tool.hatch.build.targets.sdist]
include = ["/src", "/tests"]
```

edit: it's important to note that the only reason you require any config at all is because (due to the naming convention of your package's ecosystem) the name of the package is different than the directory under `src`

---

_Comment by @Kayrnt on 2025-02-16 16:55_

@zanieb Reading through that issue (that I didn't find searching, thanks!), I can confirm that:
```
[tool.hatch.build]
only-include = ["src"]
```

~fixes the issue for my project.~
Edit: it is not producing the expected structure for the sdist artifact as described in https://github.com/astral-sh/uv/issues/11557#issuecomment-2661537345

It might be applicable to my own project but it can't be used on a project that is mainly built with the provided hatch working configuration.

Feel free to close the issue if it's not expected to support that kind of configuration.

@ofek, as you can see on https://github.com/dbt-labs/dbt-adapters/blob/main/dbt-bigquery/hatch.toml#L4-L10:
```
[build.targets.sdist]
packages = ["src/dbt"]
sources = ["src"]

[build.targets.wheel]
packages = ["src/dbt"]
sources = ["src"]
```

also doesn't work as expected with `uv build` but does with `hatch build`.

Is that kind of configuration wrong and the right way to do it should be:
```
[build.targets.sdist]
include = ["/src"]

[build.targets.wheel]
packages = ["src/dbt"]
````
?

---

_Comment by @ofek on 2025-02-16 16:59_

> Try to never use the global `[tool.hatch.build]` table and prefer always the target-specific config like `[tool.hatch.build.targets.sdist]`. Defining `packages` or `sources` for the source distribution means the top-level directory is collapsed and therefore won't maintain the structure.

Here is the example configuration I gave you, replacing `<my_package>` with the actual directory name:

```toml
[tool.hatch.build.targets.wheel]
packages = ["src/dbt"]

[tool.hatch.build.targets.sdist]
only-include = ["src", "tests"]
```

---

_Closed by @charliermarsh on 2025-02-16 17:02_

---

_Comment by @charliermarsh on 2025-02-16 17:02_

(Thanks @ofek!)

---

_Comment by @Kayrnt on 2025-02-16 17:05_

I confirm that:
```
[tool.hatch.build.targets.wheel]
packages = ["src/dbt"]

[tool.hatch.build.targets.sdist]
only-include = ["src", "tests"]
```

works as intended and produced the same artifacts for `uv` and `hatch` 👍
I'll suggest the change on the linked repository, thanks @ofek & @zanieb!

---

_Comment by @Kayrnt on 2025-02-16 17:17_

@ofek Actually unzipping the sdist artifact (`<package>.tar.gz`) will put directly the `src` directory instead of `<package>` directory (which is inside of src directory). Yet the wheel is correctly structured. I guess it still miss something for `sdist` configuration. I'm trying to figure it out.

---

_Comment by @ofek on 2025-02-16 17:18_

The behavior you're seeing is correct as you would want the source distribution structure to match the repository.

edit: perhaps this would be better, could you please articulate in your own words why the config fixed your issue?

---

_Comment by @Kayrnt on 2025-02-16 17:31_

Ok I'm not a Python expert so I assume that existing built files are producing the right file structures.
Let's take https://github.com/dbt-labs/dbt-adapters/blob/main/dbt-bigquery/hatch.toml#L4-L10 as the baseline that produce with hatch (after extracting the artifacts):
```
.
├── dbt_bigquery-1.10.0a1
│   └── dbt
└── dbt_bigquery-1.10.0a1-py3-none-any
    ├── dbt
    └── dbt_bigquery-1.10.0a1.dist-info
```

If I go for:
```
[tool.hatch.build.targets.wheel]
packages = ["src/dbt"]

[tool.hatch.build.targets.sdist]
only-include = ["src"]
```

it results in:
```
.
├── dbt_bigquery-1.10.0a1
│   └── src
│       └── dbt
└── dbt_bigquery-1.10.0a1-py3-none-any
    ├── dbt
    └── dbt_bigquery-1.10.0a1.dist-info
```

Then I suspect that though the second configuration is working with both `uv build` and `uv run hatch build`, it is not producing the artifacts that will be expected (src instead dbt at the root of the sdist artifact). 

---

_Comment by @ofek on 2025-02-16 17:38_

Yes basically there are two ways to produce a wheel: either from a repository directly or from building inside an unpacked source distribution. Unless instructed otherwise, UV does the latter while Hatch does the former.

In the case of your old config, Hatchling was instructed to strip off the `src` directory for source distributions just like the wheel. Thus when a frontend builds a wheel via an unpacked source distribution there is no more `src` directory and only what is underneath. Since the wheel target was configured to look for the `src` directory and it no longer exists then weird things happen.

---

_Comment by @Kayrnt on 2025-02-16 17:47_

I kinda understand what you mean.

Do you think there could be a hatchling configuration that produces:
```
.
├── dbt_bigquery-1.10.0a1
│   └── dbt
└── dbt_bigquery-1.10.0a1-py3-none-any
    ├── dbt
    └── dbt_bigquery-1.10.0a1.dist-info
```

and both works with `uv` and `hatch`?
It feels like it's not possible but I might miss the right understanding of how both systems work.

---

_Comment by @ofek on 2025-02-16 17:51_

No that would not be possible and I'm not sure why you would want the source distribution to match the structure of the wheel. There is no installer in existence that populates a Python environment based on the contents of only the source distribution. A wheel is first produced from a source distribution and then used to populate an environment.

---

_Comment by @Kayrnt on 2025-02-16 18:12_

I don't really expect the `sdist` and `wheel` structure to be the same, I expect that both provide the package at the root level of their respective artifact. I think that's the expected file structure to have `<my_package>` at the root level and no `src`.

> There is no installer in existence that populates a Python environment based on the contents of only the source distribution.

Maybe it's an useless requirement since I'm producing and pushing the wheel to PyPI (which is probably going to be used in 99.99% of the cases). I feel like that if it's currently working with `hatch` using its configuration, it should also work with `uv`.

Maybe we could assume that it's not possible and it won't be "fixed" but maybe the [upcoming uv build backend](https://github.com/astral-sh/uv/issues/3957) will provide a way to produce correctly both sdist & wheel with the appropriate file structures. In which case, I could use it for my own project and suggest other OSS to move to uv.

---

_Comment by @ofek on 2025-02-16 18:25_

> maybe the [upcoming uv build backend](https://github.com/astral-sh/uv/issues/3957) will provide a way to produce correctly both sdist & wheel with the appropriate file structures

It won't produce a different structure and I don't know how else to convey the reasons why. Perhaps @charliermarsh or @zanieb could provide assistance here in explaining the difference between a source distribution and wheel.

---

_Comment by @Kayrnt on 2025-02-16 18:40_

I read https://packaging.python.org/en/latest/discussions/package-formats/ to try to understand both.
It's not explicit if it should be mapping the repository file structure or if it should map the package structure.
Asking ChatGPT, it points out it should map the repository structure, in which case, the configuration you suggested and that works with both tools is the "right one".

---
