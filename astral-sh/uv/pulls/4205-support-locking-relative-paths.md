```yaml
number: 4205
title: Support locking relative paths
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/relative-paths-in-lockfile
created_at: 2024-06-10T17:04:28Z
updated_at: 2024-06-11T17:02:42Z
url: https://github.com/astral-sh/uv/pull/4205
synced_at: 2026-01-12T16:06:05Z
```

# Support locking relative paths

---

_@konstin_

By splitting `path` into a lockable, relative (or absolute) and an absolute installable path and by splitting between urls and paths by dist type, we can store relative paths in the lockfile.

---

_Label `preview` added by @konstin on 2024-06-10 17:04_

---

_@konstin reviewed on 2024-06-10 17:06_

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:176 on 2024-06-10 17:06_

This is ugly, do we know something better?

---

_@konstin reviewed on 2024-06-10 17:06_

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:247 on 2024-06-10 17:06_

This should not change

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:94 on 2024-06-10 18:26_

This is also a little weird too. I'm not quite sure what to do otherwise here. You could skip serializing empty paths, but then you'd wind up with `sdist = {}`, which is also weird...

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:176 on 2024-06-10 18:26_

Perhaps just `editable`? And adjust the parser to make a trailing `+` as optional (or even an error).

---

_@BurntSushi reviewed on 2024-06-10 18:28_

Nice! This LGTM modulo the quirks you mentioned.

The `{ path = "" }` is unfortunate though. I'm not quite sure what to do about that. Maybe just `sdist = {}` is okay?

---

_@konstin reviewed on 2024-06-11 09:10_

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:176 on 2024-06-11 09:10_

I think my preference is not using `{}+{}` at all (either `editable = "."` or split this `source` into two fields), but for now i've added a `editable+.` as workaround.

---

_@konstin reviewed on 2024-06-11 09:13_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:1089 on 2024-06-11 09:13_

So much boilerplate again just for overriding `Display`/`FromStr`

---

_Marked ready for review by @konstin on 2024-06-11 09:19_

---

_Review requested from @BurntSushi by @konstin on 2024-06-11 09:19_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:785 on 2024-06-11 11:01_

Hmmm I wonder if you could use `PathBuf` directly here and then just use `serialize_with`/`deserialize_with` functions: https://serde.rs/field-attrs.html#deserialize_with. That would let you skip defining a new intermediate `PathWire` type.

(Feel free to get this merged and experiment with it in a follow-up if you want. I'm fine with this as-is.)

---

_@BurntSushi approved on 2024-06-11 11:01_

Nice!

---

_@konstin reviewed on 2024-06-11 11:38_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:785 on 2024-06-11 11:38_

Good idea, this removes a lot of boilerplate

---

_Merged by @konstin on 2024-06-11 11:58_

---

_Closed by @konstin on 2024-06-11 11:58_

---

_Branch deleted on 2024-06-11 11:58_

---

_@charliermarsh reviewed on 2024-06-11 15:44_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:176 on 2024-06-11 15:44_

Is this tracked somewhere?

---

_@konstin reviewed on 2024-06-11 17:02_

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:176 on 2024-06-11 17:02_

Added an item to https://github.com/astral-sh/uv/issues/3611, i'm not yet deep enough here though to be clear on what the solution is.

---
