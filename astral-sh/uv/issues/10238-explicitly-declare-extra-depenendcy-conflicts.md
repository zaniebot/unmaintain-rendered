```yaml
number: 10238
title: Explicitly declare extra depenendcy conflicts
type: issue
state: closed
author: martinitus
labels:
  - question
assignees: []
created_at: 2024-12-30T12:58:38Z
updated_at: 2025-03-14T15:21:55Z
url: https://github.com/astral-sh/uv/issues/10238
synced_at: 2026-01-12T16:00:09Z
```

# Explicitly declare extra depenendcy conflicts

---

_@martinitus_

Hey  folks,
I have a followup question  / issue to the "conflicting extras / groups" features that were merged the last weeks (e.g. #8976).

I would like to declare conflicting extras - however these dependencies have no _inherent_ conflict in the package or version. Instead they happen to use the same module namespace  - `databricks-connect` and `pyspark` both use `pyspark` as module names.

So my goal is to have a package, for which the user has to explicitly enable either of the two extras, but cannot enable both.

What I tried so far was

```toml

[project.optional-dependencies]
databums = ["databricks-connect>=16.0.0",]
vanilla = ["pyspark>=3.5.4",]

[tool.uv]
conflicts = [
    [
        {extra = "databums" },
        {extra = "vanilla" },
    ]
]
```

However, when I run `uv lock` or `uv pip install -v -e  .[databums,vanilla]` or even just `uv pip -e .` it happily installs both packages (and screws up the site-package directory afaik).

I presume the current implementation only prevents scenarios where the dependency resolution would have failed due to conflicting optional dependencies. Whereas what I want is explicitly declaring dependencies as conflicting?

Let me know if I missed something and this should actually be possible!

BTW: I also tried it with optional groups, but groups are not exposed to pip (yet, afaik). As I want to be able to install my package with pip on downstream systems, dependency groups seem to not be an option yet.

---

_Comment by @charliermarsh on 2024-12-30 15:29_

These conflicts would only be enforced in `uv sync`, `uv run`, etc. They aren't currently enforced in the `uv pip` interface, since that interface doesn't use lockfiles.

---

_Label `question` added by @charliermarsh on 2024-12-30 15:29_

---

_Comment by @martinitus on 2025-01-02 09:49_

I see, just tried that and indeed when using `uv sync` and `uv run` I get part of the behavior I am looking for 👍🏼 

I guess, then I would turn my question into a feature request to also support this via the pip interface. Poetry finally supports this since https://github.com/python-poetry/poetry/pull/9553 was merged.

For reference, I have a small repo with poetry and uv test case here: 
 - poetry: https://github.com/martinitus/databums/tree/main
 - uv: https://github.com/martinitus/databums/tree/uv

It's not a huge deal, but rather an inconvenience, as switching between the two implementations in my experience usually is only necessary during development. However, it would be nice if one has the same user experience both on the developer side (uv sync & co) and the user side (pip install & co).

---

_Comment by @zanieb on 2025-01-06 20:33_

I don't quite see how we can support this. Your users could be using _any_ installer. I don't think this is enforceable with the existing standards.

---

_Comment by @martinitus on 2025-01-07 17:30_

Well, I have no idea how poetry did it but I'm quite sure it worked. Will double check this week... maybe it only works when installing via pip? What other installers would there be? Conda?

---

_Comment by @zanieb on 2025-01-07 17:46_

I don't see how it would work by installing via pip? The metadata is in a Poetry-specific section? Perhaps they construct some sort of metadata that does what you're looking for in the installable package — but then it'd have to be standards compliant which means it's feasible to do in uv too. Can you share a link to the published package?

---

_Comment by @zanieb on 2025-01-07 17:47_

It's possible the Poetry build backend enforces these relationships, which would work for `pip install -e .` and source distributions, I guess? but not installation from a wheel.

---

_Comment by @martinitus on 2025-03-14 15:16_

Hey there - sorry for the long delay. I finally came back to testing this - the GIST: You were correct, it seems it is not possible (with poetry) to enforce this as part of a published package! :-)

When installing the package (built with poetry) via 
`pip install /path/to/package.whl[databums,vanilla]`
pip indeed just installs both conflicting dependencies and as a result messes up the environment. 

For reference, the `METADATA` block of the wheel looks as follows:

```
Metadata-Version: 2.3
Name: spark-databums-conflict
Version: 0.1.0
Summary: Testing optional conflicting dependencies with poetry
Author: Martin Rueckl
Author-email: martin.rueckl@codecentric.de
Requires-Python: >=3.12,<4.0
Classifier: Programming Language :: Python :: 3
Classifier: Programming Language :: Python :: 3.12
Classifier: Programming Language :: Python :: 3.13
Provides-Extra: databums
Provides-Extra: vanilla
Requires-Dist: databricks-connect (==15.3.0) ; extra == "databums" and extra != "vanilla"
Requires-Dist: pyspark (==3.5.3) ; extra == "vanilla" and extra != "databums"
Description-Content-Type: text/markdown

... readme ...
```
What is left for me now is to figure out a way to achieve at least the same results with uv, but I think that is already possible with the changes from last year 👍🏼
 
Thank you for your explanations and a wonderful tool! ❤ 

---

_Closed by @martinitus on 2025-03-14 15:16_

---

_Comment by @charliermarsh on 2025-03-14 15:21_

Thank you for following up!

---
