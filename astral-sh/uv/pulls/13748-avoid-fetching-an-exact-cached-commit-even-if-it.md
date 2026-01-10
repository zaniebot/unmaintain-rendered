```yaml
number: 13748
title: "avoid fetching an exact, cached commit, even if it isn't locked"
type: pull_request
state: merged
author: oconnor663
labels:
  - enhancement
assignees: []
merged: true
base: main
head: jack/git_exact_commit
created_at: 2025-05-31T03:33:25Z
updated_at: 2025-06-10T09:22:59Z
url: https://github.com/astral-sh/uv/pull/13748
synced_at: 2026-01-10T11:10:42Z
```

# avoid fetching an exact, cached commit, even if it isn't locked

---

_Pull request opened by @oconnor663 on 2025-05-31 03:33_

This is ~a rough draft of~ one approach we might take to fixing https://github.com/astral-sh/uv/issues/13513

Here's a complete repro of the bug (arguably) in that ticket:

```
$ cd `mktemp -d`
$ cat << EOF > requirements.txt                                                                           
feedparser @ git+https://github.com/kurtmckee/feedparser@6c2a412a1569303c6c02d82ef38c08e19f81315e
EOF
$ uv venv
...
$ uv pip install -r requirements.txt 2>&1 | cat
   Updating https://github.com/kurtmckee/feedparser (6c2a412a1569303c6c02d82ef38c08e19f81315e)
    Updated https://github.com/kurtmckee/feedparser (6c2a412a1569303c6c02d82ef38c08e19f81315e)
Resolved 7 packages in 12ms
Installed 7 packages in 7ms
...
$ uv pip install -r requirements.txt 2>&1 | cat
   Updating https://github.com/kurtmckee/feedparser (6c2a412a1569303c6c02d82ef38c08e19f81315e)
    Updated https://github.com/kurtmckee/feedparser (6c2a412a1569303c6c02d82ef38c08e19f81315e)
Resolved 7 packages in 18ms
Audited 7 packages in 0.02ms
```

The problem is that we keep fetching the repo every time, even though this commit is definitely in cache. With the changes in this PR, the extra fetch is gone:

```
$ uv pip install -r requirements.txt 2>&1 | cat
Resolved 7 packages in 15ms
Audited 7 packages in 0.07ms
```

This ~draft~ PR notices that the ref we're trying to fetch looks exactly like a commit, and it checks (`rev-parse`) whether that commit is already present in the cached repo and interprets it as a locked (`precise`) ref if so. ~The implementation is kind of sloppy as is, and it should probably be factored into smaller functions out and also tested,~ but I need feedback on the overall approach.

Another approach we could take here could be to change our interpretation of these looks-like-a-commit refs such that we _always_ assume they're commits at parse time. (In other words, mark the with `precise` OIDs as soon as we parse them.) If we did that, we'd probably want to add checks to make sure that we catch it if that assumption is ever violated. It looks like we used to behave more like this, but then we [replaced that with the current behavior](https://github.com/astral-sh/uv/pull/10803), and if we want to go this way I'll need to make sure I understand exactly why we did that.

@ibraheemdev would you have time to take a look at this? Also cc @charliermarsh and @zanieb.

---

_Review requested from @ibraheemdev by @oconnor663 on 2025-05-31 03:33_

---

_Label `bug` added by @oconnor663 on 2025-05-31 03:33_

---

_Comment by @codspeed-hq[bot] on 2025-05-31 03:49_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/jack%2Fgit_exact_commit)

### Merging #13748 will **improve performances by 16.67%**

<sub>Comparing <code>jack/git_exact_commit</code> (41a6e9f) with <code>main</code> (f5382c0)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 112 ns | 96 ns | +16.67% |


---

