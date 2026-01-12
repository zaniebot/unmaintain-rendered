```yaml
number: 19968
title: "Add `full` output format changes to the changelog"
type: pull_request
state: merged
author: ntBre
labels:
  - release
assignees: []
merged: true
base: main
head: brent/update-changelog
created_at: 2025-08-18T13:06:36Z
updated_at: 2025-08-18T15:46:17Z
url: https://github.com/astral-sh/ruff/pull/19968
synced_at: 2026-01-12T15:56:51Z
```

# Add `full` output format changes to the changelog

---

_@ntBre_

Summary
--

I thought this might warrant a small blog-style writeup, especially since we already got a question about it (#19966), but I'm happy to switch back to a one-liner under `### Other changes` if preferred.

I'll copy whatever we add here to the release notes too.

Do we need a note at the top about the late addition?


---

_Label `release` added by @ntBre on 2025-08-18 13:07_

---

_Marked ready for review by @ntBre on 2025-08-18 13:28_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-18 13:29_

---

_@MichaReiser reviewed on 2025-08-18 13:48_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:5 on 2025-08-18 13:48_

I think this headline is a bit confusing because it suggests that we now use an entirely different output format, and that users have the option to switch back to the old format. However, this is not the case. 

I think I'd phrase it something like: *Improvements to rendering of `full` output format. 


I'm also a bit torn on the blog-like write-up. The prominence of the change now suggests that this should have been part of a minor release. On the other hand, I do think it's useful for users to understand the change. 


It's a bit unfortunate that https://github.com/astral-sh/ruff/pull/19900 landed right after the release. It would have allowed us to use it as an example and explain the entire change as part of an improved rendering for F811. 

What we could try to make it less sound like a breaking change (which it isn't, because we don't consider the exact output of full to be under semver), is to set the focus on what this will enable in the future (multifile diagnostics too). As far as the format changes go, the only change I would mention is that the file name is now rendered on a separate line.

---

_@ntBre reviewed on 2025-08-18 14:26_

---

_Review comment by @ntBre on `CHANGELOG.md`:5 on 2025-08-18 14:26_

Updated the headline! (locally)

I know what you mean about the write-up. I think uv includes more text in some of their release notes, but for Ruff this is exclusive to the minor releases, so this might give the wrong idea. I could shorten the section substantially and/or move it to a sub-bullet in `### Other changes` if that would help. It would certainly be less prominent:

### Other changes

- ...
- Improve rendering of the `full` output format ([#19415](https://github.com/astral-sh/ruff/pull/19415))
	    Ruff now uses the same default diagnostic format as ty. Below is an example diff for [`F401`](https://docs.astral.sh/ruff/rules/unused-import/):
    
    ```diff
    -unused.py:8:19: F401 [*] `pathlib` imported but unused
    +F401 [*] `pathlib` imported but unused
    +  --> unused.py:8:19
        |
      7 | # Unused, _not_ marked as required (due to the alias).
      8 | import pathlib as non_alias
    -   |                   ^^^^^^^^^ F401
    +   |                   ^^^^^^^^^
      9 |
     10 | # Unused, marked as required.
        |
    -   = help: Remove unused import: `pathlib`
    +help: Remove unused import: `pathlib`
    ```
	For now, the primary change is in the header, but this new representation will allow us to make further additions to Ruff's diagnostics, such as adding sub-diagnostics and multiple annotations to the same snippet.

<hr>

Or would it be more helpful to omit the diff and only show the new F811 diagnostic? And agreed that it would have been nice to include both, I somehow didn't quite realize that this was going out in the last release and without the F811 PR.

---

_@MichaReiser reviewed on 2025-08-18 14:33_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:5 on 2025-08-18 14:33_

Moving it under Other makes sense to me. It makes it less prominent (while still prominent enough). I would omit the mention of ty, most users won't be familiar with it and it's unclear why it even matters for them.

---

_@MichaReiser approved on 2025-08-18 15:21_

Nice, I like this a lot

---

_Renamed from "Add a section on the new diagnostic format" to "Add `full` output format changes to the changelog" by @ntBre on 2025-08-18 15:43_

---

_Merged by @ntBre on 2025-08-18 15:46_

---

_Closed by @ntBre on 2025-08-18 15:46_

---

_Branch deleted on 2025-08-18 15:46_

---
