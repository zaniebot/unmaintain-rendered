```yaml
number: 16749
title: "[red-knot] add support for `--output-format={full,concise}`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/concise-diagnostics
created_at: 2025-03-14T15:01:07Z
updated_at: 2025-03-14T18:46:19Z
url: https://github.com/astral-sh/ruff/pull/16749
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] add support for `--output-format={full,concise}`

---

_@BurntSushi_

This does pretty much what it says on the tin.

The concise format used in this PR was partially resurrected from
the diagnostic rendering used before #15856 landed. Specifically,
the concise format guarantees that every diagnostic uses exactly one
line of output. (This guarantee isn't technically enforced in every
conceivable way. For example, there's nothing stopping a diagnostic
message from containing multiple lines, but this seems like a bad idea
in general anyway.)

The motivation for this functionality is to provide a way to get
succinct diagnostics for whenever that is useful. For [example].

Many thanks to @MichaReiser for giving me the play-by-play on
how to plumb a new CLI option. Saved me a lot of time. :-)

[example]: https://github.com/astral-sh/ruff/issues/15697#issuecomment-2706477278


---

_Review requested from @carljm by @BurntSushi on 2025-03-14 15:01_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-03-14 15:01_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-03-14 15:01_

---

_Review requested from @sharkdp by @BurntSushi on 2025-03-14 15:01_

---

_Comment by @github-actions[bot] on 2025-03-14 15:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-03-14 15:18_

This works great, thank you. One minor thing I noticed is that VS code has problems detecting the `path/to/file.py:<row>:<column>` pattern (for ctrl+click from the terminal). That seems to be caused by the colons right in front of and right behind the path/row/column triple:
```
warning[lint:unresolved-reference]:/home/shark/black/black/src/black/linegen.py:1284:19:Name `current_line` used when not defined
```

If we would split on a different character, that might work better? (I only tried space)

---

_Comment by @BurntSushi on 2025-03-14 15:21_

I was unwise to not remember the lesson of Chesterton's fence. So that's why the original output used spaces. I replaced spaces with colons thinking it looked nicer and was more consistent with a grep-like format.

---

_Comment by @github-actions[bot] on 2025-03-14 15:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @BurntSushi on 2025-03-14 15:26_

@sharkdp All righty, that should be fixed now, by using spaces.

(An alternative would be to use hyperlinks. Then we could use whatever _visual_ format we wanted while retaining conveniences like VS Code handling it correctly. But IDK if that's worth it and I think out of scope for this PR.)

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:230 on 2025-03-14 15:29_

I'd remove the mention of `color`. We'll use a separate flag `color` (auto, always, never) to control the color behavior

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/old.rs`:87 on 2025-03-14 15:31_

Not related to this PR, just something I noted and I probably never mentioned before. We should print paths that are relative to `db.system().current_directory()` (or `db.project().root()` but I think current directory is more correct)



---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:985 on 2025-03-14 15:33_

Nit: Should we use a colon between the path and the message?
```suggestion
    error[lint:non-subscriptable] <temp_dir>/test.py:3:7: Cannot subscript object of type `Literal[4]` with no `__getitem__` method
```

---

_@MichaReiser approved on 2025-03-14 15:33_

Nice. This is great. 

I'd recommend removing the mentions of `color` from `OutputFormat`. The plan is to add a `color` option to `TerminalOptions` that controls coloring.

---

_Label `red-knot` added by @MichaReiser on 2025-03-14 15:34_

---

_Comment by @sharkdp on 2025-03-14 15:37_

> So that's why the original output used spaces. I replaced spaces with colons thinking it looked nicer and was more consistent with a grep-like format.

I sympathize with the grep-ability/good-CLI-citizen argument, but `:` is probably not ideal there anyway since we also use colons for the `lint:unresolved-reference` split, so if you would e.g. use `cut -d':' -f…`, the first field would be `warning[lint`.

In terms of human readability, I'm not sure I agree colons-everywhere looks nicer :smile:.

> (An alternative would be to use hyperlinks. Then we could use whatever _visual_ format we wanted while retaining conveniences like VS Code handling it correctly. But IDK if that's worth it and I think out of scope for this PR.)

Yes :+1:. Unfortunately, I think it's hard to get `file://` links working cross-tool correctly (I think you are aware, I vaguely remember following some discussion on the ripgrep issue tracker). So then you also need a way for users to customize those OSC 8 links to e.g. `vscode://` or similar, and at that point it seems easier to just use a different separation character, I think.

But hyperlinks are still an interesting idea for our diagnostics in general, I believe. For example, we could make the line numbers in a multi-line diagnostic message hyperlinks.

---

_@BurntSushi reviewed on 2025-03-14 15:49_

---

_Review comment by @BurntSushi on `crates/red_knot/tests/cli.rs`:985 on 2025-03-14 15:49_

I don't think that's what was there before: https://github.com/astral-sh/ruff/pull/15856/files#diff-dbafb0caf34282c9452acb4c5f0cd3b5c3e6f0070a66069e5762046142d7879bL235

I'm not sure if adding it will break links? I'm also not sure I like the way it looks. :P 

---

_Comment by @BurntSushi on 2025-03-14 15:51_

Yeah terminal hyperlinks are minefield of terrors. But users love them. And yes, I did indeed go through that process with ripgrep, and actually, I haven't had many complaints or bug reports. So either I got it right, or nobody is using them haha. So if we do want to do hyperlinks for diagnostics, I do at least have some helpful domain knowledge there.

(But you're absolutely right that the specific hyperlink format needs to be configurable in some way, and I don't think there is any standard way of auto-detecting it. So it's kind of a huge bummer.)

---

_@BurntSushi reviewed on 2025-03-14 15:52_

---

_Review comment by @BurntSushi on `crates/red_knot/src/args.rs`:230 on 2025-03-14 15:52_

Fair enough, done.

---

_@MichaReiser reviewed on 2025-03-14 15:52_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:985 on 2025-03-14 15:52_

I guess my main feedback is: I find it hard to see where the message starts. 

This is what ruff uses today

```
crates/ruff_python_resolver/resources/test/airflow/airflow/api/common/mark_tasks.py:1:1: I002 [*] Missing required import: `from __future__ import annotations`
```

The colon after the line number doesn't seem to break VS code


I won't argue with preferences and I'm also fine iterating on this later ;)

---

_@BurntSushi reviewed on 2025-03-14 15:52_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/old.rs`:87 on 2025-03-14 15:52_

Just to be clear, what I have here is fine for now and you're noting something for the future?

---

_@MichaReiser reviewed on 2025-03-14 15:54_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/old.rs`:87 on 2025-03-14 15:54_

Yes. What you have is fine. We'll have to change this for both output formats. Sorry for not stating this more clearly

---

_@BurntSushi reviewed on 2025-03-14 18:31_

---

_Review comment by @BurntSushi on `crates/red_knot/tests/cli.rs`:985 on 2025-03-14 18:31_

I don't have a strong preference. My main concern was definitely about breaking VS Code clicking.

Since it doesn't break thing, my main concern is resolved. So I added in the `:` here. :)

---

_Merged by @BurntSushi on 2025-03-14 18:46_

---

_Closed by @BurntSushi on 2025-03-14 18:46_

---

_Branch deleted on 2025-03-14 18:46_

---
