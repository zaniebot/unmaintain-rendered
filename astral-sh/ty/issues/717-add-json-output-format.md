```yaml
number: 717
title: "Add `json` output format"
type: issue
state: closed
author: Sh099078
labels:
  - cli
assignees: []
created_at: 2025-06-27T15:49:48Z
updated_at: 2025-11-14T08:59:11Z
url: https://github.com/astral-sh/ty/issues/717
synced_at: 2026-01-12T15:54:23Z
```

# Add `json` output format

---

_@Sh099078_

### Question

##### Context

Hi, I am currently testing `ty` on a pipeline to compare type checking errors between a pull request between the source and target branches and associate errors on both. The goal is to be able to do a summary indicating which errors are new in the source compared to the target branch and which ones it fixed. That pipeline runs on a legacy part of the project that already had a lot of typing errors before we added type checking in the CI, so we cannot fail the pipeline if there are any errors present. Ideally, we would only fail it if there are new errors in the pull request source branch compared to the target branch.

##### Questions: what's on the roadmap ?

Which raises two questions:
- Are there plans to allow to uniquely identify errors per error type + module + method + variable (or something alike) instead of identifying them by line / column number ? The goal would be to be able to match errors between two runs easily even when the line numbers have changed between the runs for some reason (formatter, new imports / code added to the file, etc).
- Is formatting the output to json on the roadmap ?

If your team doesn't consider that implementing that kind of identifier would be absurd, too time consuming, too complex or incompatible with your current objectives and if it is not currently planned I might create a feature request for it.

Thanks in advance!

### Version

_No response_

---

_Label `question` added by @Sh099078 on 2025-06-27 15:49_

---

_Comment by @carljm on 2025-06-27 16:16_

JSON-formatted (or generally, machine-readable) output format is planned, though it's not currently high priority.

We don't currently have any plans to try to create a unique key for a diagnostic that would be stable across line number changes. It seems like a problem with no good solution, only a variety of possible heuristics that may or may not work as expected. For reference, the [baseline feature of basedpyright](https://docs.basedpyright.com/v1.29.1/benefits-over-pyright/baseline/) just matches based on file, diagnostic code, and column number (excluding line number), which is a heuristic that could be implemented externally today (by parsing the existing concise output, or a future machine-readable output). It seems unlikely to me that we attempt more complex heuristics than this within ty.

---

_Comment by @MichaReiser on 2025-06-27 16:41_

The gitlab output format in ruff does compute a somewhat unique identifier (a truely unique identifier is hard) and @ntBre's working on porting Ruff to the same diagnostic infrastructure as ty is using. That will, hopefully, allow us to use the gitlab reporter in ty too that you can then enable with `--output-format gitlab`

---

_Comment by @akaihola on 2025-09-06 11:08_

[Graylint] is a tool which runs linters twice: once for a baseline commit and once for the working tree. It then suppresses messages which already existed for the baseline commit.

A mapping for unmodified lines between the working tree and the baseline commit is created using `difflib.SequenceMatcher` from the Python standard library. This works fairly reliably, but obviously loses the correct mapping when chunks of code are reordered.

The code for this is in the following [Darkgraylib] functions:
- [`map_unmodified_lines()`](/akaihola/darkgraylib/blob/fd2c11c62c8f85941249d7e93a5ae175fdce9f06/src/darkgraylib/diff.py#L95-L126)
- [`diff_and_get_opcodes()`](/akaihola/darkgraylib/blob/fd2c11c62c8f85941249d7e93a5ae175fdce9f06/src/darkgraylib/diff.py#L56-L79)

I hope this provides another possible heuristic to consider for a `ty` baseline feature.

Also, this issue seems to be related to these `ty` issues:
- #1074 
- #913 

[Graylint]: /akaihola/graylint
[Darkgraylib]: /akaihola/darkgraylib


---

_Renamed from "Is there an alternative way to match errors between several runs more reliably than column + line number planned on the roadmap ?" to "Add `json` output format" by @MichaReiser on 2025-11-14 08:54_

---

_Label `question` removed by @MichaReiser on 2025-11-14 08:54_

---

_Label `cli` added by @MichaReiser on 2025-11-14 08:54_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:55_

---

_Closed by @MichaReiser on 2025-11-14 08:59_

---
