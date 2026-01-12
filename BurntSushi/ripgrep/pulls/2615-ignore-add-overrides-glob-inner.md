```yaml
number: 2615
title: "`ignore`: Add `overrides::Glob::inner`"
type: pull_request
state: closed
author: 9999years
labels:
  - gitignore
assignees: []
base: master
head: glob-inner
created_at: 2023-09-29T17:43:18Z
updated_at: 2023-09-29T18:36:46Z
url: https://github.com/BurntSushi/ripgrep/pull/2615
synced_at: 2026-01-12T18:23:14Z
```

# `ignore`: Add `overrides::Glob::inner`

---

_@9999years_

This returns the inner glob, if any, and can be used to differentiate between matching against an explicit ignore glob and not matching against any globs.

---

_Renamed from "Add `ignore::overrides::Glob::inner`" to "`ignore`: Add `overrides::Glob::inner`" by @9999years on 2023-09-29 17:43_

---

_Comment by @BurntSushi on 2023-09-29 18:02_

I'm going to pass on this. The override glob is purposely an opaque type and this exposes its implementation to a point that I'm not quite comfortable with. Also, the fact that it returns an `Option` makes this API a bit confusing and also somewhat dependent on implementation details.

---

_Closed by @BurntSushi on 2023-09-29 18:02_

---

_Comment by @9999years on 2023-09-29 18:07_

Why bother exposing it at all, then? The docs say "This is used to report information about the highest precedent glob that matched", but it doesn't report any information outside of the `Debug` implementation.

---

_Comment by @BurntSushi on 2023-09-29 18:10_

> Why bother exposing it at all, then?

I don't know. I designed the API many years ago and I've been unhappy with it ever since. It wouldn't surprise me if I exposed it for an unprincipled reason or if I just copied the design of the `gitignore` sub-module.

I would be open to exposing routines on an override glob that let you query information about it.

---

_Comment by @9999years on 2023-09-29 18:14_

Gotcha. Maybe it's worth reworking the API and pushing a 1.0 release? Let me know if you want to collaborate on it.

---

_Branch deleted on 2023-09-29 18:16_

---

_Comment by @BurntSushi on 2023-09-29 18:25_

Oh it's absolutely worth doing. I plan on it. Problem is, I've been wanting to do it for years. The entire crate needs to be rewritten. It will take me some time to get to it. I can ping you in issues if that's okay when I start thinking about it and write something down. My first step will probably be to collect all of the issues on this tracker about the crate, including open bugs that have their issues in how the crate works.

---

_Comment by @9999years on 2023-09-29 18:34_

Sure, sounds good.

---

_Label `gitignore` added by @BurntSushi on 2023-09-29 18:36_

---
