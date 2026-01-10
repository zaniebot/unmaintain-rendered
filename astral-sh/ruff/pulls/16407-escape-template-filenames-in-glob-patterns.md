```yaml
number: 16407
title: Escape template filenames in glob patterns
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/globbing
created_at: 2025-02-26T22:17:19Z
updated_at: 2025-03-03T14:30:01Z
url: https://github.com/astral-sh/ruff/pull/16407
synced_at: 2026-01-10T19:49:01Z
```

# Escape template filenames in glob patterns

---

_Pull request opened by @ntBre on 2025-02-26 22:17_

## Summary

Fixes #9381. This PR fixes errors like 

```
Cause: error parsing glob '/Users/me/project/{{cookiecutter.project_dirname}}/__pycache__': nested alternate groups are not allowed
```

caused by glob special characters in filenames like `{{cookiecutter.project_dirname}}`. When the user is matching that directory exactly, they can use the workaround given by https://github.com/astral-sh/ruff/issues/7959#issuecomment-1764751734, but that doesn't work for a nested config file with relative paths. For example, the directory tree in the reproduction repo linked [here](https://github.com/astral-sh/ruff/issues/9381#issuecomment-2677696408):

```
.
├── README.md
├── hello.py
├── pyproject.toml
├── uv.lock
└── {{cookiecutter.repo_name}}
    ├── main.py
    ├── pyproject.toml
    └── tests
        └── maintest.py
```

where the inner `pyproject.toml` contains a relative glob:

```toml
[tool.ruff.lint.per-file-ignores]
"tests/*" = ["F811"]
```

## Test Plan

A new CLI test in both the linter and formatter. The formatter test may not be necessary because I didn't have to modify any additional code to pass it, but the original report mentioned both `check` and `format`, so I wanted to be sure both were fixed.


---

_Label `bug` added by @ntBre on 2025-02-26 22:17_

---

_Comment by @github-actions[bot] on 2025-02-26 22:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @ntBre on 2025-02-26 23:15_

I'm not sure what's going wrong on Windows. It looks like the `tempdir_filter` isn't working. I force pushed back over it, but I even tried removing my second filter [here](https://github.com/astral-sh/ruff/actions/runs/13554843187/job/37886774376) and the temp path is still wrong.

---

_Review requested from @BurntSushi by @MichaReiser on 2025-02-27 07:56_

---

_Comment by @MichaReiser on 2025-02-27 07:59_

> I'm not sure what's going wrong on Windows. It looks like the `tempdir_filter` isn't working. I force pushed back over it, but I even tried removing my second filter [here](https://github.com/astral-sh/ruff/actions/runs/13554843187/job/37886774376) and the temp path is still wrong.

Yeah, the filters are brutal on windows. Can you try what we have in 

https://github.com/astral-sh/ruff/blob/3e840ca839e40d875e28ff7f15c94d88c4d6e7ae/crates/red_knot/tests/cli.rs#L863-L865

---

_@MichaReiser reviewed on 2025-02-27 08:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:665 on 2025-02-27 08:02_

This, indeed, feels somewhat hacky :D 

I think my main concern with this is that it replaces nested alternates anywhere in the glob -- even in the part that the user provided. 

What I understand is that the problem is mainly about `{{` and `}}` in the base path up to the project. Could we escape the `{{` and `}}` in the project's base-path only instead of applying this fix to the entire glob?

---

_@ntBre reviewed on 2025-02-27 14:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:665 on 2025-02-27 14:44_

That's true, although the nested alternates will always cause an error even if the user provided them. I thought it might be nice to allow them to avoid manual escaping in that case, but at least they *can* control that directly, unlike the base paths.

We could also add the escapes to the whole path just before creating the glob. I think that version would also look less hacky than catching this error and retrying, but then we also do the work of trying to replace each time. That was the main reason I tried to keep the replacement in the error case, but we're still only compiling the globs once, so it's probably not a big deal.

---

_Comment by @ntBre on 2025-02-27 15:05_

Just pushing a test for Windows, I haven't addressed the hacky path stuff yet.

---

_Comment by @ntBre on 2025-02-27 15:14_

It looks like the regex escapes in `tempdir_filter` are causing problems, but it works for all of the other tests using this...

```
C:\/Users\/RUNNER~1\/AppData\/Local\/Temp\/.tmpCazbkF\\{{cookiecutter.repo_name}}\/tests\\*
```