_Assigned to @zanieb by @zanieb on 2025-06-02 15:57_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/source.rs`:113 on 2025-06-02 16:34_

I don't think `contains` is enough because `rev-parse` will peel references by default. You can use `git rev-parse --symbolic-full-name` to check if the reference is a direct commit or not.

---

_@ibraheemdev reviewed on 2025-06-02 16:34_

---

_@oconnor663 reviewed on 2025-06-02 16:50_

---

_Review comment by @oconnor663 on `crates/uv-git/src/source.rs`:113 on 2025-06-02 16:50_

I played with this by hand and convinced myself it was correct:

https://github.com/astral-sh/uv/blob/d65c146b2179abcbef62ff81ee24c24468800cad/crates/uv-git/src/git.rs#L342-L345

When I try to make something that looks like a commit, that `^0` suffix makes it fail (I think that's why you added it :smile:):

```
$ cd `mktemp -d`
$ git init
Initialized empty Git repository in /tmp/tmp.fXWCZPpOwN/.git/
$ echo hi > file.txt
$ git add -A
$ git commit -am "first commit"
[main (root-commit) b1ea66a] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 file.txt
$ git checkout -b aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Switched to a new branch 'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'
$ git rev-parse aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa 2>/dev/null && echo success || echo fail
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
success
$ git rev-parse 'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa^0' 2>/dev/null && echo success || echo fail
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa^0
fail
```

---

_Comment by @oconnor663 on 2025-06-02 16:56_

Hmm, I'm seeing [this test failure in CI](https://github.com/astral-sh/uv/actions/runs/15359720277/job/43225148460?pr=13748) (weirdly only on Ubuntu?):

```
        FAIL [   2.359s] uv::it sync::sync_script
──── STDOUT:             uv::it sync::sync_script

running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: sync_script-5
Source: crates/uv/tests/it/sync.rs:8128
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 2
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-Using script environment at: [CACHE_DIR]/environments-v2/script-[HASH]
          5 │+Recreating script environment at: [CACHE_DIR]/environments-v2/script-[HASH]
    6     6 │ error: `uv sync --locked` requires a script lockfile; run `uv lock --script script.py` to lock the script
────────────┴───────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
test sync::sync_script ... FAILED
```

But I'm not able to repro the failure locally with `cargo test sync_script`. Does this look like the kind of thing that could be a CI flake?

---

_Comment by @konstin on 2025-06-02 17:04_

That's one the common CI flake unfortunately, one that we should invest into fixing: https://github.com/astral-sh/uv/issues/13745

---

_Comment by @oconnor663 on 2025-06-02 17:07_

Indeed, I just reran it and got green :p

---

_@ibraheemdev reviewed on 2025-06-02 17:50_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/source.rs`:113 on 2025-06-02 17:50_

Ah right, `^0` works as well.

---

_Comment by @zanieb on 2025-06-02 18:43_

I probably would label this as an enhancement rather than a bug, since there's not actually a correctness issue.

---

_Comment by @zanieb on 2025-06-02 18:46_

This seems reasonable to me. Are there any downsides?

---

_Label `enhancement` added by @oconnor663 on 2025-06-03 13:49_

---

_Label `bug` removed by @oconnor663 on 2025-06-03 13:49_

---

_@charliermarsh reviewed on 2025-06-06 12:45_

---

_Review comment by @charliermarsh on `crates/uv-git/src/source.rs`:110 on 2025-06-06 12:45_

Do we need this check, or is the `let Ok` on `maybe_commit.parse::<GitOid>()` sufficient?

---

_Comment by @charliermarsh on 2025-06-06 12:45_

I think this is generally correct and what I was looking for.

---

_@oconnor663 reviewed on 2025-06-06 18:09_

---

_Review comment by @oconnor663 on `crates/uv-git/src/source.rs`:110 on 2025-06-06 18:09_

Kind of surprising to me, but `parse` is not sufficient: https://github.com/astral-sh/uv/blob/0109af1aa510bd4f0759b0d53d842c3716e5cc3f/crates/uv-git-types/src/oid.rs#L8

I might experiment with making `GitOid` stricter. Like we discussed before how there's probably not actually a good reason to support OIDs less than 40 chars.

---

_@oconnor663 reviewed on 2025-06-06 23:10_

---

_Review comment by @oconnor663 on `crates/uv-git-types/src/oid.rs`:54 on 2025-06-06 23:10_

I've added strict `GitOid` parsing to this PR, since it simplifies the new code in `sources.rs` and it doesn't seem to break anything.

---

