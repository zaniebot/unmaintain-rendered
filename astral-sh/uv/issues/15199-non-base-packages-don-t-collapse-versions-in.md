```yaml
number: 15199
title: "Non-base packages don't collapse versions in resolution conflict traces."
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2025-08-10T11:20:21Z
updated_at: 2025-09-22T11:26:09Z
url: https://github.com/astral-sh/uv/issues/15199
synced_at: 2026-01-12T16:02:05Z
```

# Non-base packages don't collapse versions in resolution conflict traces.

---

_@konstin_

**Summary** Error messages for non-base package, i.e. those with a marker, a badly verbose. 

Let's build a simple conflict:

```toml
requires-python = ">=3.9"
dependencies = [
  "anyio>=4.4.0",
  "anyio<=4.3.0",
]
```

We get a 2 line error with a succinct explanation:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because your project depends on anyio>=4.4.0 and anyio<=4.3.0, we can conclude that your project's requirements are unsatisfiable.
```

Let's add a marker: The conflict is still the same.

```
requires-python = ">=3.9"
dependencies = [
  "anyio>=4.4.0",
  "anyio<=4.3.0; sys_platform == 'linux'",
]
```

We get a 42 line error for the same problem:

```
  × No solution found when resolving dependencies for split (markers: sys_platform == 'linux'):
  ╰─▶ Because only the following versions of anyio{sys_platform == 'linux'} are available:
          anyio{sys_platform == 'linux'}==1.0.0
          anyio{sys_platform == 'linux'}==1.1.0
          anyio{sys_platform == 'linux'}==1.2.0
          anyio{sys_platform == 'linux'}==1.2.1
          anyio{sys_platform == 'linux'}==1.2.2
          anyio{sys_platform == 'linux'}==1.2.3
          anyio{sys_platform == 'linux'}==1.3.0
          anyio{sys_platform == 'linux'}==1.3.1
          anyio{sys_platform == 'linux'}==1.4.0
          anyio{sys_platform == 'linux'}==2.0.0
          anyio{sys_platform == 'linux'}==2.0.1
          anyio{sys_platform == 'linux'}==2.0.2
          anyio{sys_platform == 'linux'}==2.1.0
          anyio{sys_platform == 'linux'}==2.2.0
          anyio{sys_platform == 'linux'}==3.0.0
          anyio{sys_platform == 'linux'}==3.0.1
          anyio{sys_platform == 'linux'}==3.1.0
          anyio{sys_platform == 'linux'}==3.2.0
          anyio{sys_platform == 'linux'}==3.2.1
          anyio{sys_platform == 'linux'}==3.3.0
          anyio{sys_platform == 'linux'}==3.3.1
          anyio{sys_platform == 'linux'}==3.3.2
          anyio{sys_platform == 'linux'}==3.3.3
          anyio{sys_platform == 'linux'}==3.3.4
          anyio{sys_platform == 'linux'}==3.4.0
          anyio{sys_platform == 'linux'}==3.5.0
          anyio{sys_platform == 'linux'}==3.6.0
          anyio{sys_platform == 'linux'}==3.6.1
          anyio{sys_platform == 'linux'}==3.6.2
          anyio{sys_platform == 'linux'}==3.7.0
          anyio{sys_platform == 'linux'}==3.7.1
          anyio{sys_platform == 'linux'}==4.0.0
          anyio{sys_platform == 'linux'}==4.1.0
          anyio{sys_platform == 'linux'}==4.2.0
          anyio{sys_platform == 'linux'}>=4.3.0
      and your project depends on anyio>=4.4.0, we can conclude that your project and anyio{sys_platform == 'linux'}<=4.3.0 are incompatible.
      And because your project depends on anyio{sys_platform == 'linux'}<=4.3.0, we can conclude that your project's requirements are
      unsatisfiable.

      hint: Pre-releases are available for `anyio` in the requested range (e.g., 4.0.0rc1), but pre-releases weren't enabled (try:
      `--prerelease=allow`)
```

Let's add 2 markers:

```toml
requires-python = ">=3.9"
dependencies = [
  "anyio<=4.3.0; sys_platform == 'linux'",
  "anyio>=4.4.0; python_version < '3.11'",
]
```

We get a 700 line error with repeated listings of all sorts of anyio versions: https://gist.github.com/konstin/be22add47c5695ce79b56f9a63b85b75

The problem is that a) pubgrub does not realize that `foo{<marker>}` of a range implies `foo` of that range, and that b) we don't simplify `foo{<marker>}` of a range implies `foo` of that range in errors, skipping over enumerating the versions of `foo{marker}` when they are the same as `foo`. It is a also a performance concern, since we're trying all those versions of `foo{<marker>}`, when we could use a single combined incompatibility.

The examples above are MREs, but I'm seeing these long lists with real resolutions too.

---

_Label `error messages` added by @konstin on 2025-08-10 11:20_

---

_Comment by @konstin on 2025-08-10 11:28_

CC @eh2406 You may be interested in this and https://github.com/astral-sh/uv/pull/15200. I'm interested in what you think about this problem (pubgrub can't see past proxy packages to the base packages) and the attempt in the branch.

---

_Closed by @konstin on 2025-09-22 11:26_

---
