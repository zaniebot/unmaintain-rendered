---
number: 9043
title: Make docstring convention a global setting
type: issue
state: open
author: Mr-Pepe
labels:
  - configuration
  - help wanted
assignees: []
created_at: 2023-12-07T07:00:02Z
updated_at: 2024-09-13T13:28:34Z
url: https://github.com/astral-sh/ruff/issues/9043
synced_at: 2026-01-10T01:22:48Z
---

# Make docstring convention a global setting

---

_Issue opened by @Mr-Pepe on 2023-12-07 07:00_

Pydocstyle has a setting to specify the used docstring convention:

```
[tool.ruff.lint.pydocstyle]
convention = "google"
```

Pylint also has rules for docstrings and profits from setting the used convention. #8843 introduces one of those rules and adds a convention setting specific to Pylint.

I think that having two different settings for the convention is not very intuitive as a user and might hinder efforts to unify settings and rules later on if someone actually selects two different conventions for Pydocstyle and Pylint checks (no idea why someone would do that but there's always someone out there). Having a other tools, like a docstring formatter, in the future would further complicate things. The setting should therefore not only be global to linting, but global to all tools.

I propose to turn Pydocstyle's convention setting into a global setting to make it usable by the Pylint rules and other tools, like formatters. A strategy could look like the following:

- Add a `docstring-convention` setting under `[tool.ruff]`
- Deprecate the Pydocstyle setting (and Pylint's if #8843 gets merged in its current form)  in favor of the new setting
- Only mention the global setting in the docs
- Remove the original setting at some point in the future (is Ruff's deprecation process based on time or releases?)


---

_Referenced in [astral-sh/ruff#8843](../../astral-sh/ruff/pulls/8843.md) on 2023-12-07 07:02_

---

_Comment by @MichaReiser on 2023-12-07 07:36_

That makes sense to me. I wonder if it even should become a global (tool agnostic) setting, so that a docstring formatter could pick it up in the future.

---

_Comment by @Mr-Pepe on 2023-12-08 03:16_

I had to read through some issues and PRs (#8449, #7549, #7651) to figure out what the current state and trend regarding the settings structure is. Making the setting global to ruff and not only linting seems reasonable if there are plans to make use of the setting in the formatter. I have updated my original post. 

---

_Label `configuration` added by @dhruvmanila on 2023-12-08 05:27_

---

_Comment by @brendan-morin on 2024-01-14 17:06_

Probably related to this (let me know if this is a separate issue), but pydocstyle allows for specifying convention using it's own config:

```toml
[tool.pydocstyle]
convention = "google"
match = '((?!_test).)*\.py'
```

There is some inconsistently currently because (at least from what I can tell), ruff would in this case appropriately use the pydocstyle config for `match` but disregards the `convention` config. Instead, we need to also set `convention` in a ruff-specific config

```toml
[tool.ruff.lint.pydocstyle]
convention = "google"
```

Without knowing too much on ruff's philosophy towards configs, it feels like the path of least complexity would be to rely on and respect the pydocstyle configs where applicable, since that allows single-sourcing configs between the two tools. Right now the mix of some configs being respected and others not feels like it could lead to confusion and potentially duplication.

---

_Comment by @zanieb on 2024-03-12 18:42_

Generally our philosophy is that we will read Python standards i.e. `[project]` but not other tool's configuration formats.

---

_Comment by @brendan-morin on 2024-03-12 19:23_

It makes sense, I just ran into this reading another issue regarding e.g. `pip.conf` and I see where you're coming from. But this philosophy is not without downsides, unfortunately, which are duplication and possible confusing configs.

It feels like it could be worth issuing a warning from ruff if other/conflicting configs are detected in pyproject.toml. Even something like "this warning is just a heads up that what happens might be different than what you expected", just to get people used to this paradigm, since you'd have to do some issue diving to understand this nuance.

---

_Comment by @brendan-morin on 2024-03-12 19:26_

That said, there does seem to be some inconsistency - it's been a bit, but when I wrote that comment it appeared that ruff does use *some* of the configs from these other sections (`match` in this case), just not all of them.

---

_Comment by @zanieb on 2024-03-12 19:28_

Yeah unfortunately there are trade-offs in both directions. 

I'd be more willing to support a hint / warning if we detect the other configuration since then we're not as firmly on the hook for changes to the format. 

>  it appeared that ruff does use some of the configs from these other sections (match in this case), just not all of them.

I'm honestly not familiar if it does. If you know where we read that could you link to the code? Our philosophy around these things is ever-evolving of course.

---

_Comment by @charliermarsh on 2024-03-12 19:29_

> ... but when I wrote that comment it appeared that ruff does use some of the configs from these other sections (`match` in this case), just not all of them.

I'm very confident that we don't read `match` or anything from `tool.pydocstyle` -- I think it's ~impossible` since that's not encoded in our deserializer. Maybe there was something else going on?


---

_Comment by @brendan-morin on 2024-03-12 19:37_

Apologies -  you're correct. I had a separate config for ruff that was basically doing the same thing:
```toml
[tool.ruff.lint.per-file-ignores]
"test/*" = [
  "D"
]
```

---

_Label `help wanted` added by @zanieb on 2024-03-13 14:51_

---

_Comment by @zanieb on 2024-03-13 14:51_

Adding a global setting is open for contribution.

---

_Comment by @EwoutH on 2024-09-13 12:02_

What would be the steps needed to implement this?

---

_Comment by @MichaReiser on 2024-09-13 13:28_

@EwoutH 

* Add a global configuration option for the docstring convention
* Define and implement a merging logic that respects both the `pydocstyle` local and the global configuration 
* Possibly: Make it a preview only change.

---
