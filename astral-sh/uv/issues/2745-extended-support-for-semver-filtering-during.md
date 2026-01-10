```yaml
number: 2745
title: Extended support for semver filtering during version resolution
type: issue
state: open
author: samypr100
labels:
  - enhancement
assignees: []
created_at: 2024-04-01T01:12:55Z
updated_at: 2024-11-14T16:32:35Z
url: https://github.com/astral-sh/uv/issues/2745
synced_at: 2026-01-10T04:36:19Z
```

# Extended support for semver filtering during version resolution

---

_Issue opened by @samypr100 on 2024-04-01 01:12_

This is a follow up to https://github.com/astral-sh/uv/pull/2582#issuecomment-2016248743 per @zanieb's suggestion.

In a nutshell, I think it would be great to see an additional resolution strategy that supports semver-like filtering. This can potentially be introduced by a new flag (e.g. `--target` or `--resolution-target`) where possible values are `major`, `minor`, `patch`. This could be something that commands could benefit from in the future such as `uv pip list --outdated` as suggested by #2150 or equivalent.

#### Rationale

I often find myself wondering what are the "patch", "minor", or "major" updates available for packages that follow semver-like conventions. In many cases, I might want to update to the latest minor even if there's a major available. In the same vein, I also find myself often wanting to know this answer to direct dependencies over transitive, so direct resolution support plays a major role in such situations. `pip` doesn't quite support this, but I do think this is something `uv pip` is in a unique position support. In addition, I think this will benefit `uv` in the future if its aiming to support poetry-like dependency resolution since the underlying semantics are likely to be similar when that time comes.

### Scenarios

I've tried to compile some scenarios below to help, but the list is not comprehensive.

Let's consider we have installed this `requirements.txt` from a `requirements.in` that only had `django>=4` at a given point in time (some dependency versions below are hypothetical to help with the scenarios).

```
asgiref==3.7.2
django==4.2.10
sqlparse==0.4.3
tzdata==2023.1
```

Below are some examples for each scenario `major`, `minor`, `patch`. These imply a hypothetical `uv pip list --outdated` as a prefix, but it can apply to other commands. These also assumes you have a direct `django>=4` constraint and already installed the dependencies defined above.

#### Major Target (implies poetry *)

<details>
  <summary>Click to expand</summary>

With `--target=major --resolution=highest`. This is the same as the current `--resolution=highest`. It is assumed `--resolution=highest` is the default when resolution is not specified.

```
asgiref:  3.7.2  → 3.8.1   # upgrade
django:   4.2.10 → 5.0.3   # upgrade
sqlparse: 0.4.3  → 0.4.4   # upgrade
tzdata:   2023.1 → 2024.1  # upgrade
```


With `--target=major --resolution=highest-direct`. Assumes direct is obtained from the direct constraints, otherwise this can behave as `--resolution=highest`.

```
asgiref:  3.7.2  → 3.7.2   # same
django:   4.2.10 → 5.0.3   # upgrade
sqlparse: 0.4.3  → 0.4.3   # same
tzdata:   2023.1 → 2023.1  # same
```


With `--target=major --resolution=lowest`. This is the same as the current `--resolution=lowest`.

```
asgiref:  3.7.2  → 3.4.1   # downgrade; django constraint in 4.0
django:   4.2.10 → 4.0     # downgrade
sqlparse: 0.4.3  → 0.2.2   # downgrade; django constraint in 4.0
tzdata:   2023.1 → 2020.1  # downgrade
```


With `--target=major --resolution=lowest-direct`. This is not quite the same as `--resolution=lowest-direct` given we have a direct constraint and a baseline to compare against.

```
asgiref:  3.7.2  → 3.7.2   # same; no django constraint on this version
django:   4.2.10 → 4.0     # downgrade
sqlparse: 0.4.3  → 0.4.3   # same; no django constraint on this version
tzdata:   2023.1 → 2023.1  # same; no django constraint on this version
```

</details>

#### Minor Target (implies poetry ^)

<details>
  <summary>Click to expand</summary>

With `--target=minor --resolution=highest`

