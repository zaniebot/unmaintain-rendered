```yaml
number: 12447
title: Override build dependency and backend metadata, similiar to dependency-metadata
type: issue
state: closed
author: notatallshaw
labels:
  - enhancement
assignees: []
created_at: 2025-03-24T19:38:54Z
updated_at: 2025-08-21T14:48:15Z
url: https://github.com/astral-sh/uv/issues/12447
synced_at: 2026-01-10T03:32:45Z
```

# Override build dependency and backend metadata, similiar to dependency-metadata

---

_Issue opened by @notatallshaw on 2025-03-24 19:38_

### Summary

Currently it's possible to override a packages [dependency metadata](https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata), e.g.

```toml
[[tool.uv.dependency-metadata]]
name = "chumpy"
version = "0.70"
requires-dist = ["numpy>=1.8.1", "scipy>=0.13.0", "six>=1.11.0"]
```

It would be nice to do that for build dependency and backend metadata, there are two scenarios I can see this being useful in:

1. A new version of a build backend gets released with backwards incompatibilities, only a small number of unmaintained dependencies need fixing to pin to an older version of the build backend
2. A user dependency on a legacy setuptools project and it would work except for the fact the project is unmaintained and does not provide a pyproject.toml

I'm a little unsure on my second example whether in general it would be enough to specify a build backend, or if in general it would need something more. In general I may have not fully thought through the complexities, so I understand if this isn't possible. This is something I think would be helpful to the ecosystem, not something I personally need.


### Example

```toml
[[tool.uv.dependency-metadata]]
name = "foo"
version = "0.8.0"
requires-dist = ["numpy>=1.8.1", "scipy>=0.13.0", "six>=1.11.0"]
build-requires-dist = ["setuptools<78"]
build-backend = ["setuptools.build_meta"]
```



---

_Label `enhancement` added by @notatallshaw on 2025-03-24 19:38_

---

_Comment by @jamie-chang-globality on 2025-03-25 10:17_

We had the same problem with setuptools yesterday. For reference this is https://github.com/pypa/setuptools/issues/4910. It broke a lot of builds across the whole ecosystem.

So definitely +1 from me 



---

_Comment by @notatallshaw on 2025-03-25 13:44_

> For reference this is [pypa/setuptools#4910](https://github.com/pypa/setuptools/issues/4910). It broke a lot of builds across the whole ecosystem.

FWIW, I do not see this proposal as a solution to that particular situation, this is a per package solution, whereas that affected many packages, so I'm not sure this would scale well.

I think uv consistently supporting `--build-constraint` across all its commands is a better solution to that particular situation.




---

_Comment by @zanieb on 2025-04-01 19:07_

We're working on support for this, with another motivating example being the _extension_ of build requirements for specific packages.

---

_Assigned to @Gankra by @zanieb on 2025-04-01 19:07_

---

_Comment by @notatallshaw on 2025-04-05 19:02_

Also, [the spec](https://peps.python.org/pep-0517/#recommendations-for-build-frontends-non-normative) actually says that build frontends should provide some tooling for users like this:

> build frontends SHOULD provide some mechanism for users to override the above defaults. For example, a build frontend could have a --build-with-system-site-packages option that causes the --system-site-packages option to be passed to virtualenv-or-equivalent when creating build environments, or a --build-requirements-override=my-requirements.txt option that overrides the project’s normal build-requirements.
>
> The general principle here is that we want to enforce hygiene on package authors, while still allowing end-users to open up the hood and apply duct tape when necessary.

---

_Comment by @zsol on 2025-08-21 14:20_

I believe #14735 addresses this entirely. Feel free to reopen if I missed something

---

_Closed by @zsol on 2025-08-21 14:20_

---

_Comment by @notatallshaw on 2025-08-21 14:48_

I'm happy with this being closed, but on consideration this does not completely cover this, you may want to override a pinning or an upper bound, e.g. in the future you may want to override `setuptools==70` to `setuptools==100`, I don't believe that's possible with this new feature.

I don't have a specific use case for this right now though, so maybe re-raise this when someone actually needs it.

---
