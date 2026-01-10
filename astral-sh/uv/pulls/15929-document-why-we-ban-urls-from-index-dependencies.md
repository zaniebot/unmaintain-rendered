```yaml
number: 15929
title: Document why we ban URLs from index dependencies
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/url-url-url-docs
created_at: 2025-09-18T09:55:35Z
updated_at: 2025-09-25T10:18:12Z
url: https://github.com/astral-sh/uv/pull/15929
synced_at: 2026-01-10T06:36:15Z
```

# Document why we ban URLs from index dependencies

---

_Pull request opened by @konstin on 2025-09-18 09:55_

Document why we ban URLs from index dependencies, and also what the lookahead resolver does.

---

_Label `documentation` added by @konstin on 2025-09-18 09:55_

---

_@zsol reviewed on 2025-09-20 12:31_

---

_Review comment by @zsol on `docs/reference/internals/resolver.md`:184 on 2025-09-20 12:31_

it took me several takes to parse this bit, it could use a bit of restructuring. At the very least:
```suggestion
plain package name, or a URL dependency: everything with a `{name} @ {url}` or a `git`,` url`,
`path`, or `workspace` source, or put differently, all Git, remote file, local file or local path
dependencies.
```

---

_@zsol reviewed on 2025-09-20 12:34_

---

_Review comment by @zsol on `docs/reference/internals/resolver.md`:192 on 2025-09-20 12:34_

nit: might want to turn "override", "constraint", and maybe "workspace member" into a link that explains how to do these

---

_@zsol reviewed on 2025-09-20 12:34_

---

_Review comment by @zsol on `docs/reference/internals/resolver.md`:198 on 2025-09-20 12:34_

```suggestion
auditing which URLs can be accessed. For example, when only using one index URL and no URL
```

---

_Review comment by @zsol on `docs/reference/internals/resolver.md`:205 on 2025-09-20 12:36_

```suggestion
error: foo cannot be fulfilled, there is a resolver error. If URLs on index packages were allowed,
```

---

_@zsol reviewed on 2025-09-20 12:36_

---

_@zsol reviewed on 2025-09-20 12:42_

---

_Review comment by @zsol on `docs/reference/internals/resolver.md`:209 on 2025-09-20 12:42_

In other words: this is a limitation of uv's resolver implementation. In order to keep resolution time (and memory usage I assume) reasonably low, it assumes that the version universe is fixed during resolution, and no new versions can be discovered _during_ the process.

---

_@zsol approved on 2025-09-20 12:43_

I think the first and last paragraphs could use a bit of refactoring, but otherwise lgtm

<a href="https://gitme.me/image?url=https%3A%2F%2Fmedia3.giphy.com%2Fmedia%2FeEXxfHQJ0dWOrctI55%2Fgiphy.gif&token=nice" data-gitmeme-token="nice"><img src="https://media3.giphy.com/media/eEXxfHQJ0dWOrctI55/giphy.gif" title="Created by gitme.me with /nice"
        alt="nice"/></a>


---

_@konstin reviewed on 2025-09-23 14:53_

---

_Review comment by @konstin on `docs/reference/internals/resolver.md`:209 on 2025-09-23 14:53_

We could theoretically make it possible by fetching everything ahead of time and checking, but having to check every version reachable ruins performance. iirc no package manager supports this, it's not really possible to write a performant tool if you can't be incremental, just from the cost of looking at all packages.

---

_Review comment by @konstin on `docs/reference/internals/resolver.md`:209 on 2025-09-23 15:15_

It's hard to explain it with an example here to avoid having to explain the concept of resolution in the SAT sense and how you can't derive that a package-version in incompatible from a range of its dependency being exhaustively incompatible and how that's required to make both fast conclusions and to conclude failure for incompatible dependencies.

---

_@konstin reviewed on 2025-09-23 15:16_

---

_Merged by @konstin on 2025-09-25 10:18_

---

_Closed by @konstin on 2025-09-25 10:18_

---

_Branch deleted on 2025-09-25 10:18_

---
