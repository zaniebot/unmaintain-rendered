```yaml
number: 8191
title: "Add an `--exit-non-zero-on-fix` equivalent to the formatter."
type: issue
state: closed
author: lwatsondev
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2023-10-25T00:40:49Z
updated_at: 2025-03-19T14:55:06Z
url: https://github.com/astral-sh/ruff/issues/8191
synced_at: 2026-01-10T11:09:50Z
```

# Add an `--exit-non-zero-on-fix` equivalent to the formatter.

---

_Issue opened by @lwatsondev on 2023-10-25 00:40_

Title says it all really. `--exit-non-zero-on-change`? I think this would be useful when using tools like pre-commit with CI in the same way as the checker's `--exit-non-zero-on-fix`.

---

_Label `formatter` added by @MichaReiser on 2023-10-25 00:56_

---

_Comment by @MichaReiser on 2023-10-25 00:57_

How does your setup look like? Do you use `--diff` or `--check`? The formatter already exits with 0 when not using `--check` or `--diff`. 

How do you detect changes when using `ruff` in CI when using `--diff` or `--check` when it exists with 0?

---

_Comment by @lwatsondev on 2023-10-25 01:13_

> How does your setup look like?

https://github.com/TheReverend403/cappuccino/blob/main/.pre-commit-config.yaml#L34-L44

> Do you use --diff or --check? The formatter already exits with 0 when not using --check or --diff.

I already use `--check` and I know the formatter exits 0 without it. What I'm suggesting is a way to make it format *and* exit non-zero if anything changed in the same way as `check --fix --exit-non-zero-on-fix`. The problem with using `--check` alone is that if you run `pre-commit` locally, you first have to run `pre-commit` to see that things need formatting, then run `ruff format` separately without `--check`. I can't just remove `--check` because then when the CI runs `pre-commit`, it won't fail if somebody committed code without first formatting it, it will just quietly make the changes which is not useful on CI where those changes won't persist.

---

_Comment by @MichaReiser on 2023-10-25 01:16_

> I already use `--check` and I know the formatter exits 0 without it. What I'm suggesting is a way to make it format _and_ exit non-zero if anything changed in the same way as `check --fix --exit-non-zero-on-fix`.

`ruff format --check` has different semantics than `ruff check --fix`. It only checks if files would be reformatted but it doesn't change the files on disk. You want to use `ruff format` (without check) if you want to format AND persist the files.

---

_Comment by @lwatsondev on 2023-10-25 01:20_

Updated my previous comment.

---

_Comment by @MichaReiser on 2023-10-25 02:12_

Oh sorry. I misunderstood you. If I understand you correctly, you're asking that `ruff format --check` exits with a non-zero exit code if it would reformat any files but it would also write the changes back to disk so that running `ruff format` isn't necessary locally. 

I'm a bit torn where to integrate this functionality best. We could either to `ruff format --exit-non-zero-on-fix` (undecided on the name of the flag) without using check. However, the option would be incompatible when using with `--check` or `--diff`. 

Did you use black before? If so, how did your setup look like?

---

_Comment by @zanieb on 2023-10-25 03:01_

> We could either to ruff format --exit-non-zero-on-fix (undecided on the name of the flag) without using check. However, the option would be incompatible when using with --check or --diff.

This seems like a reasonable approach if we do implement it. It isn't incompatible with `--check` though, it just has no effect. With `--diff` I don't know our exit code behavior.

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-25 03:05_

---

_Comment by @lwatsondev on 2023-10-25 09:57_

> If I understand you correctly, you're asking that `ruff format --check` exits with a non-zero exit code if it would reformat any files but it would also write the changes back to disk so that running `ruff format` isn't necessary locally.

Exactly, but as an additional flag rather than changing `--check` so as to avoid breaking everyone's current CI workflows. 😏

> Did you use black before? If so, how did your setup look like?

[I used black](https://github.com/TheReverend403/cappuccino/blob/835b34d92e2c83d9e1766d383ece40cb7456c909/.pre-commit-config.yaml#L12-L16), just not with this behaviour. It was actually ruff's own `--exit-non-zero-on-fix` that gave me this idea for the formatter. 

---

_Comment by @henryiii on 2023-10-26 13:54_

FYI, `--exit-non-zero-on-fix` in the checker isn't required in pre-commit; pre-commit checks for changed files.

---

_Comment by @zanieb on 2023-10-26 13:58_

Hm so `ruff format` is all you should need in pre-commit? Having pre-commit tooling check for differences makes a lot more sense than adding more exit code flags to Ruff.

---

_Comment by @henryiii on 2023-10-26 13:59_

Yes, a non-zero exit code has always only been required if there were no file changes.

---

_Comment by @lwatsondev on 2023-10-26 14:14_

Oh shit you're right. How did I overlook the fact that pre-commit has its own exit code? Idiot. 😔

---

_Comment by @zanieb on 2023-10-26 15:29_

Good to hear it works :) I don't think this is justified then, I'm not sure what use-case would require this behavior.

