---
number: 191
title: "Don't select a prerelease if a stable version is available"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-10-26T08:37:51Z
updated_at: 2023-10-29T18:31:56Z
url: https://github.com/astral-sh/uv/issues/191
synced_at: 2026-01-10T01:23:03Z
---

# Don't select a prerelease if a stable version is available

---

_Issue opened by @konstin on 2023-10-26 08:37_

We currently select prereleases when we shouldn't, e.g. `defusedxml==0.8.0rc2` over `defusedxml==0.7.1` for `python3-openid`.

We need to allow prereleases when there are no stable releases (this used to be the case with black) or if explicitly requested. I think requiring that all requirements for a package must have a version as prerelease to opt-in to prereleases makes most sense, but that might be hard to in pubgrub. E.g. `a>=2.0a1,!=3.0` and `a>=2.0b1` would select `2.0` (and only that version) in prelease mode, while `a>=2.0a1` and `a>=1.0` wouldn't. `a>=2.0a1` and `a>=1.0a1` is an edge case, but arguably it shouldn't either.

https://github.com/konstin/monotrail-resolve/blob/4256ce0a27f964dbc4ab6f255cdc352a8b42b1b0/resolve_prototype/resolve.py#L379-L409

---

_Label `bug` added by @charliermarsh on 2023-10-26 13:29_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-26 13:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-27 00:38_

---

_Comment by @charliermarsh on 2023-10-27 00:40_

I can add this to the candidate selection struct.

---

_Comment by @charliermarsh on 2023-10-27 00:58_

Okay, I now understand why this is hard. And reading this Posy comment confirmed what I'm running into:

```
### Prereleases in specifiers

According to PEP 440, specifiers like `>= 2.0a1` are supposed to
change meaning depending on whether or not the literal version
contains a prerelease marker. So like, `>= 2.0` *doesn't* match
`2.1a1`, because that's a prerelease, and regular specifiers never
match prereleases. But `>= 2.0a1` *does* match `2.1a1`, because the
presence of a prerelease in the specifier makes it legal for
prerelease versions to match.
  
I don't think I can actually implement this using the `pubgrub`
system, since it collapses multiple specifiers for the same package
into a single set of valid ranges, and there's no way to preserve the
information about which ranges were derived from specifiers that
included prerelease suffixes, and which ranges weren't.
  
And if you think about it... that's actually because while this rule is
well-defined for a specifier in isolation, it doesn't really make sense when
you're talking about multiple packages with their own dependencies. E.g., if
package A depends on `foo == 2.0a1`, and package B depends on `foo >= 1.0`, then
is it valid to install foo v2.0a1? It feels like it ought to match all the
requirements, but technically it doesn't... according to a strict reading of PEP
440, once any package says `foo >= 1.0`, it becomes impossible to ever use a
`foo` pre-release anywhere in the dependency tree, no matter what other packages
say. Pre-release validity is just inherently a global property, not a property
of individual specifiers.
  
So I'm thinking we should use the rule:

- If all available versions are pre-releases, then pre-releases are valid
- If we're updating a set of pins that already contain a pre-release,
  then pre-releases are valid (or at least that specific pre-release
  is)
- Otherwise, to get pre-releases, you have to set some
  environment-level config like `allow-prerelease = ["foo"]`.
```

---

_Comment by @charliermarsh on 2023-10-27 00:59_

For now, I think we should just do: if all available versions are pre-releases, allow them; otherwise, require a global flag.

---

_Comment by @charliermarsh on 2023-10-27 01:00_

(Poetry requires you to mark specific packages as allowing pre-releases, which is much easier.)

---

_Comment by @charliermarsh on 2023-10-27 01:02_

We can also do the thing described above where we allow a pre-release if it's already present in a spec.

---

_Comment by @charliermarsh on 2023-10-27 01:03_

Actually, we could also support `foo == 2.0.a1` if we want. Like, if a user explicitly asks for a pre-release, we _could_ support that.

---

_Comment by @charliermarsh on 2023-10-27 01:06_

Okay, I'm going to do this:

- If all available versions for a package are pre-releases, then accept pre-releases.
- If a pre-release was already chosen in the lockfile, allow that pre-release.
- If a package was specified as a _direct dependency_ with a pre-release marker in its version specifier, allow pre-releases for that package. (I don't think we can really support this for transitive dependencies.)

---

_Referenced in [astral-sh/uv#216](../../astral-sh/uv/pulls/216.md) on 2023-10-29 18:26_

---

_Closed by @charliermarsh on 2023-10-29 18:31_

---
