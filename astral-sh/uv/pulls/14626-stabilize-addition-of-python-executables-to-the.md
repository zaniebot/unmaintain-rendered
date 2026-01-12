```yaml
number: 14626
title: Stabilize addition of Python executables to the bin
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: release/080
head: zb/stable-bin
created_at: 2025-07-15T14:33:40Z
updated_at: 2025-07-17T16:10:06Z
url: https://github.com/astral-sh/uv/pull/14626
synced_at: 2026-01-12T16:11:18Z
```

# Stabilize addition of Python executables to the bin

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/14296

As mentioned in #14681, this does not stabilize the `--default` behavior.

---

_Renamed from "Stabilize adding Python executables to the `bin` on install" to "Stabilize addition of Python executables to the bin" by @zanieb on 2025-07-15 14:35_

---

_Marked ready for review by @zanieb on 2025-07-17 13:56_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4814 on 2025-07-17 14:45_

Should we document that `--default` is still in preview?

---

_Review comment by @geofft on `crates/uv-cli/src/lib.rs`:4813 on 2025-07-17 15:00_

capitalization (or, alternatively, just write `python3.x`)

---

_Review comment by @geofft on `docs/guides/install-python.md`:35 on 2025-07-17 15:05_

It might be worth briefly saying something about why this is not the default, e.g., your OS may run `python3` and expect it to be the version and environment provided by your OS, or you might be using uv alongside other Python providers, and we don't want to conflict, and whether this is a problem depends a lot on your setup.

We might also want to document how to fix this situation if you're in it - is there something other than `rm`?

---

_@geofft reviewed on 2025-07-17 15:05_

---

_@zanieb reviewed on 2025-07-17 15:16_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4813 on 2025-07-17 15:16_

This is intentional to indicate "MAJOR"and "minor", but the point is moot because I've edited this to just use a concrete `python3.13` example and haven't pushed it yet to avoid CI churn.

---

_@zanieb reviewed on 2025-07-17 15:18_

---

_Review comment by @zanieb on `docs/guides/install-python.md`:35 on 2025-07-17 15:18_

Hm, I'm not sure if that belongs in the guide, but maybe it'd be worth explaining in the concept page?

> We might also want to document how to fix this situation if you're in it - is there something other than rm?

You can rm it or uninstall and reinstall without the default flag.

---

_Review comment by @konstin on `crates/uv/tests/it/help.rs`:472 on 2025-07-17 15:39_

inconsistent casing: `pythonX.y`

---

_Review comment by @konstin on `crates/uv/tests/it/help.rs`:473 on 2025-07-17 15:41_

(shared with the other comment) Should we document that --default is still in preview?


---

_Review comment by @konstin on `docs/concepts/python-versions.md`:134 on 2025-07-17 15:49_

```suggestion
$ uv python install 3.12 --default --preview
```

---

_@konstin reviewed on 2025-07-17 15:55_

I think we missed some docs rollback in #14681

---

_@jtfmumm approved on 2025-07-17 15:56_

---

_Review comment by @zanieb on `crates/uv/tests/it/help.rs`:472 on 2025-07-17 15:57_

haha https://github.com/astral-sh/uv/pull/14626#discussion_r2213597998 — I do this often, I didn't know people didn't like it :)

---

_@zanieb reviewed on 2025-07-17 15:57_

---

_@zanieb reviewed on 2025-07-17 15:58_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:134 on 2025-07-17 15:58_

Hm?

This should keep the leading `$` and `--preview` isn't required anymore (previously, it'd error without it). Using `--preview` enables other things like the upgrades.

---

_@zanieb reviewed on 2025-07-17 15:58_

---

_Review comment by @zanieb on `crates/uv/tests/it/help.rs`:473 on 2025-07-17 15:58_

I considered it. I'm not sure if it's important and was leaning towards not.

---

_Review comment by @zanieb on `crates/uv/tests/it/help.rs`:473 on 2025-07-17 15:59_

I'll add it.

---

_@zanieb reviewed on 2025-07-17 15:59_

---

_@konstin reviewed on 2025-07-17 16:03_

---

_Review comment by @konstin on `crates/uv/tests/it/help.rs`:472 on 2025-07-17 16:03_

When I saw those I was wondering about whether there's a semantic difference between the two "template variables", I didn't get the "MAJOR"/"minor" idea

---

_@konstin reviewed on 2025-07-17 16:07_

---

_Review comment by @konstin on `docs/concepts/python-versions.md`:134 on 2025-07-17 16:07_

Didn't mean to remove the `$`. Locally, I still get the warning that `--default` requires preview, my understanding reading #14681 was that that was intentional?

```
$ cargo run -q python install 3.12 --default
warning: The `--default` option is experimental and may change without warning. Pass `--preview` to disable this warning
```

If it was intentional that the warning is shown when using `uv python install 3.12 --default` after #14681 cause we want it to work and be documented without `--preview` enabling other things like the upgrades, than that's fine too.

---

_@konstin approved on 2025-07-17 16:07_

---

_Merged by @zanieb on 2025-07-17 16:09_

---

_Closed by @zanieb on 2025-07-17 16:09_

---

_Branch deleted on 2025-07-17 16:09_

---

_@zanieb reviewed on 2025-07-17 16:10_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:134 on 2025-07-17 16:10_

The warning is intentional yeah. Prior to this pull request, it'd _error_ without `--preview`, which is why I included it in the documentation originally.

---