_@oconnor663 reviewed on 2025-06-06 23:14_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/pip_install.rs`:2136 on 2025-06-06 23:14_

This test occasionally flakes for me, and I can't understand why. Sometimes, rarely, the `Resolved 1 package` line comes after the `Updating`/`Updated` lines. It doesn't look like that should be possible when I read the code, because this call to `operations::resolve`:

https://github.com/astral-sh/uv/blob/dc3fd464728fe0456c26e79beb56017d8d56a08d/crates/uv/src/commands/pip/install.rs#L466

...happens strictly before this call to `operations::install`...

https://github.com/astral-sh/uv/blob/dc3fd464728fe0456c26e79beb56017d8d56a08d/crates/uv/src/commands/pip/install.rs#L508

I'm clearly overlooking something, but I'm not sure what. Maybe the answer is to just filter out the "Resolved..." line, but I want to understand it.

---

_Comment by @oconnor663 on 2025-06-06 23:26_

> Are there any downsides?

The only downside I can think of (other than added code complexity) is the cost of shelling out to `git rev-parse` before we fetch. That's obviously worth it in the cached case where it lets us skip the rest of the fetch entirely, but it's an additional cost in the initial, uncached case that we weren't paying before. That said, the cost of the fetch that you're going to do in that case will dwarf the `rev-parse`, so I don't think it matters much?

---

_Marked ready for review by @oconnor663 on 2025-06-06 23:47_

---

_Comment by @oconnor663 on 2025-06-06 23:49_

Still not sure about the flake I mentioned above, but still I'm moving this out of Draft Status :) I found it hard to get rid of that temporary closure I'm using, so that's still in from the draft, though the `GitOid` change makes it at bit better.

---

_Review comment by @zanieb on `crates/uv-git/src/source.rs`:95 on 2025-06-09 21:53_

Nit: we'd probably style as this
```suggestion
            // If we have a locked revision, and we have a pre-existing database which has that
```

---

_@zanieb reviewed on 2025-06-09 21:53_

---

_@zanieb reviewed on 2025-06-09 21:54_

---

_Review comment by @zanieb on `crates/uv-git/src/source.rs`:105 on 2025-06-09 21:54_

https://github.com/astral-sh/uv/pull/13748/files#r2136565690
```suggestion
            // and we do have a pre-existing database, then check whether it is, in fact, a commit
```

---

_@zanieb reviewed on 2025-06-09 21:54_

---

_Review comment by @zanieb on `crates/uv-git/src/source.rs`:120 on 2025-06-09 21:54_

```suggestion
            // ... otherwise, we use this state to update the Git database. Note that we still check
```

---

_@zanieb reviewed on 2025-06-09 21:56_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:731 on 2025-06-09 21:56_

```suggestion
    /// Disable GitHub-specific requests that allow uv to skip `git fetch` in some circumstances.
```

---

_@zanieb approved on 2025-06-09 21:56_

---

_Comment by @oconnor663 on 2025-06-09 22:43_

I finally tracked down the test flake. The common case would take this path (`-v` logs):

```
DEBUG At least one requirement is not satisfied: git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
DEBUG Using request timeout of 30s
DEBUG Attempting GitHub fast path for: git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
DEBUG Querying GitHub for commit at: https://api.github.com/repos/astral-test/uv-public-pypackage/commits/b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
DEBUG Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/astral-test/uv-public-pypackage/b270df1a2fb5d012294e9aaf05e7e0bab1e6a389/pyproject.toml
DEBUG Found static metadata via GitHub fast path for: git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
```

But the uncommon case (which happened to trigger here in CI, maybe for rate limit reasons) would take this path:

```
DEBUG At least one requirement is not satisfied: git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
DEBUG Using request timeout of 30s
DEBUG Attempting GitHub fast path for: git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
DEBUG Querying GitHub for commit at: https://api.github.com/repos/astral-test/uv-public-pypackage/commits/b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
DEBUG No netrc file found
DEBUG GitHub API request failed for: https://api.github.com/repos/astral-test/uv-public-pypackage/commits/b270df1a2fb5d012294e9aaf05e7e0bab1e6a389 (403 Forbidden)
DEBUG Fetching source distribution from Git: https://github.com/astral-test/uv-public-pypackage
```

This failure in the GitHub fast path would lead us to fetch the repo as part of resolution, instead of a part of building, which appeared as a reordering of the `Resolved` and `Updating`/`Updated` logs. Disabling the GitHub fast path entirely currently makes the test reliable.

@charliermarsh @ibraheemdev, this is ready for a re-review.

---

_Comment by @zanieb on 2025-06-09 22:58_

I think you're fine to merge unless you think any reviewers have outstanding concerns or you feel uncertain about something in particular.

---

_Comment by @oconnor663 on 2025-06-09 23:37_

This should also fix https://github.com/astral-sh/uv/issues/12746.

---

_Merged by @oconnor663 on 2025-06-09 23:50_

---

_Closed by @oconnor663 on 2025-06-09 23:50_

---

_Branch deleted on 2025-06-09 23:50_

---

_@konstin reviewed on 2025-06-10 09:22_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:2136 on 2025-06-10 09:22_

Is that the one resolved by `UV_NO_GITHUB_FAST_PATH`?

---
