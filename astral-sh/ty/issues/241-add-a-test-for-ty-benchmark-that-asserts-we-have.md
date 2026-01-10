```yaml
number: 241
title: "add a test for `ty_benchmark` that asserts we have roughly the number of expected errors"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - testing
assignees: []
created_at: 2024-09-03T13:43:29Z
updated_at: 2025-11-25T07:58:35Z
url: https://github.com/astral-sh/ty/issues/241
synced_at: 2026-01-10T01:58:59Z
```

# add a test for `ty_benchmark` that asserts we have roughly the number of expected errors

---

_Issue opened by @AlexWaygood on 2024-09-03 13:43_

It seems like uv had some change in 0.4 that meant our red-knot benchmark silently stopped working, in that the dependencies for our benchmark projects weren't being installed into the right virtual environment: https://github.com/astral-sh/ruff/pull/13228#issuecomment-2326322306. (Probably a skill issue in our usage of uv rather than a breaking change from uv.)

It would be great if we could have some kind of test in CI that checks that the tools we're running emit roughly the number of errors we expect, so that it doesn't silently become invalid in the future.

Relatedly: mypy emits one error when checking black via our benchmark infrastructure, and we're not sure why that is. Black is compiled with mypyc and they run mypy in CI, so there should probably be 0 errors there! We're probably invoking mypy slightly wrong somehow? If we could get a clean run of mypy on black in our benchmark, that would make it significantly easier to add this kind of test.


---

_Label `help wanted` added by @AlexWaygood on 2024-09-03 13:43_

---

_Label `testing` added by @AlexWaygood on 2024-09-03 13:43_

---

_Comment by @AlexWaygood on 2024-09-03 20:08_

https://github.com/astral-sh/ruff/pull/13235 fixes the issue where mypy was emitting an error on black when invoked from the benchmark script

---

_Renamed from "Add a test for `knot_benchmark` that asserts we have roughly the number of expected errors" to "[red-knot] add a test for `knot_benchmark` that asserts we have roughly the number of expected errors" by @carljm on 2025-03-27 19:00_

---

_Comment by @MatthewMckee4 on 2025-03-29 15:00_

Is this what `mypy_primer` does?

---

_Comment by @AlexWaygood on 2025-04-01 12:34_

> Is this what `mypy_primer` does?

no, `mypy_primer` is a separate tool to [`knot_benchmark`](https://github.com/astral-sh/ruff/tree/main/scripts/knot_benchmark). This issue is about adding a regression test for [`knot_benchmark`](https://github.com/astral-sh/ruff/tree/main/scripts/knot_benchmark) that ensures that it doesn't unexpectedly stop working. 

---

_Renamed from "[red-knot] add a test for `knot_benchmark` that asserts we have roughly the number of expected errors" to "add a test for `knot_benchmark` that asserts we have roughly the number of expected errors" by @MichaReiser on 2025-05-07 15:27_

---

_Comment by @alex-gregory-ds on 2025-10-19 20:31_

Is there still interest in this? If so I would be interested in contributing.

Also, has the `knot_benchmark` been renamed to the `ty_benchmark`? Looks like it was changed in this commit: [b51c4f82](https://github.com/astral-sh/ruff/tree/b51c4f82ea6b6e40dcf3622dd009d463bb7e2247).

---

_Renamed from "add a test for `knot_benchmark` that asserts we have roughly the number of expected errors" to "add a test for `ty_benchmark` that asserts we have roughly the number of expected errors" by @MichaReiser on 2025-10-20 06:21_

---

_Comment by @AlexWaygood on 2025-10-23 13:18_

Hey @alex-gregory-ds -- yes, I still think it would probably be useful to have some kind of CI check to enforce this! Feel free to have at it :-)

---

_Assigned to @alex-gregory-ds by @AlexWaygood on 2025-10-23 13:18_

---

_Comment by @alex-gregory-ds on 2025-10-26 09:43_

Great to hear. I have a few ideas for implementing this but it would be good to get your advice.

We could capture the output for each hyperfine run and count the number of errors and warnings in stdout. The lines we'd have to change are [here](https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/scripts/ty_benchmark/src/benchmark/__init__.py#L73). To capture the `ty` outputs from hypervine we would have to add the `--show-output` argument which according to the hypervine documentation should only be used for debugging. This may affect the benchmark times. Here is the relevant hypervine documentation:

```
--show-output
    Print the stdout and stderr of the benchmark instead of suppressing
    it. This will increase the time it takes for benchmarks to run, so it
    should only be used for debugging purposes or when trying to benchmark
    output speed.
```

To ensure the benchmark times are unaffected by counting the number of warnings/errors in stdout, should we run an extra run at the end of benchmarking that captures the standard output from which we can then compute the number of warnings and errors?

Finally, I suspect that counting the number of instances of the words Error/Warning may not be the best way to count the number of errors/warnings. But, I cannot find a `ty` option that returns the number of errors/warnings. Should we consider adding a command line option to `ty` that returns the number of errors/warnings that were raised?

---

_Comment by @MichaReiser on 2025-10-27 06:52_

> To ensure the benchmark times are unaffected by counting the number of warnings/errors in stdout, should we run an extra run at the end of benchmarking that captures the standard output from which we can then compute the number of warnings and errors?

I agree, we should avoid any unnecessary work when running hyperfine for a fair comparison. I suggest a separate run before running hyperfine as it doesn't make much sense to perform any measurement if it is incorrect anyway.

> Finally, I suspect that counting the number of instances of the words Error/Warning may not be the best way to count the number of errors/warnings. But, I cannot find a ty option that returns the number of errors/warnings. Should we consider adding a command line option to ty that returns the number of errors/warnings that were raised?

I'm leaning towards using `--output-format=concise` and simply counting the lines. We should also check that ty exits with 0 and use `--exit-zero` (that's even something that hyperfine can do for us)

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 15:48_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @MichaReiser on 2025-11-20 09:42_

I'm inclined to close this. https://github.com/astral-sh/ruff/pull/21536 adds some improvements to catch non-type-checking errors. E.g. if a CLI option no longer works. 

It's also important that we update the benchmarks periodically to use newer mypy, pyrefly, ty, and pyright versions and update the benchmarked projects. Whenever we do that, we'll have to review every single project manually to see if there are new missing dependencies, whether the Python versions are still correct, whether the project layout has changed, etc. This process is entirely manual (I use `uv run benchmark --tool ty -v --project <each_project>` for every project and tool) and I added documentation mentioning the expected diagnostics when the tools were last updated.

While a more automated way of asserting the diagnostics would certainly be nice, I'm not sure it's worth the complexity, especially since updating still requires manual verification.

---

_Comment by @AlexWaygood on 2025-11-20 10:02_

I'm fine with that. This definitely turned out to be more complex than I anticipated.

Thanks for the contribution anyway, @alex-gregory-ds — really appreciate it! I'm sorry I led you astray here :-(

---

_Comment by @MichaReiser on 2025-11-20 10:41_

> Thanks for the contribution anyway, @alex-gregory-ds — really appreciate it! I'm sorry I led you astray here :-(

I still think it would be cool to snapshot the diagnostics somehow 😆 Maybe by having a different command mode `uv run benchmark --snapshot`? This might not even be that hard because all it requires is to snapshot the outputs for each benchmark and tool. The files might just get very very large

---

_Closed by @MichaReiser on 2025-11-25 07:58_

---
