```yaml
number: 19869
title: "Don't cache files with diagnostics"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/dont-cache-lint-diagnostics
created_at: 2025-08-11T17:04:04Z
updated_at: 2025-08-12T19:28:46Z
url: https://github.com/astral-sh/ruff/pull/19869
synced_at: 2026-01-10T17:52:17Z
```

# Don't cache files with diagnostics

---

_Pull request opened by @ntBre on 2025-08-11 17:04_

Summary
--

To take advantage of the new diagnostics, we need to update our caching model to include all of the information supported by `ruff_db`'s diagnostic type. Instead of trying to serialize all of this information, Micha suggested simply not caching files with diagnostics, like we already do for files with syntax errors. This PR is an attempt at that approach.

This has the added benefit of trimming down our `Rule` derives since this was the last place the `FromStr`/`strum_macros::EnumString` implementation was used, as well as the (de)serialization macros and `CacheKey`.

Test Plan
--

Existing tests, with their input updated not to include a diagnostic, plus a new test showing that files with lint diagnostics are not cached.

Benchmarks
--

In addition to tests, we wanted to check that this doesn't degrade performance too much. I posted part of this new analysis in https://github.com/astral-sh/ruff/issues/18198#issuecomment-3175048672, but I'll duplicate it here. In short, there's not much difference between `main` and this branch for projects with few diagnostics (`home-assistant`, `airflow`), as expected. The difference for projects with many diagnostics (`cpython`) is quite a bit bigger (~300 ms vs ~220 ms), but most projects that run ruff regularly are likely to have very few diagnostics, so this may not be a problem practically. 

I guess GitHub isn't really rendering this as I intended, but the extra separator line is meant to separate the benchmarks on `main` (above the line) from this branch (below the line).

| Command                                                       | Mean [ms] | Min [ms] | Max [ms] |
|:--------------------------------------------------------------|----------:|---------:|---------:|
| `ruff check cpython --no-cache --isolated --exit-zero`        |     322.0 |    317.5 |    326.2 |
| `ruff check cpython --isolated --exit-zero`                   |     217.3 |    209.8 |    237.9 |
| `ruff check home-assistant --no-cache --isolated --exit-zero` |     279.5 |    277.0 |    283.6 |
| `ruff check home-assistant --isolated --exit-zero`            |      37.2 |     35.7 |     40.6 |
| `ruff check airflow --no-cache --isolated --exit-zero`        |     133.1 |    130.4 |    146.4 |
| `ruff check airflow --isolated --exit-zero`                   |      34.7 |     32.9 |     41.6 |
|:--------------------------------------------------------------|----------:|---------:|---------:|
| `ruff check cpython --no-cache --isolated --exit-zero`        |     330.1 |    324.5 |    333.6 |
| `ruff check cpython --isolated --exit-zero`                   |     309.2 |    306.1 |    314.7 |
| `ruff check home-assistant --no-cache --isolated --exit-zero` |     288.6 |    279.4 |    302.3 |
| `ruff check home-assistant --isolated --exit-zero`            |      39.8 |     36.9 |     42.4 |
| `ruff check airflow --no-cache --isolated --exit-zero`        |     134.5 |    131.3 |    140.6 |
| `ruff check airflow --isolated --exit-zero`                   |      39.1 |     37.2 |     44.3 |

