```yaml
number: 16598
title: "Allow `ruff analyze` to be run across a uv workspace packages"
type: issue
state: closed
author: mjclarke94
labels:
  - wish
  - analyze
assignees: []
created_at: 2025-03-10T11:00:30Z
updated_at: 2025-05-01T15:29:54Z
url: https://github.com/astral-sh/ruff/issues/16598
synced_at: 2026-01-12T15:54:55Z
```

# Allow `ruff analyze` to be run across a uv workspace packages

---

_@mjclarke94_

We currently have a monorepo setup where customer specific packages load in logic from several core (shared) libraries. It would be useful to be able to use ruff graph analyze to identify recursively what the minimum set of files from our other packages that is required to run the customer code package, enabling sparse checkouts.

This is also useful when wanting to run test suites in a way where tests which might be impacted by code changes are run first/exclusively.

There is a related feature in [Pants](https://www.pantsbuild.org/dev/docs/using-pants/advanced-target-selection#running-over-changed-files-with---changed-since) which would be a great feature to have parity with.

---

_Comment by @MichaReiser on 2025-03-10 11:07_

Hi @mjclarke94 

I'm not sure I understand the feature you're requesting, or it's unclear if you're requesting multiple features. Can you tell me more about how it's related to uv workspaces and how it is related to Pant's `changed-since` feature (I didn't see any mention of changed-since)?

It would also be useful to have a more concrete example on how you'd want to use it. E.g. what commands do you want to run to enable what?

---

_Label `needs-info` added by @MichaReiser on 2025-03-10 11:07_

---

_Label `analyze` added by @AlexWaygood on 2025-03-10 11:32_

---

_Comment by @mjclarke94 on 2025-03-10 12:33_

Sorry, I brain dumped a bit there and realize now there was a lot of context missing! Ok, taking the layout from the 
[albatross virtual workspace](https://github.com/astral-sh/uv/tree/aa629c4a54c31d6132ab1655b90dd7542c17d120/scripts/workspaces/albatross-virtual-workspace) as an example, where albatross depends on bird-feeder, which depends on seeds. 

Currently were I to do ` ruff analyze graph packages/albatross/` I'd get:
```
{
  "packages/albatross/check_installed_albatross.py": [
    "packages/albatross/src/albatross/__init__.py"
  ],
  "packages/albatross/src/albatross/__init__.py": []
}
```

So it correctly identifies the dependencies within the package. That said, `albatross/__init__.py` actually imports the workspace member `bird_feeder`.

So ideally, I would want to see:
```
{
  "packages/albatross/check_installed_albatross.py": [
    "packages/albatross/src/albatross/__init__.py"
    "packages/albatross/src/bird_feeder/__init__.py"
  ],
  "packages/albatross/src/albatross/__init__.py": [
     "packages/albatross/src/bird_feeder/__init__.py"
  ]
}
```

So feature request one would be something along the lines of "Can we have the graph analyze command work across workspaces rather than just within a single package." I don't know if this is something that exists in another form in `red-knot`, or even at a more granular level (say, understanding the hierarchical relationship at the function level rather than at the file level).

Once that exists, there's then the "why" element. One useful thing this enables is running test suites in a targeted way. Pants builds a similar graph of files across the whole repo, and then the `changed-since` flag enables you to filter this graph to only include files which have changed since the last git commit (or some other milestone). You might for example have modified a function and want to understand what tests feasibly could have been impacted by this change and only run those.

Again, a function level implementation of this would be even more powerful (not rerunning all tests downstream of a file just because someone updated a docstring).


---

_Comment by @MichaReiser on 2025-03-14 09:13_

Thanks, that makes sense to me. @AlexWaygood please correct me if I'm talking nonsense but the way this would be supported in Red Knot is that all packages would be linked as editable installs into your virtual environment (uv does that for you) and the virtual environment is part of the module resolvers search path. I'd have to play with `ruff analyze` but this might not be too hard to support if we add a configuration option for additional search paths (it would then also start picking up third party dependencies, unless you don't want that).

---

_Label `needs-info` removed by @MichaReiser on 2025-03-14 09:13_

---

_Label `wish` added by @MichaReiser on 2025-03-14 09:13_

---

_Comment by @mik-laj on 2025-04-07 11:37_

In the Apache Airflow project, we also encountered this problem. We use `ruff analyze graph` to detect invalid imports between provider packages as part of pre-commit hook - `check-imports-in-providers` . but after we started using uv workspace this hook no longer detects anything. For details, see: https://github.com/apache/airflow/issues/48870

---

_Comment by @MichaReiser on 2025-04-07 11:47_

CC: @charliermarsh 

---

_Comment by @MichaReiser on 2025-04-07 18:04_

@ntBre maybe one of the bug fixes you could look into. It's more an extension than a bug fix but it could help to unblock airflow and it shouldn't be too much work (a `--python` argument that takes a path to a venv and passing that along to red knot's module resolver should do the trick)

---

_Closed by @ntBre on 2025-05-01 15:29_

---
