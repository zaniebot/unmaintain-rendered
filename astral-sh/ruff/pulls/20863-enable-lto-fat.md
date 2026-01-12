```yaml
number: 20863
title: Enable lto=fat
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/fat-lto
created_at: 2025-10-14T12:59:01Z
updated_at: 2025-10-27T14:02:52Z
url: https://github.com/astral-sh/ruff/pull/20863
synced_at: 2026-01-12T15:57:11Z
```

# Enable lto=fat

---

_@MichaReiser_

## Summary

This PR switches from thin to fat LTO optimization.

The main motivation is that using lto=fat fixes the binary size increase that we've seen after switching to inventory for [ingredient registration](https://github.com/astral-sh/ruff/issues/20845#issuecomment-3401604238). The regression is mainly due to symbols not being removed when using lto=thin, but they're successfully removed when using lto=fat (from 19MB to 17MB).

Using lto=fat also results in a significant performance improvement, which I think is worth it on its own.

Unfortunately, using lto=flat does have the downside that release builds take significantly longer. One of the main motivations for using `lto=thin` when we switched from `fat` to `thin` in https://github.com/astral-sh/ruff/pull/9031 was to improve performance. To mitigate this, I changed the `--profiling` profile to only use `lto=thin`, similar to what we do in uv (where we disable lto entirely). This PR is also likely to make the mypy primer and benchmark jobs slower (or when running mypy primer locally), because of a significant increase in compile time.

This setup now mirrors uv's (with the exception that `profiling` use `lto=fat`).

I do feel a bit bad about making this change while @BurntSushi is out, because I know he feels the most vocal about this.

Clean build timing:

* fat: 2m 02s
* thin: 1m 00s

Fixes https://github.com/astral-sh/ruff/issues/20845

Relevant discussions:

* https://github.com/astral-sh/uv/pull/5904
* https://github.com/astral-sh/ruff/pull/9031
* https://github.com/astral-sh/uv/pull/5955

---

_Comment by @codspeed-hq[bot] on 2025-10-14 13:16_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto)

### Merging #20863 will **degrade performances by 5.28%**

<sub>Comparing <code>micha/fat-lto</code> (6e2b809) with <code>main</code> (abf685b)</sub>



### Summary

`⚡ 25` improvements  
`❌ 1 (👁 1)` regression  
`✅ 25` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Instrumentation | [`` linter/all-rules[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation) | 17.7 ms | 16.2 ms | +9.37% |
| ⚡ | Instrumentation | [`` linter/all-rules[numpy/ctypeslib.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bnumpy%2Fctypeslib.py%5D&runnerMode=Instrumentation) | 4.2 ms | 3.9 ms | +7.89% |
| ⚡ | Instrumentation | [`` linter/all-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation) | 712.7 µs | 648.1 µs | +9.97% |
| ⚡ | Instrumentation | [`` linter/all-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation) | 8.1 ms | 7.5 ms | +8.33% |
| ⚡ | Instrumentation | [`` linter/all-rules[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation) | 1.8 ms | 1.7 ms | +10.14% |
| 👁 | Instrumentation | [`` linter/default-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Adefault_rules%3A%3Abenchmark_default_rules%3A%3Alinter%2Fdefault-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation) | 194.4 µs | 205.3 µs | -5.28% |
| ⚡ | Instrumentation | [`` linter/all-with-preview-rules[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation) | 21.3 ms | 19.5 ms | +9.07% |
| ⚡ | Instrumentation | [`` linter/all-with-preview-rules[numpy/ctypeslib.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fctypeslib.py%5D&runnerMode=Instrumentation) | 4.9 ms | 4.6 ms | +7.88% |
| ⚡ | Instrumentation | [`` linter/all-with-preview-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation) | 805 µs | 731.3 µs | +10.09% |
| ⚡ | Instrumentation | [`` linter/all-with-preview-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation) | 9.6 ms | 8.9 ms | +7.92% |
| ⚡ | Instrumentation | [`` linter/all-with-preview-rules[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation) | 2.1 ms | 1.9 ms | +10.15% |
| ⚡ | Instrumentation | [`` ty_check_file[cold] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_cold%3A%3Aty_check_file%5Bcold%5D&runnerMode=Instrumentation) | 122.8 ms | 115.2 ms | +6.6% |
| ⚡ | Instrumentation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation) | 5.2 ms | 4.4 ms | +17.85% |
| ⚡ | Instrumentation | [`` ty_micro[many_enum_members] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members%3A%3Aty_micro%5Bmany_enum_members%5D&runnerMode=Instrumentation) | 92.6 ms | 88.1 ms | +5.03% |
| ⚡ | Instrumentation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation) | 898.9 ms | 828.5 ms | +8.5% |
| ⚡ | Instrumentation | [`` attrs ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aattrs%3A%3Aproject%3A%3Aattrs&runnerMode=Instrumentation) | 412.6 ms | 382.7 ms | +7.8% |
| ⚡ | Instrumentation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation) | 194.9 ms | 182.3 ms | +6.91% |
| ⚡ | Instrumentation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation) | 935.8 ms | 853 ms | +9.7% |
| ⚡ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime) | 42.9 s | 38.9 s | +10.33% |
| ⚡ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime) | 12.3 s | 11.1 s | +11.07% |
| ... | ... | ... | ... | ... | ... |

<br/>

> :information_source: _Only the first 20 benchmarks are displayed. [Go to the app to view all benchmarks](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffat-lto)._


---

_Comment by @github-actions[bot] on 2025-10-14 13:20_

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

_Label `release` added by @MichaReiser on 2025-10-14 14:27_

---

_Closed by @MichaReiser on 2025-10-14 14:42_

---

_Reopened by @MichaReiser on 2025-10-14 14:42_

---

_Marked ready for review by @MichaReiser on 2025-10-14 14:49_

---

_Comment by @MichaReiser on 2025-10-14 14:49_

I'm not sure why that one instrumented benchmark regresses. But a 10% improvement on our walltime benchmarks is very convincing

---

_Review comment by @ntBre on `Cargo.toml`:311 on 2025-10-14 15:32_

```suggestion
# that we aren't shipping, but in order to make those two the same, we'd
```

---

_@ntBre approved on 2025-10-14 15:32_

Makes sense to me!

---

_Renamed from "Enable fat LTO" to "Enable lto=fat" by @MichaReiser on 2025-10-14 16:47_

---

_Merged by @MichaReiser on 2025-10-15 06:59_

---

_Closed by @MichaReiser on 2025-10-15 06:59_

---

_Branch deleted on 2025-10-15 07:00_

---

_Comment by @BurntSushi on 2025-10-27 14:02_

Yeah I think I've slowly become okay with this sort of change. I even recently did the same for ripgrep. I still don't like that we have different compilation settings for profiling versus release, but I haven't been bitten too hard by it yet.

---
