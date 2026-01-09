---
number: 7875
title: "uv changes my package version when doing \"uv build --sdist\" or \"uv lock\""
type: issue
state: closed
author: cmalek
labels:
  - question
assignees: []
created_at: 2024-10-02T17:46:34Z
updated_at: 2024-10-02T18:10:22Z
url: https://github.com/astral-sh/uv/issues/7875
synced_at: 2026-01-07T13:12:17-06:00
---

# uv changes my package version when doing "uv build --sdist" or "uv lock"

---

_Issue opened by @cmalek on 2024-10-02 17:46_

I'm using `uv` version 0.4.16.   

We're currently in the process of switching from straight `pip` to `uv`.  We've been happily using semver for our versioning for a long time, and I'm having issues with pre-release versions and `uv`

https://semver.org/ says about pre-release versions:

> A pre-release version MAY be denoted by appending a hyphen and a series of dot separated identifiers immediately following the patch version. Identifiers MUST comprise only ASCII alphanumerics and hyphens [0-9A-Za-z-]. Identifiers MUST NOT be empty. Numeric identifiers MUST NOT include leading zeroes. Pre-release versions have a lower precedence than the associated normal version. A pre-release version indicates that the version is unstable and might not satisfy the intended compatibility requirements as denoted by its associated normal version. Examples: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92, 1.0.0-x-y-z.--.

So if I have a pre-release version id named `dev` and this is the first `dev` pre-release of 0.1.0, my version would be (according to the above) `0.1.0-dev.1`

In my `pyproject.toml`, I have:

```
[project]
name = "myproject"
version = "0.1.0-dev.1"
...
```

If I do a `uv lock` I get:

```
$ uv lock
Resolved xx packages in 384ms
Updated myproject v0.1.0 -> v0.1.0.dev1
```

Note that it changed my version number string.  `uv build` does the same thing.

I have a lot of CI/CD tooling that depends on the version numbers being strict semver, and this breaks that.

Is this intentional?  I would prefer if it did not mess with my version numbering.

---

_Comment by @zanieb on 2024-10-02 18:00_

Note we follow Python [version specification standards](https://packaging.python.org/en/latest/specifications/version-specifiers/) which were originally defined in [PEP 440.](https://peps.python.org/pep-0440/)

~This looks like a bug though, as `-` should be a valid [separator for pre-releases](https://packaging.python.org/en/latest/specifications/version-specifiers/#pre-release-separators)~

---

_Comment by @zanieb on 2024-10-02 18:04_

Ah actually this is correct, as development releases are separate from pre-releases.

The [canonical specification](https://packaging.python.org/en/latest/specifications/version-specifiers/#public-version-identifiers) which we _must_ normalize to is:

> [N!]N(.N)*[{a|b|rc}N][.postN][.devN]

And [normalization for development releases](https://packaging.python.org/en/latest/specifications/version-specifiers/#development-release-separators) is as follows:

> Development releases allow a ., -, or a _ separator as well as omitting the separator all together. The normal form of this is with the . separator. This allows versions such as 1.2-dev2 or 1.2dev2 which normalize to 1.2.dev2.

There's a note in the spec about [compatibility with semver](https://packaging.python.org/en/latest/specifications/version-specifiers/#semantic-versioning).

---

_Label `question` added by @zanieb on 2024-10-02 18:04_

---

_Comment by @cmalek on 2024-10-02 18:06_

Egads thanks for the information!  I didn't know about PEP 440.  Seems better for me to stick to that than semver.

But I also agree with your observations about development releases allowing ., - or _ as separators, so it would be nice if that were supported.

---

_Comment by @zanieb on 2024-10-02 18:08_

We do support it, but the specification says we "MUST normalize" to the canonical format.

---

_Comment by @cmalek on 2024-10-02 18:09_

Thanks!  I understand now.  I'll just adjust my tooling to the normalized version string.

---

_Closed by @zanieb on 2024-10-02 18:10_

---
