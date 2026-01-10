```yaml
number: 19552
title: Check Watch does not show output format error message from clearing too late
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - cli
assignees: []
created_at: 2025-07-25T06:09:34Z
updated_at: 2025-11-22T14:25:19Z
url: https://github.com/astral-sh/ruff/issues/19552
synced_at: 2026-01-10T11:09:59Z
```

# Check Watch does not show output format error message from clearing too late

---

_Issue opened by @MeGaGiGaGon on 2025-07-25 06:09_

### Summary

I found this while helping on #19550. When running `check` in `watch` mode with a non-full `output-format`, this error:
```
warning: `--output-format full` is always used in watch mode.
``` 
Does not get displayed due to the terminal being cleared directly after. Here's the contents of `out.txt` I get when running `uvx ruff check --watch . --output-format json 2>&1 > out.txt`:
```
warning: `--output-format full` is always used in watch mode.
[H[2J[3J[23:04:31 PM] Starting linter in watch mode...

[23:04:31 PM] Found 7 errors. Watching for file changes.

... normal error output here
```

I assume `�[H�[2J�[3J` is the magic characters that cause a terminal clear in PowerShell, which shows that it's erasing the warning. This should be fixable by clearing the terminal prior to warnings being output, though I haven't looked at the code to see how easy it would be.

Also just a side tangent thought, but it might be better if the error said `concise` instead of `full`, since it definitely does not give the same output as non-watch check on `full`.

### Version

ruff 0.12.5 (d13228ab8 2025-07-24)

---

_Label `bug` added by @MichaReiser on 2025-07-25 06:35_

---

_Label `cli` added by @MichaReiser on 2025-07-25 06:35_

---

_Comment by @ntBre on 2025-07-25 13:31_

Possibly even more surprising is the fact that `--preview` enables the `full` format:

https://github.com/astral-sh/ruff/blob/6663571c9000a081f42ccb9c9f20a6f6905d9942/crates/ruff/src/printer.rs#L460-L462

Compare the `with_show_source` call to the `write_once` implementation:

https://github.com/astral-sh/ruff/blob/6663571c9000a081f42ccb9c9f20a6f6905d9942/crates/ruff/src/printer.rs#L262-L265

But yeah, without `--preview`, the default is definitely `concise`.

---

_Comment by @mikeleppane on 2025-07-31 11:33_

Hi! I've addressed the main issue, but I have a few comments regarding the output formatting:

- What is the reason for using only "one" formatting output logic in `--watch` mode compared to non-watch mode?
- As mentioned earlier, is it an oversight that the `--preview` option enables full mode formatting?
- Is the warning: `warning: `--output-format full` is always used in watch mode.` needed in the first place? Or is it trying to say that in `watch` mode, we only apply `full` mode?

Note: I looked at the [#19550](https://github.com/astral-sh/ruff/issues/19550) for more details. I think these two can be combined and tackled together. 

---

_Comment by @ntBre on 2025-07-31 13:07_

Those are good questions. I'm not sure I know the exact answers, but I can at least weigh in :)

- I'm sure there was a reason at some point only to use one output format with `--watch`, but I think it makes sense to support `--output-format` now (possibly `--watch` was added well before we had multiple output formats)
- I think I view the `--preview` flag as changing the default output format, in the context of adding `--output-format` support. I think we can do both by continuing to default to `concise` output (with no `--ouput-format` specified) but default to `full` when `--preview` is specified. We should still stabilize that behavior in a future minor release. We should probably open a separate issue for that and add it to the 0.13 milestone. However, it might be a bit less necessary with `--output-format` support, so we could also consider dropping the preview feature. In either case, we should hold off there until the next minor release.
- If we add `--output-format` support, we should be able to drop the warning entirely, as you suggested.

---

_Comment by @MichaReiser on 2025-07-31 15:13_

> What is the reason for using only "one" formatting output logic in --watch mode compared to non-watch mode?

I don't know the reason but there's an argument that some output formats seem less useful with `--watch`. I'm not sure if it's worth restricting the output formats because of it but it could make sense to take a quick look at what other tools support with `--watch`.

---

_Comment by @MeGaGiGaGon on 2025-07-31 19:53_

It might have been to maintain backwards compatibility? Since very old versions of ruff only supported concise-style outputs, and it looks like the decision to only support one output style with watch was made here https://github.com/astral-sh/ruff/issues/181#issuecomment-1246711236

Edit: I did some more searching, and it looks also like `watch` got forgotten about when https://github.com/astral-sh/ruff/issues/7350 happened, since prior to that change (in `ruff@0.1`) `--watch` with `--show-source` does work, but after (in `ruff@0.2` +) it no longer works.

---

_Added to milestone `v0.15` by @MichaReiser on 2025-11-22 14:24_

---

_Comment by @MichaReiser on 2025-11-22 14:25_

We should stabilize https://github.com/astral-sh/ruff/pull/21097 in the next release which will fix this issue (I assigned this issue to 0.15) to make sure we stabilizie the behavior change

---
