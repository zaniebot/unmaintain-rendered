```yaml
number: 10299
title: "FA102 errors eventhough requires-python in pyproject.toml is \">=3.12\""
type: issue
state: closed
author: SimonMarynissen
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-03-08T15:26:33Z
updated_at: 2025-06-09T05:22:16Z
url: https://github.com/astral-sh/ruff/issues/10299
synced_at: 2026-01-10T11:09:52Z
```

# FA102 errors eventhough requires-python in pyproject.toml is ">=3.12"

---

_Issue opened by @SimonMarynissen on 2024-03-08 15:26_

I have the following (standard) directory layout:

```
├── src
│   └── packagename
│       └── ...
├── pyproject.toml
└── ruff.toml
```

Part from my `pyproject.toml`:

```toml
[project]
name = "packagename"
requires-python = ">=3.12"
```

My complete `ruff.toml`:

```toml
src = ["src"]

[format]
skip-magic-trailing-comma = true
line-ending = "lf"

[lint]
select = ["ALL"]
ignore = ["ANN101", "D203", "D213", "S320"]
```

Ruff version: 0.3.0, run with the command `ruff check` in the directory of `pyproject.toml`.
In the rule [FA102](https://docs.astral.sh/ruff/rules/future-required-type-annotation/), it is mentioned:
> This rule respects the [target-version](https://docs.astral.sh/ruff/settings/#target-version) setting. For example, if your project targets Python 3.10 and above, adding from __future__ import annotations does not impact your ability to leverage PEP 604-style unions (e.g., to convert Optional[str] to str | None). As such, this rule will only flag such usages if your project targets Python 3.9 or below.

In the documentation of `target-version` it is mentioned that `requires-python` in `pyproject.toml` is preferred over the `target-version` option in `ruff.toml`.
If I add `target-version = "py312"` to my `ruff.toml`, then there are no FA102 errors.


---

_Comment by @charliermarsh on 2024-03-08 23:49_

I think it's possible that if we find a `ruff.toml`, we don't even look for the `pyproject.toml`, since we only use a single configuration file to resolve configuration for the project. So you _could_ fix this (I suspect) by moving your configuration into your `pyproject.toml`, like:

```toml
[project]
name = "packagename"
requires-python = ">=3.12"

[tool.ruff]
src = ["src"]

[tool.ruff.format]
skip-magic-trailing-comma = true
line-ending = "lf"

[tool.ruff.lint]
select = ["ALL"]
ignore = ["ANN101", "D203", "D213", "S320"]
```

But, we should probably also be respecting the `pyproject.toml` here.

---

_Label `bug` added by @charliermarsh on 2024-03-08 23:49_

---

_Label `configuration` added by @charliermarsh on 2024-03-08 23:49_

---

_Comment by @charliermarsh on 2024-03-08 23:51_

(If possible, can you give that a try to confirm my suspicion?)

---

_Comment by @SimonMarynissen on 2024-03-12 16:28_

That does indeed work, but I would prefer to have a separate `ruff.toml`.

---

_Comment by @MichaReiser on 2024-03-18 14:48_

@charliermarsh considering that you marked this as a bug. What would the desired resolution order be?

The documentation mentions

> Ruff can be configured through a pyproject.toml, ruff.toml, or .ruff.toml file.

which seems like `pyproject.toml` intentionally has higher precedence than `ruff.toml`. Changing the resolution order would probably be a breaking change. 

Or is the fix that we should disregard any `pyproject.toml` without any `tool.ruff` section (but that could lead to other false positives because we might disregard a `requires-python` configuration). That makes me think that the only "proper" solution is to merge the configurations, but that might lead to other unintended or unexpected behavior.

---

_Comment by @charliermarsh on 2024-03-18 14:54_

> Or is the fix that we should disregard any pyproject.toml without any tool.ruff section (but that could lead to other false positives because we might disregard a requires-python configuration).

We already do this IIRC.

---

_Comment by @charliermarsh on 2024-03-18 14:55_

> That makes me think that the only "proper" solution is to merge the configurations, but that might lead to other unintended or unexpected behavior.

Yeah fixing this _requires_ that we merge configuration in some way, because the user is inherently asking for information to come from two different files. So we either do something like "when we see a `ruff.toml`, check for a `pyproject.toml` in the same directory and use its `requires-python`", or mark it as unsupported.

---

_Comment by @SimonMarynissen on 2024-03-21 18:18_

If I may add my 2 cents: if the precedence is `pyproject.toml` > `ruff.toml` > `.ruff.toml`, then `pyproject.toml` is always parsed (if it exists) and if no ruff elements are found, I guess you either get bad defaults or an error (I am not too familiar with ruff), so it shouldn't be big change to look for `ruff.toml` in that case. That would certainly solve my case and would most likely not break workflows from other ruff users.

---

_Comment by @DimitriPapadopoulos on 2024-04-09 19:29_

Reproduced in https://github.com/sphinx-doc/sphinx/pull/12250. Removing `target-version = "py39"` from `.ruff.toml` breaks the tests as `requires-python = ">=3.9"` from `pyproject.toml` is **not** taken into account.

I think ruff should take into account `requires-python"` from `pyproject.toml`, alternatively fix the documentation:
https://github.com/astral-sh/ruff/blob/42d52ebbec275389ee0458a3bf373268278bd51a/crates/ruff_workspace/src/options.rs#L314-L317

---

_Comment by @DimitriPapadopoulos on 2024-04-15 08:08_

Same issue in https://github.com/jaraco/skeleton/issues/119. The situation is even worse here, as `requires-python` is defined in `setup.cfg` instead of `pyproject.toml`.

The rationale for a `ruff.toml` file separate from `pyproject.toml` is described here:
https://github.com/pypa/setuptools/pull/3979#pullrequestreview-1691482008

---

_Comment by @DimitriPapadopoulos on 2024-04-15 08:18_

In the documentation, [Config file discovery](https://docs.astral.sh/ruff/configuration/#config-file-discovery) reads:
> Unlike [ESLint](https://eslint.org/docs/latest/user-guide/configuring/configuration-files#cascading-and-hierarchy), Ruff does not merge settings across configuration files; instead, the "closest" configuration file is used, and any parent configuration files are ignored. In lieu of this implicit cascade, Ruff supports an [`extend`](https://docs.astral.sh/ruff/settings/#extend) field, which allows you to inherit the settings from another config file, like so:
> 
> [**pyproject.toml**](https://docs.astral.sh/ruff/configuration/#__tabbed_4_1) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [**ruff.toml**](https://docs.astral.sh/ruff/configuration/#__tabbed_4_2)
> ```ruff.toml
> # Extend the `ruff.toml` file in the parent directory...
> extend = "../ruff.toml"
> 
> # ...but use a different line length.
> line-length = 100
> ```
> All of the above rules apply equivalently to `pyproject.toml`, `ruff.toml`, and `.ruff.toml` files. If Ruff detects multiple configuration files in the same directory, the `.ruff.toml` file will take precedence over the `ruff.toml` file, and the `ruff.toml` file will take precedence over the `pyproject.toml` file.

I was thinking about adding `extend = "pyproject.toml"` to `ruff.toml` to pick `requires-python` from `pyproject.toml`, but this doesn't make sense as the namespaces and the way the files are parsed are different.

---

_Comment by @DimitriPapadopoulos on 2024-04-15 08:40_

My suggestion would be to:
1. Keep a single configuration file for _ruff_ itself, that is, disregard the `[tool.ruff]` subtable in `pyproject.toml` if files `.ruff.toml` or `ruff.toml` exist - or _ruff_ is passed option `--config`.
2. Take into account other tables of `pyproject.toml` that contain keys that impact _ruff_, typically the `[project]` table and its `requires-python` key.
3. Support  keys of interest in both `setup.cfg` and `pyproject.toml`. Not sure about precedence in the ill-formed case where `requires-python` would be defined in both `setup.cfg` and `pyproject.toml`.
4. Keep the existing precedence if `pyproject.toml` or `setup.cfg` define `requires-python` and  the _ruff_ config file defines `target-version` simultaneously: the _ruff_ config file and `target-version` take precedence.

---

_Comment by @kubotty on 2024-10-28 03:06_

Hi, I encountered the same issue and I found this solution. 
This is what is mentioned: 
> I was thinking about adding `extend = "pyproject.toml"` to `ruff.toml` to pick `requires-python` from `pyproject.toml`, but this doesn't make sense as the namespaces and the way the files are parsed are different.

However, I tried writing `extend = "./pyproject.toml"` in `ruff.toml` and it worked.

---

_Comment by @jaraco on 2024-10-28 16:01_

I'd like to make the case that ruff should not be relying on declared configuration and should instead rely on the [core metadata](https://packaging.python.org/en/latest/specifications/core-metadata/#requires-python) for the package. Although `pyproject.toml` is a standard for a source distribution to declare metadata, it's not (and should not) be required that the package's metadata be declared through pyproject.toml. As an extreme example, the [coherent build system](https://bit.ly/coherent-system) doesn't require any pyproject.toml and instead derives all of the metadata (including the `Requires-Python` declaration) from environmental state, specifically the [tags on the CPython repository in GitHub](https://github.com/coherent-oss/coherent.build/blob/06a28adab87c2bdc6eef6c16ac3b722d443a7d2e/discovery.py#L151-L166).

For this reason, the supported/required Python versions for a Coherent project or any other build system that dynamically or programmatically generates that metadata field won't have a declaration in the `pyproject.toml` nor `ruff.toml`.

There are other build backends that produce "dynamic" values for various metadata fields, so addressing just the Requires-Python field will leave those fields unaddressed.

There's clear value for projects in not having to statically manifest all of their metadata in source code (and maintain that state, possibly shared across or even diverging within many projects).

My recommendation would be for Ruff to generate the package metadata through the [PEP 517 `prepare_metadata_for_build_wheel` hook](https://peps.python.org/pep-0517/#prepare-metadata-for-build-wheel) and then read the `Requires-Python` field.

I know this approach can be potentially expensive (and for Coherent projects is currently _very_ expensive, but that's largely the backend's problem), so it may be necessary to cache the value (or the metadata) for extended periods.

This approach would have added benefits that _other_ core metadata can be resolved by ruff, even when it's dynamic.

Such an approach would sidestep workarounds like `extend = "pyproject.toml"` or duplicating the metadata in two files and would honor the essential metadata of the package.

---

_Comment by @Avasam on 2024-10-30 19:11_

~~As mentioned in https://github.com/pypa/setuptools/pull/4718#issuecomment-2448099867, it seems `include = ["pyproject.toml"]` doesn't really work as a true workaround, it just hides version-related errors, even if `requires-python` is set in the `pyproject.toml`, even if you explicitly pass the flag by CLI: `ruff check --select=UP035 --target-version=py39`~~

Edit: because it should be `extend = "pyproject.toml"`, not `include`...

---

_Comment by @jaraco on 2024-12-24 14:48_

> workarounds like `extend = "pyproject.toml"`

I recently learned that this workaround isn't as elegant as I'd originally expected (see jaraco/skeleton#155). While it does work for skeleton-based projects, the addition of the `extend` directive means that `ruff.toml` file is no longer portable, no longer usable in other contexts.

This finding is yet another factor in favor of simply honoring the value from core metadata.

---

_Comment by @MichaReiser on 2024-12-24 16:20_

We should probably do the same as for uv and what's planned for Red Knot. 

If there's both a `ruff.toml` and `pyproject.toml`, use the `ruff.toml` but fallback to the `pyproject.toml`'s `requires-python` if the `target-version` is left unspecified. 

---

_Comment by @Winand on 2025-02-12 08:01_

I've installed ruff as an extension for VS Code. Where are settings read from if I modify them via VS Code settings? They are in my profile's _settings.json_ file and `requires-python` in _pyproject.toml_ is ignored.

---

_Comment by @MichaReiser on 2025-02-12 08:10_

> I've installed ruff as an extension for VS Code. Where are settings read from if I modify them via VS Code settings? They are in my profile's _settings.json_ file and `requires-python` in _pyproject.toml_ is ignored.

Could you please open a new issue that contains more details about your setup. What VS code settings do you use, what's in your pyproject.toml. It's otherwise very hard for us to help you. You can reference this issue if you think it's related.

---

_Comment by @Winand on 2025-02-12 09:41_

> Could you please open a new issue that contains more details about your setup. What VS code settings do you use, what's in your pyproject.toml. It's otherwise very hard for us to help you. You can reference this issue if you think it's related.

But I have exactly the same issue: FA102 errors eventhough requires-python in pyproject.toml is ">=3.12"

Now I understand why it's like that:
1. [configurationPreference](https://docs.astral.sh/ruff/editors/settings/#configurationpreference) Default value: "editorFirst"
2. ["Ruff ignores any pyproject.toml files that lack a [tool.ruff] section"](https://docs.astral.sh/ruff/configuration/#config-file-discovery)
3. "If no config file is found in the filesystem hierarchy, Ruff will fall back to using a default configuration"
4. [target-version](https://docs.astral.sh/ruff/settings/#target-version) Default value: "py39"

So the configuration from _settings.json_ is applied first, then ruff tries to find a config file but ignores _pyproject.toml_ due to the lack of a _[tool.ruff]_ section in it. The **solution** here is to add an empty _[tool.ruff]_ section to my _pyproject.toml_.

Another solution is to add `"ruff.configuration": "pyproject.toml",` to _settings.json_ but I like it less.

---
But really I would prefer if settings from all configurations are merged together taking their precedence into account:
Editor settings > _pyproject.toml_ > _ruff.toml_ > etc.

---

_Comment by @DimitriPapadopoulos on 2025-02-12 09:56_

@Winand I may be wrong, but I think this issue is specifically about taking into account `requires-python` from `pyproject.toml` when using `ruff.toml`. If so, your issue seems slightly different.

---

_Comment by @Winand on 2025-02-12 10:16_

> about taking into account `requires-python` from `pyproject.toml` when using `ruff.toml`

yes, but my issue is "about taking into account `requires-python` from `pyproject.toml` when using **default settings**" which looks like pretty much the same problem. That's why I don't want to create a separate issue.

---

_Comment by @MichaReiser on 2025-02-12 11:51_

@Winand whether you want to open a new issue depends on your goal:

* if you want help, please open a new issue. 
* if you don't need help and you only want to document another use case where this behavior is problematic, commenting on this issue is the right place.

---

_Comment by @Avasam on 2025-03-17 05:17_

Is this solved by https://github.com/astral-sh/ruff/pull/16319 ?

---

_Closed by @MichaReiser on 2025-03-17 07:35_

---

_Comment by @kiyoon on 2025-06-09 05:06_

It still doesn't work with mono-repo styled structure.  
I'd like to keep my repo structured like this:

```
ruff.toml  # every ruff settings
project-that-requires-py3.9/
    - pyproject.toml  # have requires-python >= 3.9
project-that-requires-py3.12/
    - pyproject.toml  # have requires-python >= 3.12
```

So my ruff lint configuration is not dependent to the python version my projects use, but it doesn't pick up the requires-python and I don't know a correct way to make ruff understand the python version of the project except for having all ruff settings separate to each project.

---

_Comment by @MichaReiser on 2025-06-09 05:14_

You can use [`per-file-target-version`](https://docs.astral.sh/ruff/settings/#per-file-target-version) or `extend` with `target-version`

---

_Comment by @kiyoon on 2025-06-09 05:22_

Thank you. I used `extend` like this and it works.

```toml
# project-that-requires-py3.9/pyproject.toml
[tool.ruff]
extend = "../ruff.toml"
```

---
