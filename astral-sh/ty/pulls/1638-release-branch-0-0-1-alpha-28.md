```yaml
number: 1638
title: Release branch 0.0.1 alpha.28
type: pull_request
state: merged
author: oconnor663
labels:
  - release
assignees: []
merged: true
base: main
head: release_branch_0.0.1-alpha.28
created_at: 2025-11-25T21:26:46Z
updated_at: 2025-11-26T00:45:01Z
url: https://github.com/astral-sh/ty/pull/1638
synced_at: 2026-01-12T15:54:27Z
```

# Release branch 0.0.1 alpha.28

---

_@oconnor663_

I've kept the edits to the CHANGELOG in a separate commit here so that we can eyeball them.

On a lark I've also put up https://github.com/astral-sh/ty/pull/1636 to see if we might want to pin commit hashes in "View source" links, but I haven't used that here. If we wanted to, I could rebase it.

---

_@oconnor663 reviewed on 2025-11-25 21:27_

---

_Review comment by @oconnor663 on `CHANGELOG.md`:997 on 2025-11-25 21:27_

Have past releasers been editing out the autoformatting corrections by hand maybe?

---

_@carljm reviewed on 2025-11-25 21:33_

---

_Review comment by @carljm on `CHANGELOG.md`:997 on 2025-11-25 21:33_

I don't recall anything like this coming up before. Are you sure this wasn't done by your editor, rather than any in-repo-configured tooling?

---

_Review comment by @carljm on `CHANGELOG.md`:15 on 2025-11-25 21:34_

```suggestion
- Add go-to-definition for `Unknown` when it appears in an inlay hint ([#21545](https://github.com/astral-sh/ruff/pull/21545))
```

---

_Review comment by @carljm on `CHANGELOG.md`:16 on 2025-11-25 21:36_

```suggestion
- Add go-to-definition support for more types ([#21546](https://github.com/astral-sh/ruff/pull/21546))
```

---

_Review comment by @carljm on `CHANGELOG.md`:17 on 2025-11-25 21:37_

```suggestion
- Add go-to-definition support  for typing special forms ([#21544](https://github.com/astral-sh/ruff/pull/21544))
```

---

_Review comment by @carljm on `CHANGELOG.md`:18 on 2025-11-25 21:37_

```suggestion
- Don't suggest completions that aren't subclasses of `BaseException` after `raise` ([#21571](https://github.com/astral-sh/ruff/pull/21571))
```

---

_Review comment by @carljm on `CHANGELOG.md`:29 on 2025-11-25 21:38_

```suggestion
- Resolve applicable overloads for hover on an overloaded function call ([#21417](https://github.com/astral-sh/ruff/pull/21417))
```

---

_Review comment by @carljm on `CHANGELOG.md`:35 on 2025-11-25 21:39_

```suggestion
- Exit with code `2` if there's any IO error ([#21508](https://github.com/astral-sh/ruff/pull/21508))
```

---

_Review comment by @carljm on `CHANGELOG.md`:41 on 2025-11-25 21:39_

```suggestion
- Avoid expression re-inference for diagnostics ([#21267](https://github.com/astral-sh/ruff/pull/21267))
```

---

_@oconnor663 reviewed on 2025-11-25 21:39_

---

_Review comment by @oconnor663 on `CHANGELOG.md`:997 on 2025-11-25 21:39_

Hmm, if I run `release.sh` on a clean branch off `origin/main`, I still see these edits. If it doesn't repro that easily for you, I wonder if I have a different local version of something.

---

_@oconnor663 reviewed on 2025-11-25 21:42_

---

_Review comment by @oconnor663 on `CHANGELOG.md`:997 on 2025-11-25 21:42_

Specifically, if I update the `ruff` submodule and then `git add` that, then I can run `uv run --isolated --only-group release rooster release` and see these edits (in addition to the new changelog entry at the top).

---

_Review comment by @carljm on `CHANGELOG.md`:58 on 2025-11-25 21:42_

```suggestion
- Support PEP 613 `typing.TypeAlias` type aliases ([#21394](https://github.com/astral-sh/ruff/pull/21394))
```

---

_@carljm reviewed on 2025-11-25 21:42_

---

_@carljm reviewed on 2025-11-25 21:44_

---

_Review comment by @carljm on `CHANGELOG.md`:997 on 2025-11-25 21:44_

I'm not sure (and I haven't tried to repro locally). cc @zanieb re Rooster. I need to run to lunch now but can look later. I guess maybe we can manually edit these out for now to keep the release moving?

---

_@oconnor663 reviewed on 2025-11-25 21:48_

---

_Review comment by @oconnor663 on `CHANGELOG.md`:997 on 2025-11-25 21:48_

Ah ok, CI is failing when it runs the pre-commit hooks. It looks like I just hadn't set those up. When I do a `pre-commit install` and redo this, the diff goes away (actually it fails the first attempt to commit). I'll manually edit them out of this PR.

---

_@oconnor663 reviewed on 2025-11-25 21:53_

---

_Review comment by @oconnor663 on `CHANGELOG.md`:997 on 2025-11-25 21:53_

I've also tweaked the release instructions to call this out: 47b6ad04258335134238578f2abe2a7e41f219a0

---

_@carljm approved on 2025-11-25 23:35_

Looks good!

---

_Comment by @carljm on 2025-11-25 23:37_

(I'd rather leave #1636 for review by someone with more context on our docs build, so let's ship this one without it. Thanks for observing that opportunity for improvement, hopefully we can get it in for the beta.)

---

_Merged by @oconnor663 on 2025-11-26 00:11_

---

_Closed by @oconnor663 on 2025-11-26 00:11_

---

_Branch deleted on 2025-11-26 00:11_

---

_Label `release` added by @AlexWaygood on 2025-11-26 00:18_

---

_Comment by @oconnor663 on 2025-11-26 00:32_

`schemastore` followup: https://github.com/SchemaStore/schemastore/pull/5166

---

_Comment by @oconnor663 on 2025-11-26 00:45_

`ty-vscode` followup: https://github.com/astral-sh/ty-vscode/pull/219

---