OH, maybe it's the cookiecutter causing problems again. The escapes for the braces might be getting in the way of the tempdir replacement. Let me try nesting it one level deeper.

---

_@MichaReiser reviewed on 2025-02-27 15:14_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:665 on 2025-02-27 15:14_

> That's true, although the nested alternates will always cause an error even if the user provided them. I thought it might be nice to allow them to avoid manual escaping in that case, but at least they can control that directly, unlike the base paths.

The problem I see with this is that they may have wanted to use nested alternates and are now surprised that the glob doesn't match anything.



---

_Comment by @MichaReiser on 2025-02-27 15:15_

You can try to use `regex::escape` to avoid regex problems

---

_Comment by @ntBre on 2025-02-27 15:19_

`tempdir_filter` uses `regex::escape` already, unless I'm misunderstanding. My current guess is that the escapes added by `regex::escape` are disrupting the TMP replacement because they end up right next to the tempdir.

```
C:\/Users\/RUNNER~1\/AppData\/Local\/Temp\/.tmpCazbkF\\{{cookiecutter.repo_name}}\/tests\\*
                                                     ^^^ this looks suspicious to me
```

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/settings/types.rs`:659 on 2025-02-27 16:13_

This is also not _technically_ right in all cases I think, e.g., `[{{]`, although that's pretty pathological.

---

_@BurntSushi reviewed on 2025-02-27 16:16_

Thanks for taking a look at this!

I think the thing I'm not understanding here is why nested alternates are the only problem here? Isn't the problem that we're using arbitrary input to build a glob? And shouldn't that imply the input (in this case, a file path) should always be escaped? Like, the nested alternates is just _one_ way for this problem to manifest. But what if a file path had a `*` or a `[` in it? We would want to escape it there too.

So I guess, can we fix this by just always escaping the literal inputs to a glob?

---

_Comment by @ntBre on 2025-02-27 16:42_

Thanks for the review! I failed to notice the `globset::escape` function. I think we can just use that on the base part of the path that we're adding to the glob pattern. That will be much more robust than my manual escaping here and also leave the user's glob alone in case that's what they intended.

---

_Comment by @ntBre on 2025-02-27 19:48_

This is now a much simpler fix, thanks again for the reviews and suggestions! I also removed the warning, which was the source of the path in the snapshot, ~~so hopefully the Windows tests will pass too~~.

I realized it was also possible to trigger this for the non-absolute path, although in a pretty niche case. The test I added extends `per-file-ignores` with the working directory set to `tempdir/{{cookiecutter.repo_name}}`. I peeked at the implementation of `Path::absolutize` and it just calls `Path::absolutize_from` with `CWD` (via the private `get_cwd!` macro), so I think the change here should be safe.

---

_Comment by @ntBre on 2025-02-27 20:05_

I was checking for the other uses of `Glob::new`, and I think this could also be a problem for the `FilePath::User` variant.

https://github.com/astral-sh/ruff/blob/cf83584abb130574059064ad5f0dd806c22e45d5/crates/ruff_linter/src/settings/types.rs#L162-L166

I'll start adding the `escape` calls for it too.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:239 on 2025-02-28 08:01_

Can we introduce a `cwd` helper function in `fs`. WASM doesn't like `path_dedot::CWD` 

https://github.com/astral-sh/ruff/blob/e7a6c19e3a2c88d2eaad88a311c60fbd4e733c14/crates/ruff_linter/src/fs.rs#L36-L48

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:335 on 2025-02-28 08:02_

Same here, let's avoid spreading `path_dedot::CWD` over our code base and instead use a helper to maximize WASM compatibility

---

_@MichaReiser approved on 2025-02-28 08:02_

Nice, this looks great

---

_Review requested from @BurntSushi by @MichaReiser on 2025-02-28 08:02_

---

_@BurntSushi approved on 2025-02-28 12:42_

LGTM

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:239 on 2025-02-28 16:23_

Oh good idea, I factored this out into a `get_cwd` function and call it everywhere in the linter. I left the calls in the `ruff` and `ruff_workspace` crates alone for now. Should I replace those too?

---

_@ntBre reviewed on 2025-02-28 16:23_

---

_Merged by @ntBre on 2025-03-03 14:29_

---

_Closed by @ntBre on 2025-03-03 14:29_

---

_Branch deleted on 2025-03-03 14:30_

---
