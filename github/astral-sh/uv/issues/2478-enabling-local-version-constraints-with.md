---
number: 2478
title: Enabling local version constraints with dependency range
type: issue
state: open
author: brendan-morin
labels: []
assignees: []
created_at: 2024-03-15T17:45:48Z
updated_at: 2025-10-10T20:52:30Z
url: https://github.com/astral-sh/uv/issues/2478
synced_at: 2026-01-07T13:12:17-06:00
---

# Enabling local version constraints with dependency range

---

_Issue opened by @brendan-morin on 2024-03-15 17:45_

# Background

We have some internally patched versions of some OSS packages, republished with [local version identifiers](https://packaging.python.org/en/latest/specifications/version-specifiers/#local-version-identifiers), and an extra index where these are hosted. We would like to specify package requirements (or constraints) so that `uv` only resolves packages with this identifier. For example, if we have an oss package `some_package` published with version `1.0.0`, we might have extra packages published that include `1.0.0.0+internal`, `1.0.0.1+internal`, etc. (so possibly multiple local versions per public version).

## Details

We would like to resolve some requirement such as `some_package>=1.0.0` would be respected (e.g. in the case of a third party library that is unaware of our local versions) but also include the constraint that we only resolve versions with this local version identifier. We can do direct pinning, e.g. `some_package==1.0.0.0+internal`, but ideally we would like a little more flexibility in our specification, for e.g. internal libraries that have other internal patches of OSS software as requirements.

If there is some OSS version that meets the range intent but doesn't specify the local identifier, it should not be resolved as a valid version. For example, adopting a syntax like `some_package>=1.0.0+internal`, could resolve to either `3.2.0.0+internal`, `3.2.0.1+internal`, or `4.0.0.0+internal`, but would not, resolve to something like `1.0.0` or `2.0.0`, or any other version without this particular internal specifier.

As far as I know, this capability does not exist in `pip` at present and would be new functionality. 

## Example Scenario

Let's assume the following scenario, where we have a library `some_package` that is compatible with a `+internal` local version patch only. 

Let's assume the internal index for `some_package` carries the following versions:
```
1.0.0.0+internal
1.0.0.1+internal
2.0.0.0+internal
```
And the open PyPI has the following versions:
```
1.0.0
2.0.0
3.0.0
```
We run the following in pip:
```
pip install 'my_package>=2.0.0+internal' --extra-index-url="https://internal.mycompany.com/patched-index/simple"
```
This could result in the following erroneous resolution to a non-internal version: 
```
Collecting my_package>=2.0.0+internal
  Downloading https://pypi.blah.com/.../my-package-3.0.0.tar.gz 
```
`uv pip install` does I think a slightly better job and at least just fails early:
```
Operator >= is incompatible with versions containing non-empty local segments (`+internal`)
```

## Possible Options

### Option 1: Break existing behavior

The most straightforward solution could be to reinterpret the nature of these requirements specifications.

Given there is an existing (if probably my great) history where local versions are basically treated as just special sub-patch versions (e.g. `1.0.0` < `1.0.0.0+internal` < `1.0.1`) for the purposes of dependency resolution, I don't think reinterpreting behavior of the existing requirements definition syntax of `+local-version` is the way to go here as it would likely have too many unintended consequences for people who may unwittingly rely on this.

### Option 2: Local version constraints files

It would be possible to specify these as command line constraints to the installer, using format similar to constraints files
```
uv pip install -r requirements.txt --local-version-constraints local_constraints.txt
```
where `local_constraints.txt` could look like:
```
my_package+internal
```
While requiring an additional file isn't the most elegant approach, it is nice that this approach would impose no additional constraints on requirements specification syntax, and should be fully backwards compatible with current behavior. The downside with this approach is this relies on the end user to supply these constraints at install time. If there is some e.g. internal package with internal-patched requirements, there still isn't a clear mechanism for a package to specify its own constraints in this way, so every user would have to know to do this.

### Option 3: Update requirements specification syntax

One approach would be to incorporate the ability to interpret additional tokens in the requirements specification for this intent, and could look something like this:

```
uv pip install 'my_package>=2.0.0&internal'
```

This would allow new packages to specify the local version tag as an additional constraint. The coolest part about this approach is that, while this fails in `uv` today, `pip` itself *does not care* about this extra token, and will continue to resolve the package regardless. This means that we could add future-facing functionality without breaking other users ability to specify these constraints. This is also nice compared to Option 2, because it allows packages to specify these internal constraints in their own requirements and `pyproject.toml` files.

## Other Considerations
- It's possible a library could be compatible with only a local version constraint *up to a point*, after which it may work w/ OSS versions (e.g. if the required feature was eventually upstreamed). I'm not sure how the best way to handle this would be, or if this is a case worth considering.

Overall, Option 3 seems like a pretty solid way forward to me, with possible additional integration of Option 2 as well (for when we need to install OSS packages and have them pull in internal dependencies). Would love to hear thoughts on this. Thank you.

---

_Comment by @zanieb on 2024-03-15 19:26_

Lots to consider here, I'll just link some related topics to start:

- https://github.com/astral-sh/uv/issues/1855
- https://github.com/astral-sh/uv/issues/1497
- https://github.com/astral-sh/uv/pull/2430
- https://github.com/astral-sh/uv/pull/2471
- https://github.com/astral-sh/uv/issues/2310
- https://github.com/astral-sh/uv/issues/171

---

_Comment by @econchick on 2024-04-30 19:38_

Heya - I'm just stopping by to say that I ran into this, too. My use case looks like this:

```txt
# requirements.in
--extra-index-url https://download.pytorch.org/whl/cpu

torch~=2.1 ; sys_platform == 'darwin'
torch>=2.1+cpu,<3+cpu ; sys_platform == 'linux'
```

Right now, `pip` and `pip-compile` both do not handle `torch~=2.1+cpu` (considers it an "Invalid requirement"), but they both handle explicit bounds (`torch>=2.1+cpu,<2.2+cpu`). 

In running the above:

```sh
$ uv pip compile -o requirements.txt requirements.in
error: Couldn't parse requirement in `requirements.in` at position 112
  Caused by: Operator >= is incompatible with versions containing non-empty local segments (`+cpu`)
torch>=2.1+cpu,<3+cpu ; sys_platform == 'linux'
     ^^^^^^^^^
```

I checked with just `torch<3+cpu ; sys_platform == 'linux'` and still the same error (thinking maybe multiple bounds didn't work, for some reason). It does work fine with hard pinning though, e.g. `torch==2.1.0+cpu ; sys_platform == 'linux'`.

---

_Referenced in [astral-sh/uv#16216](../../astral-sh/uv/issues/16216.md) on 2025-10-10 18:01_

---
