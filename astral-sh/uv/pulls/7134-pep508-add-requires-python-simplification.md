```yaml
number: 7134
title: "pep508: add requires-python simplification/complexification to `MarkerTree`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/remove-hidden-state
created_at: 2024-09-06T18:29:40Z
updated_at: 2024-09-07T17:46:05Z
url: https://github.com/astral-sh/uv/pull/7134
synced_at: 2026-01-10T12:53:41Z
```

# pep508: add requires-python simplification/complexification to `MarkerTree`

---

_Pull request opened by @BurntSushi on 2024-09-06 18:29_

In #6268, we refactored how we handled `requires-python`. Before
that PR, we simplified markers inside resolution in lock-step with
requires-python version narrowing. But this turns out to be quite hard
to do correctly. After that PR, we now generally only simplify markers
when we write the lock file.

So for example, if a package has this marker:

```
python_full_version >= '3.8' and python_full_version <= '3.10'
```

And we have `requires-python = ">=3.8"`, then its simplified form is this:

```
python_full_version <= '3.10'
```

The problem is that if we use the simplified form in a place where we
shouldn't, perhaps in a place where we don't handle the context of
`requires-python` correctly or if it changes, then the marker can lead
to incorrect results.

So #6268 fixed this, but there was a hiccup: previously, in order to
actually do the simplification, we did this:

1. Asked `MarkerTree` to "restrict" any Python version expressions
such that they were limited to the intersection of what was in the
marker and whas was provided by `requires-python`. 2. When printing
a `MarkerTree`, we specifically set the lower and upper bounds to
negative and positive infinity, respectively.

The end result is that we had a simplified marker, but it relied on a
subtle trick during printing to work. This made it difficult to reason
about markers since they might internally have finite lower/upper
bounds, but when printed, they had non-finite lower/upper bounds.

In order to access the simplified marker, #6268 would run the
"restrict" operation, print the marker as a string and then parse it.
This removed any "hidden" state and made the internal representation of
the marker match its printed form.

But this is bad juju to do, and having the hidden state when we don't
need it any more makes things very difficult to reason about. So in
this PR, we essentially port the old restrict+print approach to one
routine, and then also add a dedicated routine to do the reverse
operation as well (instead of relying on marker conjunction, as we did
before).


---

_Review requested from @konstin by @BurntSushi on 2024-09-06 18:30_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-09-06 18:30_

---

_Review requested from @ibraheemdev by @BurntSushi on 2024-09-06 18:30_

---

_@charliermarsh approved on 2024-09-07 01:00_

This generally makes sense to me, though may want @ibraheemdev to look too.

---

_Comment by @BurntSushi on 2024-09-07 17:45_

I've been chatting loosely with @ibraheemdev about this, so I feel pretty good about this being a generally good approach. (Although I do think we discussed some alternative implementations of `complexify`.) In any case, I'd be happy to address any feedback in a follow-up PR!

---

_Merged by @BurntSushi on 2024-09-07 17:46_

---

_Closed by @BurntSushi on 2024-09-07 17:46_

---

_Branch deleted on 2024-09-07 17:46_

---