---

_Closed by @zanieb on 2023-10-26 15:29_

---

_Comment by @noamraph on 2023-12-25 19:42_

@zanieb I do have a need for this, so perhaps this could be re-opened? My use case: I can't use pre-commit, because my project is a subdirectory in a larger git repo ("monorepo"). pre-commit decided to [not support](https://github.com/pre-commit/pre-commit/issues/466#issuecomment-921945015) this use case, so I have my own "pre-commit" script, which indeed, I want to fail if it makes changes. Currently I wrote this:

```
if ! ruff format --check .; then ruff format .; false; fi
```

but having a --exit-non-zero-on-fix would be much nicer.

If this is approved, I will be happy to try and implement this.

Thanks!

---

_Reopened by @zanieb on 2024-01-07 17:50_

---

_Label `needs-decision` added by @zanieb on 2024-01-07 17:50_

---

_Comment by @MichaReiser on 2024-01-08 07:55_

@noamraph I'm trying to understand your use case and I'm sorry if this is obvious but my bash knowledge is rather terrible :sweat: 

What I understand from the script is that it runs `ruff format .` only if `ruff format --check` failed. Can you explain the reason for it? I would also be interested to understand what functionality you miss from using `ruff format --diff`. It doesn't write the files as `ruff format` does.

---

_Comment by @noamraph on 2024-01-08 12:33_

@MichaReiser it's convenient for me to have one script which formats files and returns a nonzero code if it modified files. This is the behavior of the pre-commit tool. It's convenient because I use the same tool on my machine before committing, to format all the files, and also in CI, to block a pull request which isn't formatted according to the rules. For running on my machine, I need it to format the files. For running in CI, I need it to return nonzero if files aren't formatted. It's convenient to combine the two uses.

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-01-11 14:08_

---

_Comment by @danieleades on 2024-02-22 11:53_

the use case for us would be to have a single `format` makefile target that can be used both for:

- developers formatting their local repo
- CI jobs checking the formatting

something like

```makefile
.PHONY: format
format:
	poetry run ruff format . --exit-error-on-fix
```

without this we need separate targets for dev and CI

```makefile
.PHONY: format-check
format-check:
	poetry run ruff format . --diff

.PHONY: format
format:
        poetry run ruff format .
```

---

_Comment by @noamraph on 2024-06-13 16:58_

@zanieb I have another use case: I'm trying leftcommit (a pre-commit alternative) and it doesn't check if the files were changed.

If you want, I hope I can submit a PR.

---

_Comment by @rayjlinden on 2024-08-23 15:56_

Also need this.  Like above its fine to make the changes "ruff format" - we just need to know if it ended up making any changes...

If "ruff format" returned a 1 if it made changes then we know.  We stop the pre-commit.  Developer does a git status and can see we already fixed some things.  They review and do a commit and all is good.

Otherwise, as mentioned before we have to run ruff twice.  (Though since it is so much faster than black that isn't as bad as it could be!  :)
 

---

_Comment by @chadrik on 2024-12-26 01:29_

I have a similar need.   I'm moving to [moon](https://moonrepo.dev/) for monorepo task management, and it has most of what I need for a pre-commit alternative, and by eliminating pre-commit I can have a single source of truth for task execution. 

As a developer running a formatter on pre-commit, I want the formatter to format my code, but I _also_ want pre-commit to abort so that the user can review the changes that were made (same behavior as with `pre-commit` tool).   As it stands, I have to run `ruff format` twice:  with and without `--check`. 


---

_Comment by @thejcannon on 2025-02-05 03:09_

Toss my name in the mix of users "who want this", I'm trying out using `lefthook`: https://github.com/evilmartians/lefthook

---

_Comment by @zanieb on 2025-02-05 04:47_

I think I'm okay with adding this for parity with `ruff check`.

Why aren't various `git` command sufficient for checking if files have changed?

---

_Comment by @thejcannon on 2025-02-05 15:49_

My 2c: They (maybe*) are, but that's bad ergonomics.

*: You'd have to either fold the `git` checking into the command running `ruff format` or the overarching command which is running `ruff format` (and there's a chance both is impossible/improbable)

---

_Closed by @ntBre on 2025-03-19 14:55_

---
