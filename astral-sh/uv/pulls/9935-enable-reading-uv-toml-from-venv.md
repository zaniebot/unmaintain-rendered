```yaml
number: 9935
title: "Enable reading `uv.toml` from venv"
type: pull_request
state: open
author: JonneDatanen
labels:
  - needs-decision
assignees: []
base: main
head: read-config-from-venv
created_at: 2024-12-16T13:38:36Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9935
synced_at: 2026-01-12T16:09:03Z
```

# Enable reading `uv.toml` from venv

---

_@JonneDatanen_

Resolves: https://github.com/astral-sh/uv/issues/8107
<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

  Sometimes I would like to have per-venv `uv.toml` so that there could be e.g. multiple venv's with different constraints where different projects are run/tested. System/user wide configuration forbids doing many venv's and project settings doesn't allow running with different set of constraints.
  
Having configuration files within the venv is also a feature that pip supports: https://pip.pypa.io/en/stable/topics/configuration/#location
  
## Test Plan

Manually tested by creating venv and adding `uv.toml` there -> constraints were resolved correctly. 
<!-- How was it tested? -->


---

_Label `needs-decision` added by @zanieb on 2024-12-17 15:45_

---

_Comment by @zanieb on 2024-12-17 15:46_

I think we're pretty hesitant to do this. When we recreate a virtual environment, we delete the entire directory. It doesn't seem like a good place for additional state, like settings.

What does

> project settings doesn't allow running with different set of constraints

mean?

---

_Comment by @JonneDatanen on 2024-12-17 16:03_

> What does
> 
>  >   project settings doesn't allow running with different set of constraints
> 
> mean?

We have some "fixed" python production environments. E.g.: 
- prod-env0 -> python==3.8, numpy==1.17.0, pandas==0.24.0, ...
- prod-env1 -> python==3.10, numpy==1.24.4, pandas==1.4.4, ...
- prod-env2 -> python==3.12, numpy==2.13.0, pandas==2.2.2, ...
- etc...

And then we have e.g. python package `foo` that we ci-test and guarantee to run smoothly in e.g. env1 and env2.

That means that we want to give some set of hard constraints while installing `foo`, but:
- They cannot be in `pyproject.toml` as the constraints are different based on the env we are testing/running the package
- They cannot be in `~/.config/uv.toml` as the same user might want to test in different venv's

So for our use case it is better to have venv-specific configs, that can be generated (with script, `prod-env init 0`) after I create new venv. Currently we have that kind of script for `pip`, but I would definitely like to switch to `uv pip` if pip-like venv specific uv.toml would be possible.

Of course this doesn't maybe fit nicely to `uv sync`/`uv lock` way of working, but having this in `uv pip` would be nice

---

_Comment by @JonneDatanen on 2025-02-25 06:46_

@zanieb Any updates on the decision? I'm looking forward to implementing uv in our workflows once this is approved.

---

_Comment by @zanieb on 2025-02-26 22:36_

I don't think we want to do this, it's a core principle that virtual environments are ephemeral and disposable. Maybe we can find some other way for you to override settings per target environment. It sort of overlaps with #5903 

---

_Comment by @larstiq on 2025-02-27 12:17_

> it's a core principle that virtual environments are ephemeral and disposable

Is the problem then that as `uv` may delete and recreate the virtual environment at any time there is no guarantee that the constraints will be recreated at the right time and thus considered when doing installs?

Our current steps are:
- create venv
- install package with all possible environmental constraints
- run `initialize environment X`
- pip install the rest of the packages

Whereas with `uv` that's all one step?  https://github.com/astral-sh/uv/issues/5903 looks quite different to me. Although there is mention of `environment` people seem to focus on running scripts. What we are looking for is more like composing constraints from different environments with project specific constraints.

---
