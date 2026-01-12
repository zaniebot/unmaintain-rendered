```yaml
number: 4631
title: "uv-resolver: make source=git more structured"
type: pull_request
state: closed
author: BurntSushi
labels: []
assignees: []
base: main
head: ag/rejigger-git-source
created_at: 2024-06-28T17:18:23Z
updated_at: 2024-07-31T15:38:11Z
url: https://github.com/astral-sh/uv/pull/4631
synced_at: 2026-01-12T16:06:21Z
```

# uv-resolver: make source=git more structured

---

_@BurntSushi_

This splits out the git fields into an inline table instead of smushing
them into a single URL.

I'm unsure about this change, because it seems like we are using the git
URLs in other user facing ways. For example, some of the snapshot diffs
look like this:

         Installed 2 packages in [TIME]
          - project==0.1.0 (from file://[TEMP_DIR]/)
          + project==0.1.0 (from file://[TEMP_DIR]/)
    -     + uv-public-pypackage==0.1.0 (from git+https://github.com/astral-test/uv-public-pypackage@0dacfd662c64cb4ceb16e6cf65a157a8b715b979?rev=0.0.1#0dacfd662c64cb4ceb16e6cf65a157a8b715b979)
    +     + uv-public-pypackage==0.1.0 (from git+https://github.com/astral-test/uv-public-pypackage@0dacfd662c64cb4ceb16e6cf65a157a8b715b979)

which seems non-ideal. It should be possible to make the lock file use
structured fields while keeping the URL for terser output as above, but
I'm not sure that it's worth it.

This builds on top of #4627


---

_Review requested from @charliermarsh by @BurntSushi on 2024-06-28 17:19_

---

_Review request for @charliermarsh removed by @BurntSushi on 2024-06-28 17:19_

---

_Review requested from @konstin by @BurntSushi on 2024-06-28 17:19_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-06-28 17:19_

---

_@konstin approved on 2024-07-01 07:59_

The new snapshot looks more correct to me, losing the duplication it had

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-01 16:02_

---

_Comment by @charliermarsh on 2024-07-01 16:33_

I can own this one.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:1431 on 2024-07-01 16:43_

Why was `Rev` removed?

---

_@charliermarsh reviewed on 2024-07-01 16:43_

---

_@BurntSushi reviewed on 2024-07-08 11:56_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1431 on 2024-07-08 11:56_

Because, at least in this change, "rev" is no longer a distinct kind. It is strictly duplicative with the fact that the lock file always records a revision regardless of what the underlying kind is.

---

_@charliermarsh reviewed on 2024-07-08 13:02_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:1431 on 2024-07-08 13:02_

But how does it record the revision that was _requested_ by the user?

---

_@BurntSushi reviewed on 2024-07-08 13:25_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1431 on 2024-07-08 13:25_

Ah I see. I _believe_ that is exactly the thing I am unsure about here. As in, I don't think I grok why we need to keep track of what the user requested specifically in the lock file, since it is presumably in `pyproject.toml`. But yeah, if we do need to keep track of what the user requested in the lock file, specifically for git (but notably not for other things), then we will indeed need to capture some extra state here somehow. Ideally without duplication the revision hash.

---

_@charliermarsh reviewed on 2024-07-08 14:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:1431 on 2024-07-08 14:07_

So if the user has `rev = "refs/foo"` in their requirements, and we lock with that request, and then they re-lock with the same requirements... We want to know that the locked requirement matches `rev = "refs/foo"` _even if_ the commit that maps to that ref has changed (unless the user passed `--upgrade`).

---

_@BurntSushi reviewed on 2024-07-09 13:01_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1431 on 2024-07-09 13:01_

OK, so my mistake here is assuming that "revision" and "precise sha1" were equivalent. They aren't, because a revision might be a named reference to something. I'll add this back.

---

_Comment by @BurntSushi on 2024-07-09 13:34_

@charliermarsh OK, I've updated this PR to include the `revision` in the lock file so that doesn't get dropped.

I think the main issue remaining here is that some of the snapshot diffs for `uv pip compile` output look wrong due to the URLs changing. I think the root cause is that, since the lock file no longer uses a single URL string to represent all of the git information, things external to the lock data structure can't reuse it. So we might need a step that flattens the new structured representation to the URL format for `uv pip compile`?

We could also just keep the URL representation in the lock file and not use structured data. I wouldn't mind that personally.

---

_@charliermarsh reviewed on 2024-07-09 20:54_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1468 on 2024-07-09 20:54_

Maybe we use `commit` instead of `precise` for this, since it's somewhat user-facing?

---

_@charliermarsh reviewed on 2024-07-09 20:55_

---

_Review comment by @charliermarsh on `crates/uv/tests/branching_urls.rs`:606 on 2024-07-09 20:55_

Should we omit the revision if they _are_ the same?

---

_@BurntSushi reviewed on 2024-07-10 14:50_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:1468 on 2024-07-10 14:50_

Yeah I like it. Done.

---

_@BurntSushi reviewed on 2024-07-10 14:53_

---

_Review comment by @BurntSushi on `crates/uv/tests/branching_urls.rs`:606 on 2024-07-10 14:53_

So I am in favor of this, but I think it then becomes ambiguous. Specifically, I'm thinking about this part in the code:

https://github.com/astral-sh/uv/blob/65cbeb9fe670d829ef3fd92a437bd75d4d4968bf/crates/uv-resolver/src/lock.rs#L1372-L1396

Namely, this is where we deserialize our structured git data back into a `Source`. Currently, when `branch`, `tag` and `revision` are all missing, we assume "default branch" semantics. Which means that if we drop `revision = "..."` when it's identical to `commit = "..."`, we won't be able to distinguish between "user specified a specific revision" and "user asked for the default branch."

---

_@charliermarsh approved on 2024-07-31 15:21_

---

_Review comment by @BurntSushi on `crates/uv/tests/edit.rs`:189 on 2024-07-31 15:24_

@charliermarsh Other than preference, I think this was the main issue preventing me from merging this PR. I'm not sure that this is correct.

(I do kinda prefer the URL approach since it's consistent with what we do in other places.)

---

_@BurntSushi reviewed on 2024-07-31 15:24_

---

_@charliermarsh reviewed on 2024-07-31 15:26_

---

_Review comment by @charliermarsh on `crates/uv/tests/edit.rs`:189 on 2024-07-31 15:26_

Ok, let's run with URL. It seems fine to me. Feel free to close.

---

_Closed by @BurntSushi on 2024-07-31 15:38_

---
