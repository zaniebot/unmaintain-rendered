```yaml
number: 11513
title: fix duplicate packages with multiple conflicting extras declared
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: ag/fix-11479
created_at: 2025-02-14T15:48:27Z
updated_at: 2025-02-18T12:45:26Z
url: https://github.com/astral-sh/uv/pull/11513
synced_at: 2026-01-10T11:10:38Z
```

# fix duplicate packages with multiple conflicting extras declared

---

_Pull request opened by @BurntSushi on 2025-02-14 15:48_

This implements a somewhat of a cop-out fix for #11479, where the lock
file produced was missing some conflict markers. This in turn could lead
to multiple versions of the same package being installed into the same
environment.

(What follows is one of the commit messages that gets into the weeds
about the specific problem here.)

The particular example I honed in on here was the `e3nn -> sympy 1.13.1`
and `e3nn -> sympy 1.13.3` dependency edges. In particular, while the
former correctly has a conflict marker, the latter's conflict marker was
getting simplified to `true`. This makes the edges trivially
overlapping, and results in both of them getting installed
simultaneously. (A similar problem happens for the `e3nn -> torch`
dependency edges.)

Why does this happen? Well, conflict marker simplification works by
detecting which extras are known to be enabled (and disabled) for each
node in the graph. This ends up being expressed as a set of sets, where
each inner set contains items corresponding to "extras is included" or
"extra is excluded."

The logic then is if _all_ of these sets are satisfied by the conflict
marker on the dependency edge, then this conflict marker can be
simplified by assuming all of the inclusions/exclusions to be true.

In this particular case, we run into an issue where the set of
assumptions discovered for `e3nn` is:

    {test[sevennet]}, {}, {~test[m3gnet], ~test[alignn], test[all]}

And the corresponding conflict marker for `e3nn -> sympy 1.13.1` is:

    extra == 'extra-4-test-all'
    or extra == 'extra-4-test-chgnet'
    or (extra != 'extra-4-test-alignn' and extra != 'extra-4-test-m3gnet')

And the conflict marker for `e3nn -> sympy 1.13.3` is:

    extra == 'extra-4-test-alignn' or extra == 'extra-4-test-m3gnet'

Evaluating each of the sets above for `sympy 1.13.1`'s conflict
marker results in them all being true. Simplifying in turn results in
the marker being true. For `sympy 1.13.3`, not all of the sets are
satisfied, so this marker is not simplified.

I think the fundamental problem here is that our inferences aren't quite
rich enough to make these logical leaps. In particular, the conflict
marker for `e3nn -> sympy 1.13.3` is not satisfied by _any_ of our sets.
One might therefore conclude that this dependency edge is impossible.
But! The `test[sevennet]` set doesn't actually rule out `test[m3gnet]`
from being included, for example, because there is no conflict. So it is
actually possible for this marker to evaluate to true.

And I think this reveals the problem: for the `e3nn -> sympy 1.13.1`
conflict marker, the inferences don't capture the fact that
`test[sevennet]` _might_ have `test[m3gnet]` enabled, and that would in
turn result in the conflict marker evaluating to `false`. This directly
implies that our simplification here is inappropriate.

It would be nice to revisit how we build our inferences here so that
they are richer and enable us to make correct logical leaps. For now, we
fix this particular bug with a bit of a cop-out: we skip conflict marker
simplification when there are ambiguous dependency edges.

Fixes #11479


---

_Label `bug` added by @BurntSushi on 2025-02-14 15:48_

---

_Label `lock` added by @BurntSushi on 2025-02-14 15:48_

---

_Review requested from @charliermarsh by @BurntSushi on 2025-02-14 17:18_

---

_@charliermarsh reviewed on 2025-02-14 21:43_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/output.rs`:97 on 2025-02-14 21:43_

Necessary?

---

_Comment by @charliermarsh on 2025-02-14 21:47_

I'm sort of just confirming my understanding, but if we simplified the conflict marker for each set independently, and then checked if each simplification resulted in the same outcome, could we use _that_?

Or, what if for each set `A` and `B`, we did:

```
A and (extra == 'extra-4-test-all'
or extra == 'extra-4-test-chgnet'
or (extra != 'extra-4-test-alignn' and extra != 'extra-4-test-m3gnet'))
or B and (extra == 'extra-4-test-all'
or extra == 'extra-4-test-chgnet'
or (extra != 'extra-4-test-alignn' and extra != 'extra-4-test-m3gnet'))
```

Would that be sound?


---

_@charliermarsh reviewed on 2025-02-16 17:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:23933 on 2025-02-16 17:09_

Is it not still possible for us to simplify out terms like `platform_machine != 'aarch64' and extra == 'extra-7-project-cpu' and extra == 'extra-7-project-cu124'`?

---

_@BurntSushi reviewed on 2025-02-16 23:17_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:23933 on 2025-02-16 23:17_

Yes I think it's possible. Even before this change, conflict marker simplification does not produce the minimal possible markers in all cases. I think it's just a question of how much time you want me to allocate to the problem. :-)

---

_Comment by @BurntSushi on 2025-02-16 23:20_

> I'm sort of just confirming my understanding, but if we simplified the conflict marker for each set independently, and then checked if each simplification resulted in the same outcome, could we use that?

If I'm understanding you correctly, I believe that's what the code was already doing (and is still doing for cases where there is no definitive ambiguity). Namely, simplification _only_ happens when evaluating each set results in `true`. But that still isn't enough here.

I think the problem is that the assumptions inferred from the dependency graph aren't rich enough. They aren't expressing the full set of possibilities.

---

_@BurntSushi reviewed on 2025-02-16 23:40_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/output.rs`:97 on 2025-02-16 23:40_

Oops. Nope. Fixed.

---

_@charliermarsh approved on 2025-02-18 01:25_

---

_Comment by @charliermarsh on 2025-02-18 01:27_

I'm a little worried about how this will affect #11548 and #11559.

---

_Comment by @BurntSushi on 2025-02-18 12:45_

> I'm a little worried about how this will affect #11548 and #11559.

Yeah hmmm. I think the upside is that this should only apply to cases where there are ambiguous dependency edges (i.e., two different edges with the same package name but different versions). So the lack of simplification I think should be somewhat limited?

---

_Merged by @BurntSushi on 2025-02-18 12:45_

---

_Closed by @BurntSushi on 2025-02-18 12:45_

---

_Branch deleted on 2025-02-18 12:45_

---
