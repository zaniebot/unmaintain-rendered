```yaml
number: 7340
title: Add a benchmarking script for the formatter CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/formatter-benchmark
created_at: 2023-09-13T15:27:56Z
updated_at: 2023-09-13T17:15:58Z
url: https://github.com/astral-sh/ruff/pull/7340
synced_at: 2026-01-12T02:39:09Z
```

# Add a benchmarking script for the formatter CLI

---

_Pull request opened by @charliermarsh on 2023-09-13 15:27_

## Summary

This PR adds a benchmarking script for the formatter, which benchmarks the Ruff formatter against Black, yapf, and autopep8.

Three benchmarks are included:

1. Format everything.
2. Format everything, but use a single thread.
3. Format everything, but `--check` (don't write to disk).

There's some nuance in figuring out the right combination of arguments to each command, but the _main_ nuance is to ensure that we always run the given formatter (and modify the target repo in-place) prior to benchmarking it, so that the formatters aren't disadvantaged by the existing formatting of the target repo. (E.g.: prior to benchmarking Black's preview style, we need to make sure we format the target repo with Black's preview style; otherwise, preview style appears much slower.)

Part of https://github.com/astral-sh/ruff/issues/7309.

---

_@charliermarsh reviewed on 2023-09-13 15:28_

---

_Review comment by @charliermarsh on `scripts/benchmarks/pyproject.toml`:17 on 2023-09-13 15:28_

Honestly don't know if Poetry is worth preserving here. I end up using `pipx` for these selectively.

---

_Label `formatter` added by @charliermarsh on 2023-09-13 15:28_

---

_@zanieb approved on 2023-09-13 15:47_

---

_Review comment by @MichaReiser on `scripts/benchmarks/run_formatter.sh`:18 on 2023-09-13 15:56_

`ignore-failure` can mask real failures. Can we remove it and replace it with e.g. `-e`?

---

_Review comment by @MichaReiser on `scripts/benchmarks/run_formatter.sh`:57 on 2023-09-13 15:59_

The comparision with `yapf` and `autopep8` is a bit unfair because the source formatting probably doesn't match their style guide -> Potentially more work, need to write every file where other tools only need to write changed file. 

---

_Review comment by @MichaReiser on `scripts/benchmarks/run_formatter.sh`:18 on 2023-09-13 16:01_

I'm not entirely sure but I believe `--setup` only runs once. Meaning, subsequent runs don't work on a clean git directory. We may want to use `--prepare` instead

---

_@MichaReiser reviewed on 2023-09-13 16:01_

---

_@charliermarsh reviewed on 2023-09-13 16:05_

---

_Review comment by @charliermarsh on `scripts/benchmarks/run_formatter.sh`:57 on 2023-09-13 16:05_

No, the warmup ensures that the first run formats the directory to match the style. (We then do this manually for the `--check` version.)

---

_@charliermarsh reviewed on 2023-09-13 16:05_

---

_Review comment by @charliermarsh on `scripts/benchmarks/run_formatter.sh`:18 on 2023-09-13 16:05_

This is intentional. We don't want to reset the directory on each invocation, otherwise the benchmarks will suffer from the issue you pointed out in the comment above.

---

_@charliermarsh reviewed on 2023-09-13 16:06_

---

_Review comment by @charliermarsh on `scripts/benchmarks/run_formatter.sh`:18 on 2023-09-13 16:06_

While I'd prefer `-e`, not every command in the benchmark supports such a flag (or, at least, I haven't been able to find one for Black).

---

_@MichaReiser reviewed on 2023-09-13 16:22_

---

_Review comment by @MichaReiser on `scripts/benchmarks/run_formatter.sh`:57 on 2023-09-13 16:22_

We should document the caveats of our benchmarks and highlight any potential incorrectness. 

---

_Review comment by @charliermarsh on `scripts/benchmarks/run_formatter.sh`:57 on 2023-09-13 16:23_

I'll add some additional notes to the script. Is this a caveat though? What we're doing here seems correct.

---

_@charliermarsh reviewed on 2023-09-13 16:24_

---

_@MichaReiser reviewed on 2023-09-13 16:24_

---

_Review comment by @MichaReiser on `scripts/benchmarks/run_formatter.sh`:18 on 2023-09-13 16:24_

The example is to use `--setup` to compile your program. That means it only runs once for the whole benchmark. 

I guess the reset doesn't really matter because each formatter will format the file to their expected output as part of the warmup. 

---

_Review comment by @qarmin on `scripts/benchmarks/run_formatter.sh`:18 on 2023-09-13 16:40_

So maybe it's worth adding a new benchmark that would at least partially test the formatting performance(with a note that probably most of the test files were previously reformatted according to black guidelines) 

---

_@qarmin reviewed on 2023-09-13 16:40_

---

_Merged by @charliermarsh on 2023-09-13 17:15_

---

_Closed by @charliermarsh on 2023-09-13 17:15_

---

_Branch deleted on 2023-09-13 17:15_

---

_@charliermarsh reviewed on 2023-09-13 17:15_

---

_Review comment by @charliermarsh on `scripts/benchmarks/run_formatter.sh`:18 on 2023-09-13 17:15_

Yeah, perhaps over a repo like CPython that (AFAIK) isn't formatted.

---
