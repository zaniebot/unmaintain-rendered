```yaml
number: 437
title: Always¹ clear temporary directories
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: clear-source-dists
created_at: 2023-11-16T14:14:43Z
updated_at: 2023-11-16T20:49:49Z
url: https://github.com/astral-sh/uv/pull/437
synced_at: 2026-01-12T16:03:57Z
```

# Always¹ clear temporary directories

---

_@konstin_

Always¹ clear the temporary directories we create.

* Clear source dist downloads: Previously, the temporary directories would remain in the cache dir, now they are cleared properly
* Clear wheel file downloads: Delete the `.whl` file, we only need to cache the unpacked wheel
* Consistent handling of cache arguments: Abstract the handling for CLI cache args away, again making sure we remove the `--no-cache` temp dir.

There are no more `into_path()` calls that persist `TempDir`s that i could find.

¹Assuming drop is run, and deleting the directory doesn't silently error.

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:310 on 2023-11-16 17:41_

Wondering if we should split this into two enums (for Git vs. URL) so that it doesn't need to be optional?

---

_@charliermarsh reviewed on 2023-11-16 17:41_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:278 on 2023-11-16 17:41_

Does this need to be `Option`?

---

_@charliermarsh reviewed on 2023-11-16 17:41_

---

_@charliermarsh reviewed on 2023-11-16 17:41_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:310 on 2023-11-16 17:41_

(Is there any way for `PhantomData` to help with this? I've never actually used it so I don't know what I'm talking about. \cc @BurntSushi)

---

_@charliermarsh reviewed on 2023-11-16 17:42_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/cli.rs`:25 on 2023-11-16 17:42_

I might suggest just making a named struct for this, for clarity? That way it at least encapsulates this detail rather than requiring that callers destructure this tuple and ignore the first field.

---

_@charliermarsh approved on 2023-11-16 17:42_

Small comments but overall makes sense.

---

_@BurntSushi reviewed on 2023-11-16 17:50_

---

_Review comment by @BurntSushi on `crates/puffin-installer/src/downloader.rs`:310 on 2023-11-16 17:50_

I don't think `PhantomData` applies here. `PhantomData` is a way of marking a type or lifetime parameter as "used" by a struct without actually using it. It is most frequently associated (but not universally) with `unsafe` code that is escaping or eliding type/lifetime parameters. It's also used to prescribe semantics of other types onto the containing type. For example, variance. But now we're really digging into rabbit holes here. :-)

As for the enum question, I would probably lean that way but hard to say if that's the right thing to do here. I think for me it would depend on how much consuming code really cares about Git vs. URL. If this is the only distinction then maybe it isn't worth it. (Sorry for my contextless generic opinion.)

---

_@charliermarsh reviewed on 2023-11-16 18:06_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:278 on 2023-11-16 18:06_

I guess it's ok for this to be optional actually.

---

_Merged by @charliermarsh on 2023-11-16 20:49_

---

_Closed by @charliermarsh on 2023-11-16 20:49_

---

_Branch deleted on 2023-11-16 20:49_

---
