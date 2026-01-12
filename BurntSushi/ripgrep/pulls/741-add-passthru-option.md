```yaml
number: 741
title: "Add `--passthru` option"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/passthru
created_at: 2018-01-10T08:40:44Z
updated_at: 2018-01-11T23:45:52Z
url: https://github.com/BurntSushi/ripgrep/pull/741
synced_at: 2026-01-12T18:23:13Z
```

# Add `--passthru` option

---

_@okdana_

Fixes #740 

I ran into one issue that i didn't consider, which is that `--replace` won't work properly with this option (since it treats zero-width matches the same as any others). That's kind of a shame i think (if for no other reason than it would make the unit tests more accurate), but i suppose people can use `sed` if they really need that functionality.

I made it override with `--count` since it seemed relatively safe and intuitive to me. Don't feel strongly about it though.

Also, in hind sight maybe it's not worth mentioning that it's equivalent to adding a `^` pattern, since it's just an implementation detail that isn't really useful to anyone. Let me know if you agree and i'll remove it.

Some of the changes here might conflict with #728, but only slightly if so.

---

_Review comment by @BurntSushi on `src/app.rs`:171 on 2018-01-11 12:41_

Hmm I kind of feel like `--passthru` should conflict with `--count`, no? They are somewhat mutually incompatible flags. I could be convinced though! Maybe explain this a bit more?

I agree with the other conflicts. Thanks for thinking about that!

---

_Review comment by @BurntSushi on `doc/rg.1.md`:284 on 2018-01-11 12:42_

Yeah I agree that we shouldn't mention `^`. I could see the implementation changing for example to support `--replace` by implementing this flag differently. (e.g., By not modifying the regex.)

---

_@BurntSushi reviewed on 2018-01-11 12:44_

Looks great overall! Just have some questions (that you anticipated). Thanks for thinking through the corner cases. :)

---

_@okdana reviewed on 2018-01-11 16:07_

---

_Review comment by @okdana on `src/app.rs`:171 on 2018-01-11 16:07_

I guess i feel like option conflict errors are pretty heavy-handed and disruptive, and they should only be used if it's likely that the user would be confused by the behaviour otherwise;  if not, they should just override each other or be ignored.

In this case, i don't think there's any reason to expect `rg --passthru -c` to do anything but print the count. And that's consistent with stuff like `rg --line-number -c`, which feels similarly unambiguous to me.

(There is a difference here in that `rg -c --passthru` actually disables `-c`, whereas `rg -c --line-number` just ignores the `--line-number`. If we do keep the non-conflicting behaviour, i think i should actually change it so that it just checks `!self.is_present("count")` when it adds the pattern in `args.rs`.)

That's my inclination, anyway — like i said, i don't feel super strongly about it. If you aren't feeling it i'll change it obv

---

_@BurntSushi reviewed on 2018-01-11 16:54_

---

_Review comment by @BurntSushi on `src/app.rs`:171 on 2018-01-11 16:54_

Hmmmm... I am conflicted. But you're right, we already have this problem today with other flags. I'm fine with the behavior in this PR, and adding `!self.is_present("count")` sounds good.

---

_Comment by @okdana on 2018-01-11 17:28_

Updated

---

_Merged by @BurntSushi on 2018-01-11 23:45_

---

_Closed by @BurntSushi on 2018-01-11 23:45_

---
