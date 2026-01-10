```yaml
number: 12968
title: "Add upload time to `uv.lock`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/upload-time
created_at: 2025-04-18T14:03:52Z
updated_at: 2025-05-06T14:37:23Z
url: https://github.com/astral-sh/uv/pull/12968
synced_at: 2026-01-10T11:10:40Z
```

# Add upload time to `uv.lock`

---

_Pull request opened by @charliermarsh on 2025-04-18 14:03_

## Summary

This is included in PEP 751, so we lose it when converting from `uv.lock`. I think it's a good piece of information to include in the `uv.lock` anyway.


---

_Marked ready for review by @charliermarsh on 2025-04-18 14:09_

---

_Review requested from @BurntSushi by @charliermarsh on 2025-04-18 14:09_

---

_Label `enhancement` added by @charliermarsh on 2025-04-18 14:09_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/mod.rs`:73 on 2025-04-18 16:35_

\o/

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/mod.rs`:73 on 2025-04-18 16:36_

Do we need any process around rolling out a new revision?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/mod.rs`:73 on 2025-04-18 16:36_

Wait it looks like upload time is optional? Do we need to bump the revision number here?

---

_@BurntSushi reviewed on 2025-04-18 16:38_

This LGTM, with one question about bumping the revision number.

Otherwise, I think using RFC 3339 with a Zulu offset is the right thing for this.

---

_@zanieb reviewed on 2025-04-18 16:43_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:73 on 2025-04-18 16:43_

I think the bumped revision means we can tell if the optional field is expected to be available. 

---

_@zanieb reviewed on 2025-04-18 16:43_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:73 on 2025-04-18 16:43_

(REVISION != VERSION)

---

_@BurntSushi reviewed on 2025-04-18 17:02_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/mod.rs`:73 on 2025-04-18 17:02_

Ah okay!

---

_@BurntSushi approved on 2025-04-18 17:02_

---

_Merged by @charliermarsh on 2025-04-21 01:58_

---

_Closed by @charliermarsh on 2025-04-21 01:58_

---

_Branch deleted on 2025-04-21 01:58_

---

_Comment by @adamchainz on 2025-04-28 12:55_

Mentioning so this PR appears in search results for `upload_time` (with underscore): this PR added `upload_time` to `uv.lock` files.

---

_Comment by @charliermarsh on 2025-04-28 13:25_

Ugh, that's my mistake -- this should've been kebab-cased. I will likely change it ASAP to minimize the number of users that have to deal with the churn twice (but retain support for reading `upload_time`).

---

_Comment by @adamchainz on 2025-04-28 13:32_

Oh, I wasn't even commenting on that. I was just noting that searching for `upload_time` in the release notes and issues list found nothing, because the field had never been mentioned.

But well-spotted on the inconsistency and quick fix.

---

_Comment by @charliermarsh on 2025-04-28 13:33_

👍 It turns out we have very few fields with multiple segments but we do use `requires-dist` and `exclude-newer`, etc.

---

_Comment by @LDVSOFT on 2025-04-29 17:34_

Can you hint is there a way to cleanly update `uv.lock` for the new revision without actually updating any versions?

---

_Comment by @konstin on 2025-04-29 19:56_

uv shouldn't update any package versions when migrating to the new lockfile revision, the package versions should only change when the requirements change. Do you see package version updates you didn't expect, and if so, can you share commands to reproduce this?

---

_Comment by @LDVSOFT on 2025-04-29 20:03_

I was doing `uv sync -U` and a lot of the file was different due to revision change, not package version changes.

I've managed to side step it like this:
* Find in a package that was not updated,
* `git restore uv.lock && uv sync`,
* `uv sync --update-package=‹unchanged package›`
* Congratulations, `uv.lock` is rewritten, but not a single version change! Now I can commit this, and then do another proper `uv sync -U` with clearly visible changes.

---

_Comment by @JeanFred on 2025-05-06 11:33_

> I was doing `uv sync -U` and a lot of the file was different due to revision change, not package version changes.
> 
> I've managed to side step it like this:
> 
>     * Find in a package that was not updated,
> 
>     * `git restore uv.lock && uv sync`,
> 
>     * `uv sync -U --update-package=‹unchanged package›`
> 
>     * Congratulations, `uv.lock` is rewritten, but not a single version change! Now I can commit this, and then do another proper `uv sync -U` with clearly visible changes.

Thanks, I had the same issue. I think your third point should be `uv sync --upgrade-package=<unchanged>`

---

_Comment by @LDVSOFT on 2025-05-06 14:37_

@JeanFred Thanks, I was writing from memory.

---