I had Claude adapt one of the [scripts](https://github.com/sharkdp/hyperfine/blob/master/scripts/plot_whisker.py) from the hyperfine repo to make this plot, so it's not quite perfect, but maybe it's still useful. The table is probably more reliable for close comparisons. I'll put more details about the benchmarks below for the sake of future reproducibility.

<img width="4472" height="2368" alt="image" src="https://github.com/user-attachments/assets/1c42d13e-818a-44e7-b34c-247340a936d7" />

<details><summary>Benchmark details</summary>
<p>

The versions of each project:
- CPython: 6322edd260e8cad4b09636e05ddfb794a96a0451, the 3.10 branch from the contributing docs
- `home-assistant`: 5585376b406f099fb29a970b160877b57e5efcb0
- `airflow`: 29a1cb0cfde9d99b1774571688ed86cb60123896

The last two are just the main branches at the time I cloned the repos.

I don't think our Ruff config should be applied since I used `--isolated`, but these are cloned into my copy of Ruff at `crates/ruff_linter/resources/test`, and I trimmed the `./target/release/` prefix from each of the commands, but these are builds of Ruff in release mode.

And here's the script with the `hyperfine` invocation:

```shell
#!/bin/bash

cargo build --release --bin ruff

# git clone --depth 1 https://github.com/home-assistant/core crates/ruff_linter/resources/test/home-assistant
# git clone --depth 1 https://github.com/apache/airflow crates/ruff_linter/resources/test/airflow

bin=./target/release/ruff
resources=./crates/ruff_linter/resources/test
cpython=$resources/cpython
home_assistant=$resources/home-assistant
airflow=$resources/airflow

base=${1:-bench}

hyperfine --warmup 10 --export-json $base.json --export-markdown $base.md \
		  "$bin check $cpython --no-cache --isolated --exit-zero" \
		  "$bin check $cpython --isolated --exit-zero" \
		  "$bin check $home_assistant --no-cache --isolated --exit-zero" \
		  "$bin check $home_assistant --isolated --exit-zero" \
		  "$bin check $airflow --no-cache --isolated --exit-zero" \
		  "$bin check $airflow --isolated --exit-zero"
```

I ran this once on `main` (`baseline` in the graph, top half of the table) and once on this branch (`nocache` and bottom of the table).

</p>
</details> 


---

_Comment by @github-actions[bot] on 2025-08-11 17:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-08-11 17:45_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-11 17:45_

---

_Comment by @MichaReiser on 2025-08-11 18:08_

I don't mind the regression for CPython too much. It has a vast amount of diagnostics which should be uncommon. What I'm more surprised by is the regression for homeassitant and airflow. Do they have any diagnostics, is it just noise, or do you know where the regression is comming from?

---

_@MichaReiser reviewed on 2025-08-11 18:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:392 on 2025-08-11 18:08_

What's preventing us from removing the Notebook index too? (in which case `FileCacheData` only needs to store whether a file had any violations)

---

_Comment by @ntBre on 2025-08-11 18:14_

> I don't mind the regression for CPython too much. It has a vast amount of diagnostics which should be uncommon. What I'm more surprised by is the regression for homeassitant and airflow. Do they have any diagnostics, is it just noise, or do you know where the regression is comming from?

I _think_ it's mostly noise (you can see CPython without a cache was 8 ms slower in the second run), but they both have some diagnostics too. `home-assistant` has 21 diagnostics, and `airflow` has 143, compared to 15,577 in `cpython`.

I'll take another look at the notebook index too.

---

_@ntBre reviewed on 2025-08-11 19:13_

---

_Review comment by @ntBre on `crates/ruff/src/cache.rs`:392 on 2025-08-11 19:13_

Ah of course, thanks. It kind of looked used, but obviously we won't actually look up a notebook in this map if the file has no diagnostics. I replaced `LintCacheData` with a `linted` field to mirror the `formatted` field.

I also updated `lint_path` (and `lint_stdin`) to avoid constructing a `NotebookIndex` that wouldn't be cached (e2bc85e56a). Otherwise the `same_results` cache tests were failing with differences in the `NotebookIndex` field. At first I just fixed this in the test, but it seemed like a small optimization in the real code anyway.

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:331 on 2025-08-12 07:28_

This feels a bit overkill, given that it always returns an empty `Diagnostics` or `None`. I think I'd change this to a `bool` which should also allow simplifying the call site, e.g. the following can be removed entirely:


```
// `FixMode::Generate` and `FixMode::Diff` rely on side-effects (writing to disk,
// and writing the diff to stdout, respectively). If a file has diagnostics, we
// need to avoid reading from and writing to the cache in these modes.
```

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:580 on 2025-08-12 07:31_

Nit: Maybe rename `errors` to `paths_with_diagnostics` to make it clear that this isn't storing the errors, but is a set of paths

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:329 on 2025-08-12 07:33_

```suggestion
        // We don't cache files with diagnostics.
```

I think this allows us to now remove `result.has_syntax_errors` all together. It's only used in the benchmarking code but we can just get that information from the parsed module instead.

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:340 on 2025-08-12 07:35_

I think this is fine, given that this is pre-existing code. But which code path sets `linted` to false if a file has diagnostics (how do we clear the state?)

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:347 on 2025-08-12 07:37_

This feels a bit subtle and it might not be evident for callers that `notebook_indexes` is empty if there are no diagnostics. We should document this constraint in `Diagnostics` or consider moving it into a constructor within `Diagnostics`. 

---

_@MichaReiser reviewed on 2025-08-12 07:38_

I think there's some more simplification that can be done here.

---

_@ntBre reviewed on 2025-08-12 14:05_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:329 on 2025-08-12 14:05_

I think the current assertion in the benchmark is a bit stronger than what we get from the parsed module because it also includes semantic syntax errors, but I'm not sure that was intentional. The comment on the assertion just says "parse errors," so it's probably fine to assert only on those.

---

_@ntBre reviewed on 2025-08-12 14:15_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:329 on 2025-08-12 14:15_

Do we need some kind of assertion on the benchmark result to make sure it doesn't get optimized out? If we assert on the parsed module, `result` becomes unused.

We can at least trim down the `LinterResult` methods to `has_no_syntax_errors` and `has_invalid_syntax`. Those are both only used in one place:

https://github.com/astral-sh/ruff/blob/ad28b80f967282aaaaaff59c722faf087125ffc1/crates/ruff_benchmark/benches/linter.rs#L91-L92

(after removing the `!` and using `has_no_syntax_errors`)

https://github.com/astral-sh/ruff/blob/ad28b80f967282aaaaaff59c722faf087125ffc1/crates/ruff_server/src/fix.rs#L78-L81

---

_@MichaReiser reviewed on 2025-08-12 14:23_

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:329 on 2025-08-12 14:23_

>  The comment on the assertion just says "parse errors," so it's probably fine to assert only on those.

Yeah, I think that's fine. The reason I added the assertion originally because we used to download the test files from Github and I once copied the wrong link (not the raw.github but the regular github.com link). The result was that we downloaded and benchmarked the HTML files instead of the Python files... whoops :) 

That's why this assertion is there

---

_@MichaReiser reviewed on 2025-08-12 14:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:329 on 2025-08-12 14:24_

I think you could return `LintResult` and criterion will automatically wrap it in a hint::black_box? (and also drop it)

---

_@ntBre reviewed on 2025-08-12 14:31_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:340 on 2025-08-12 14:31_

Ah, I think you're right. I guess this is different from `formatted` since that always gets set to `true` whether the file was reformatted or already formatted. I reworked this section a bit to set `linted` to `diagnostics.is_empty() && use_fixes` where `use_fixes` is the result of the `match` above this.

---

_@ntBre reviewed on 2025-08-12 14:40_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:347 on 2025-08-12 14:40_

We could also just do this filtering step in the test (or adjust the assertion not to check the notebook index), if you prefer. This didn't show any difference in the benchmarks, so it's not really helpful as an optimization, especially if it's confusing.

---

_@ntBre reviewed on 2025-08-12 15:25_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:347 on 2025-08-12 15:25_

I did this in 13d7cf9197 but happy to revert and document/update this code instead if you prefer it.

---

_@ntBre reviewed on 2025-08-12 15:26_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:347 on 2025-08-12 15:26_

I'm realizing now that we probably _also_ need the docs since it still affects `Diagnostics` created from the cache...

---

_@MichaReiser approved on 2025-08-12 16:02_

Nice. I think the perf regressions aren't unreasonable and it simplifies the implementation quiet a lot

---

_Label `internal` added by @ntBre on 2025-08-12 19:25_

---

_Comment by @ntBre on 2025-08-12 19:28_

`internal` might not be the right label here, but I don't really want it in the release notes. I certainly hope it doesn't cause any user-noticeable slowdown.

---

_Merged by @ntBre on 2025-08-12 19:28_

---

_Closed by @ntBre on 2025-08-12 19:28_

---

_Branch deleted on 2025-08-12 19:28_

---