```
asgiref:  3.7.2  → 3.8.1   # upgrade minor only
django:   4.2.10 → 4.2.11  # upgrade minor only
sqlparse: 0.4.3  → 0.4.4   # upgrade minor only
tzdata:   2023.1 → 2023.4  # upgrade minor only
```


With `--target=minor --resolution=highest-direct`

```
asgiref:  3.7.2  → 3.7.2   # same
django:   4.2.10 → 4.2.11  # upgrade minor only
sqlparse: 0.4.3  → 0.4.3   # same
tzdata:   2023.1 → 2023.1  # same
```


With `--target=minor --resolution=lowest`

```
asgiref:  3.7.2  → 3.4.1   # downgrade minor only; django constraint in 4.0
django:   4.2.10 → 4.0     # downgrade minor only
sqlparse: 0.4.3  → 0.2.2   # downgrade minor only; django constraint in 4.0
tzdata:   2023.1 → 2023.1  # same
```


With `--target=minor --resolution=lowest-direct`

```
asgiref:  3.7.2  → 3.7.2   # same; no django constraint
django:   4.2.10 → 4.0     # downgrade minor only
sqlparse: 0.4.3  → 0.4.3   # same; no django constraint
tzdata:   2023.1 → 2023.1  # same; no django constraint
```

</details>

#### Patch Target (implies poetry ~)

<details>
  <summary>Click to expand</summary>

With `--target=patch --resolution=highest`

```
asgiref:  3.7.2  → 3.7.2   # same
django:   4.2.10 → 4.2.11  # upgrade patch only
sqlparse: 0.4.3  → 0.4.4   # upgrade patch only
tzdata:   2023.1 → 2023.1  # same
```


With `--target=patch --resolution=highest-direct`

```
asgiref:  3.7.2  → 3.7.2   # same
django:   4.2.10 → 4.2.11  # upgrade patch only
sqlparse: 0.4.3  → 0.4.3   # same
tzdata:   2023.1 → 2023.1  # same
```


With `--target=patch --resolution=lowest`

```
asgiref:  3.7.2  → 3.7.0   # downgrade patch only; no django constraint on this version
django:   4.2.10 → 4.2     # downgrade patch only
sqlparse: 0.4.3  → 0.4.0   # downgrade patch only; no django constraint on this version
tzdata:   2023.1 → 2023.1  # same
```


With `--target=patch --resolution=lowest-direct`

```
asgiref:  3.7.2  → 3.7.2   # same
django:   4.2.10 → 4.2     # downgrade patch only
sqlparse: 0.4.3  → 0.4.3   # same
tzdata:   2023.1 → 2023.1  # same
```

</details>

---

Given above scenarios, `--target` probably makes most sense when have a baseline to compare against. For example, already installed dependencies. As such on scenarios where there is absolutely nothing to compare against, target should likely fallback to the default behavior of the `--resolution` mode semantics instead. Poetry can achieve this due to `*`, `^`, `~` syntax in the `pyproject.toml` dependency list, and can compare these against the installed versions and/or the lock file.
In addition, dependency constraints should likely always take priority over output filtering with `--target`. For example, if I didn't have the `django>=4` direct constraint, we would've gotten `django==1.1.3` on `--target=major --resolution=lowest`.

### Other tooling

* poetry: Built-in support by using their [dependency-specification](https://python-poetry.org/docs/dependency-specification/) via poetry update.
* pip: You'd use something like [pip-check-updates](https://github.com/zehengl/pip-check-updates) to approximate this behavior, but no built-in support exists.
* npm: Built-in support. npm update will often try stay within your dependency constraints (unless you have overrides). If you want a friendly table output, typically you'd use [npm-check-updates](https://www.npmjs.com/package/npm-check-updates).
* composer: Supports `--major-only`, `--minor-only`, `--patch-only` built-in for outdated checks and semver version constraints (`*`, `^`, `~`) for updates.

---

_Label `enhancement` added by @zanieb on 2024-04-01 14:36_

---

_Comment by @matterhorn103 on 2024-11-14 16:32_

Now that we have the `--upgrade` and `--upgrade-package` options for `uv sync`, the ability to control how far things are upgraded would be really great!

(/I almost thought that the default might be cargo-like behaviour, where a limited upgrade ceiling is the default. Though maybe that makes less in Python where SemVer is not as ubiquitous.)

---
