---
number: 15249
title: "Ability to configure `override-dependencies` for  a specific dependency group"
type: issue
state: open
author: ramonvermeulen
labels:
  - enhancement
assignees: []
created_at: 2025-08-13T07:55:04Z
updated_at: 2025-09-19T15:43:33Z
url: https://github.com/astral-sh/uv/issues/15249
synced_at: 2026-01-10T01:25:54Z
---

# Ability to configure `override-dependencies` for  a specific dependency group

---

_Issue opened by @ramonvermeulen on 2025-08-13 07:55_

### Summary

Currently you are able to configure override-dependencies for your whole project, see:
https://docs.astral.sh/uv/reference/settings/#override-dependencies

For example:

```toml
[tool.uv]
# Always install Werkzeug 2.3.0, regardless of whether transitive dependencies request
# a different version.
override-dependencies = ["werkzeug==2.3.0"]
```

It would be nice if this can be configured for a specific dependency group, so for example when you do a `uv sync --no-dev` and the override-dependency is only needed for a dev dependency it can be skipped during installation.

### Example

Potential configuration:

```toml
[tool.uv]
# Always install Werkzeug 2.3.0, regardless of whether transitive dependencies request
# a different version.
override-dependencies = [{ group = "dev", deps = ["werkzeug==2.3.0"] }]
```

Then a `uv sync` will install the `override-dependency`, however a `uv sync --no-dev` will not because it is specifically for the `dev` group.

---

_Label `enhancement` added by @ramonvermeulen on 2025-08-13 07:55_

---

_Comment by @PeganovAnton on 2025-09-19 10:34_

Could you please implement it? It would be extremely useful for our project.

---

_Comment by @konstin on 2025-09-19 10:49_

Please check with the uv team before implementing such a complex feature.

You can currently emulate this conflicting dependencies, so you can use a different werkzeug version for different groups.

---

_Comment by @PeganovAnton on 2025-09-19 11:05_

@konstin my use case is slightly different. Within **the same** group I have disjoint dependencies:
```
# The Pydantic override is added because of dependency conflict between openai-harmony
    # and ai-dynamo:
    # ai-dynamo-runtime==0.4.0+8f24c027 depends on pydantic>=2.10.6,<2.11.0 and openai-harmony==0.0.4 depends on pydantic>=2.11.7
```
I cannot resolve it by upgrading or downgrading 1 of the packages, so I must use `override-dependencies`.
I resolve the requirement in the dependencies:
```
dependencies = [
    "ai-dynamo-runtime==0.4.0+8f24c027",
    "openai-harmony==0.0.4",
    "pydantic==2.11.7",
]
```
and add it into the `override-dependencies`:
```
override-dependencies = [
    "pydantic==2.11.7",
]
```

But I want the resolution of such conflicts to be group specific.

---

_Comment by @PeganovAnton on 2025-09-19 11:18_

@konstin For the same reason, `constraint-dependencies` and `build-constraint-dependencies` are better to make group specific as well.

---

_Comment by @PeganovAnton on 2025-09-19 15:23_

Hi @konstin @charliermarsh @zanieb , could you please tell whether you plan to implement the feature?

---

_Comment by @konstin on 2025-09-19 15:26_

Please don't ping people from the team asking for updates, instead, you can leave a :+1: reaction on the top post (https://github.com/astral-sh/uv/issues/9452). This is a complex feature and not on our immediate roadmap.

---

_Comment by @PeganovAnton on 2025-09-19 15:30_

@konstin If I implement it what do I need to take into account?

---

_Comment by @konstin on 2025-09-19 15:43_

This feature is very complex from the perspective of the resolver, as group-specific overrides effectively mean conflicting dependencies, where both overrides and conflicting dependencies are already error prone parts of uv. Before we implement something this complex, we need enough motivating use cases, which details on the project structure and why existing workarounds or less complex additions don't work. After that, we need a design for the configuration that is consistent with our existing design principals. Currently, I don't think we can add such a feature. If you want to move this forward, please share a minimal reproducible example (ideally with the production dependencies) that you cannot solve with the current uv feature set, with an explanation on what workflows you want to use and why you need those workflows, context that allows to understand if there may be alternative solutions for your situation.

---
