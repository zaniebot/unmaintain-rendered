```yaml
number: 6964
title: Run cargo update
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/update
created_at: 2023-08-29T03:11:57Z
updated_at: 2023-08-31T18:11:12Z
url: https://github.com/astral-sh/ruff/pull/6964
synced_at: 2026-01-12T15:55:22Z
```

# Run cargo update

---

_@charliermarsh_

_No description provided._

---

_Comment by @codspeed-hq[bot] on 2023-08-29 03:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/update)

### Merging #6964 will **degrade performances by 2.97%**

<sub>Comparing <code>charlie/update</code> (11e1d17) with <code>main</code> (68f605e)</sub>



### Summary

`❌ 1` regressions
`✅ 19` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/update)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/update` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 38.2 ms | 39.4 ms | -2.97% |


---

_Comment by @charliermarsh on 2023-08-29 03:27_

Huh, could that possibly be correct? I'll have to run these manually.

---

_Comment by @github-actions[bot] on 2023-08-29 03:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Converted to draft by @charliermarsh on 2023-08-29 04:16_

---

_Label `internal` added by @charliermarsh on 2023-08-29 04:16_

---

_Comment by @MichaReiser on 2023-08-29 05:41_

> Huh, could that possibly be correct? I'll have to run these manually.

The reports have been very accurate this far. Maybe force push again to trigger a new run. I otherwise recommend upgrading in smaller batches (eg only memchar)

---

_Comment by @MichaReiser on 2023-08-29 06:19_

I ran the benchmarks locally. All `linter/all-rules` tests are regressing, but `types.py` and `ctypeslib` by in a very bad way

```
linter/all-rules/numpy/globals.py
                        time:   [196.32 µs 197.08 µs 197.93 µs]
                        thrpt:  [14.907 MiB/s 14.972 MiB/s 15.030 MiB/s]
                 change:
                        time:   [+6.1293% +6.5304% +6.9420%] (p = 0.00 < 0.05)
                        thrpt:  [-6.4913% -6.1300% -5.7753%]
                        Performance has regressed.
Benchmarking linter/all-rules/pydantic/types.py: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 20.0s. You may wish to increase target time to 25.5s, enable flat sampling, or reduce sample count to 60.
linter/all-rules/pydantic/types.py
                        time:   [5.0083 ms 5.0100 ms 5.0121 ms]
                        thrpt:  [5.0884 MiB/s 5.0904 MiB/s 5.0922 MiB/s]
                 change:
                        time:   [+69.055% +69.334% +69.599%] (p = 0.00 < 0.05)
                        thrpt:  [-41.037% -40.945% -40.848%]
                        Performance has regressed.
Found 3 outliers among 100 measurements (3.00%)
  2 (2.00%) high mild
  1 (1.00%) high severe
linter/all-rules/numpy/ctypeslib.py
                        time:   [2.1310 ms 2.1330 ms 2.1365 ms]
                        thrpt:  [7.7938 MiB/s 7.8065 MiB/s 7.8139 MiB/s]
                 change:
                        time:   [+38.913% +39.053% +39.233%] (p = 0.00 < 0.05)
                        thrpt:  [-28.178% -28.085% -28.013%]
                        Performance has regressed.
Found 12 outliers among 100 measurements (12.00%)
  7 (7.00%) high mild
  5 (5.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [6.6676 ms 6.6977 ms 6.7216 ms]
                        thrpt:  [6.0525 MiB/s 6.0741 MiB/s 6.1015 MiB/s]
                 change:
                        time:   [+8.3097% +8.8593% +9.3198%] (p = 0.00 < 0.05)
                        thrpt:  [-8.5252% -8.1383% -7.6722%]
                        Performance has regressed.
```

---

_Comment by @MichaReiser on 2023-08-29 09:49_

It seems like libCST hits some bad case for aho-corasick

![image](https://github.com/astral-sh/ruff/assets/1203881/b675d02e-03aa-4427-9328-65b51542c79c)

[Profile](https://share.firefox.dev/3sxr40A)

For comparison, the `types` benchmark on main [Profile](https://share.firefox.dev/3EflJ0j)

![image](https://github.com/astral-sh/ruff/assets/1203881/0796cf2f-2a42-445a-a46c-3d221691329d)


where the Regex construction "only" accounts for 3% of the overall time

![image](https://github.com/astral-sh/ruff/assets/1203881/62eed3c1-9fcc-4338-96dc-85538863ba9c)


The easiest fix is to change libCST to either cache the Regex in `Whitespace::new`. Even better, use `memchr2` directly instead of building a Regex for this 

https://github.com/Instagram/LibCST/blob/c91655fbba7c27db05423673609f10203316b9c0/native/libcst/src/tokenizer/whitespace_parser.rs#L76-L80

CC: @BurntSushi you might be interested in the regression. It seems that building a `Regex` has become more expensive.


---

_Comment by @MichaReiser on 2023-08-29 09:58_

It seems this is "fixed" in the latest version of LibCST. 

https://github.com/Instagram/LibCST/pull/996


---

_Comment by @BurntSushi on 2023-08-29 13:28_

I don't think this is related to the `aho-corasick` upgrade specifically. From master:

```
$ cargo bench --bench linter linter/all-rules/numpy/globals.py
    Finished bench [optimized] target(s) in 0.08s
     Running benches/linter.rs (target/release/deps/linter-48c46d225533251a)
Gnuplot not found, using plotters backend
linter/all-rules/numpy/globals.py
                        time:   [131.16 µs 131.23 µs 131.29 µs]
                        thrpt:  [22.475 MiB/s 22.485 MiB/s 22.496 MiB/s]
                 change:
                        time:   [-0.9577% -0.8264% -0.7114%] (p = 0.00 < 0.05)
                        thrpt:  [+0.7165% +0.8333% +0.9670%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  2 (2.00%) low mild
  2 (2.00%) high mild
  2 (2.00%) high severe

$ cargo update -p aho-corasick@1.0.2
    Updating crates.io index
    Updating aho-corasick v1.0.2 -> v1.0.4

$ cargo bench --bench linter linter/all-rules/numpy/globals.py
    Finished bench [optimized] target(s) in 1m 50s
     Running benches/linter.rs (target/release/deps/linter-bed0760206629487)
Gnuplot not found, using plotters backend
linter/all-rules/numpy/globals.py
                        time:   [131.35 µs 131.40 µs 131.44 µs]
                        thrpt:  [22.449 MiB/s 22.456 MiB/s 22.464 MiB/s]
                 change:
                        time:   [+0.0104% +0.1040% +0.1955%] (p = 0.03 < 0.05)
                        thrpt:  [-0.1951% -0.1039% -0.0104%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  3 (3.00%) low mild
  2 (2.00%) high mild
  1 (1.00%) high severe
```

I also tried to check whether it might be a change in the `regex` (or related) crates:

```
$ cargo update -p regex -p regex-automata@0.3.0 -p regex-syntax@0.7.3 -p memchr
    Updating crates.io index
    Updating memchr v2.5.0 -> v2.6.0
    Updating regex v1.9.0 -> v1.9.4
    Updating regex-automata v0.3.0 -> v0.3.7
    Updating regex-syntax v0.7.3 -> v0.7.5

$ cargo bench --bench linter linter/all-rules/numpy/globals.py
    Finished bench [optimized] target(s) in 1m 51s
     Running benches/linter.rs (target/release/deps/linter-142801da0ac5da00)
Gnuplot not found, using plotters backend
linter/all-rules/numpy/globals.py
                        time:   [133.43 µs 133.47 µs 133.51 µs]
                        thrpt:  [22.101 MiB/s 22.107 MiB/s 22.114 MiB/s]
                 change:
                        time:   [+1.5378% +1.6424% +1.7609%] (p = 0.00 < 0.05)
                        thrpt:  [-1.7304% -1.6158% -1.5145%]
                        Performance has regressed.
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) low mild
  2 (2.00%) high severe
```

It looks like @MichaReiser found that it might be an issue in LibCST? Basically, one possible cause for this is that something changed not in regex compilation but in the frequency of regex compilation. If that happened, you'd expect to see the most expensive parts of regex compilation pop up in a profile.

---

_Comment by @MichaReiser on 2023-08-29 14:23_

> It looks like @MichaReiser found that it might be an issue in LibCST? Basically, one possible cause for this is that something changed not in regex compilation but in the frequency of regex compilation.

Hmm, interesting find. We didn't update LibCST as part of this PR, so it must be a shared dependency OR we now run the LibCST code paths more often but I then would expect the ecosystem check to fail (because we have more or fewer diagnostics). Anyway, thanks for chiming in and the best way to narrow it down is probably to upgrade the dependencies incrementally. 

---

_Comment by @BurntSushi on 2023-08-29 14:32_

@MichaReiser Aye. Note that there [were big changes to `AhoCorasick` construction in the 1.0.4 release](https://github.com/BurntSushi/aho-corasick/pull/121), so I would still treat it as a candidate to be suspicious of. (That was released a few weeks ago and I haven't yet received any reports of major regressions though.)

---

_Comment by @MichaReiser on 2023-08-29 14:34_

Woah nice memory improvements. Maybe we should do some memory profiling too ;)

@BurntSushi I took the same steps you mentioned above but with the `linter/all-rules/pydantic/types.py` benchmark, which seems most impacted, and I can reproduce (x86). (Note: I ran the command on main)

```
❯ cargo update -p aho-corasick@1.0.2
    Updating crates.io index
    Updating aho-corasick v1.0.2 -> v1.0.4

ruff on  main [$!] is 📦 v0.0.286 via 🐍 v3.11.3 via 🦀 v1.74.0-nightly took 2s 
❯ cargo bench --bench linter linter/all-rules/pydantic/types.py -- --baseline main
   Compiling aho-corasick v1.0.4
   Compiling regex-automata v0.3.0
   Compiling regex v1.9.0
   Compiling bstr v1.6.0
   Compiling criterion v0.5.1
   Compiling globset v0.4.10
   Compiling ruff_cache v0.0.0 (/home/micha/astral/ruff/crates/ruff_cache)
   Compiling ruff_benchmark v0.0.0 (/home/micha/astral/ruff/crates/ruff_benchmark)
   Compiling pep440_rs v0.3.11
   Compiling libcst v0.1.0 (https://github.com/Instagram/LibCST.git?rev=3cacca1a1029f05707e50703b49fe3dd860aa839#3cacca1a)
   Compiling pep508_rs v0.2.1
   Compiling pyproject-toml v0.6.1
   Compiling ruff v0.0.286 (/home/micha/astral/ruff/crates/ruff)
    Finished bench [optimized] target(s) in 1m 51s
     Running benches/linter.rs (target/release/deps/linter-e9767e92860b0320)
Gnuplot not found, using plotters backend
Benchmarking linter/all-rules/pydantic/types.py: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 20.0s. You may wish to increase target time to 25.0s, enable flat sampling, or reduce sample count to 60.
linter/all-rules/pydantic/types.py
                        time:   [4.9141 ms 4.9162 ms 4.9187 ms]
                        thrpt:  [5.1849 MiB/s 5.1875 MiB/s 5.1898 MiB/s]
                 change:
                        time:   [+59.590% +59.758% +59.925%] (p = 0.00 < 0.05)
                        thrpt:  [-37.471% -37.405% -37.339%]
```

But then, the main issue is the way LibCST uses/used a Regex (which ideally wouldn't be a regex in the first place) and updating / patching our branch is the way to go.

---

_Comment by @BurntSushi on 2023-08-29 18:12_

Higher level: yeah totally agree that code should just be using `memchr2` or something. It also just seems like something is compiling a regex over and over, so there might be a bug somewhere there too. But maybe not. Anyway, the fact that you saw the regression by just updating `aho-corasick` made me investigate this more to make sure there weren't any issues on my end. Indeed, there is a regression! The fix and details are at: https://github.com/BurntSushi/aho-corasick/pull/126

To confirm it's fixed, starting from master:

```
$ cargo bench --bench linter linter/all-rules/pydantic/types.py
    Finished bench [optimized] target(s) in 2m 03s
     Running benches/linter.rs (target/release/deps/linter-48c46d225533251a)
Gnuplot not found, using plotters backend
linter/all-rules/pydantic/types.py
                        time:   [2.3441 ms 2.3445 ms 2.3449 ms]
                        thrpt:  [10.876 MiB/s 10.878 MiB/s 10.880 MiB/s]
                 change:
                        time:   [-0.0075% +0.0318% +0.0705%] (p = 0.12 > 0.05)
                        thrpt:  [-0.0705% -0.0318% +0.0075%]
                        No change in performance detected.
Found 8 outliers among 100 measurements (8.00%)
  1 (1.00%) low severe
  2 (2.00%) low mild
  4 (4.00%) high mild
  1 (1.00%) high severe
```

Bump to `aho-corasick 1.0.4` to observe the regression:

```
$ cargo update -p aho-corasick@1.0.2 --precise 1.0.4
    Updating crates.io index
    Updating aho-corasick v1.0.2 -> v1.0.4
$ cargo bench --bench linter linter/all-rules/pydantic/types.py
    Finished bench [optimized] target(s) in 1m 50s
     Running benches/linter.rs (target/release/deps/linter-bed0760206629487)
Gnuplot not found, using plotters backend
Benchmarking linter/all-rules/pydantic/types.py: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 20.0s. You may wish to increase target time to 22.4s, enable flat sampling, or reduce sample count to 60.
linter/all-rules/pydantic/types.py
                        time:   [4.4151 ms 4.4161 ms 4.4174 ms]
                        thrpt:  [5.7733 MiB/s 5.7750 MiB/s 5.7764 MiB/s]
                 change:
                        time:   [+88.299% +88.363% +88.422%] (p = 0.00 < 0.05)
                        thrpt:  [-46.928% -46.911% -46.893%]
                        Performance has regressed.
Found 9 outliers among 100 measurements (9.00%)
  2 (2.00%) low mild
  1 (1.00%) high mild
  6 (6.00%) high severe
```

And now bump to recently released `aho-corasick 1.0.5` to observe that it is fixed:

```
$ cargo update -p aho-corasick@1.0.4 --precise 1.0.5
    Updating crates.io index
    Updating aho-corasick v1.0.4 -> v1.0.5
$ cargo bench --bench linter linter/all-rules/pydantic/types.py
    Finished bench [optimized] target(s) in 1m 50s
     Running benches/linter.rs (target/release/deps/linter-748d94d5650895b1)
Gnuplot not found, using plotters backend
linter/all-rules/pydantic/types.py
                        time:   [2.3391 ms 2.3395 ms 2.3399 ms]
                        thrpt:  [10.899 MiB/s 10.901 MiB/s 10.903 MiB/s]
                 change:
                        time:   [-47.036% -47.022% -47.009%] (p = 0.00 < 0.05)
                        thrpt:  [+88.711% +88.758% +88.808%]
                        Performance has improved.
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) low mild
  2 (2.00%) high mild
```

Thanks for catching this!

---

_Comment by @MichaReiser on 2023-08-30 07:55_

> To confirm it's fixed, starting from master:

Oh wow, thank you and I'm glad we were able to help catching the regression. But I'm sorry that I pushed some extra work on your shoulders. I should have created a minimized repro benchmark and open an issue on the aho-corasick repository instead. Thanks again.

Edit: [Upstream patch for LibCST](https://github.com/Instagram/LibCST/pull/1007) to remove the Regex use (there are more, but this replaces at least one) 

---

_Comment by @charliermarsh on 2023-08-31 17:41_

That `linter/default-rules` benchmark has been all over the place so it may just be noise here. The rest of the changes seem to be resolved after updating to latest `aho-corasick` (which, if I read the discussion correctly, is expected).

---

_Marked ready for review by @charliermarsh on 2023-08-31 17:41_

---

_@zanieb approved on 2023-08-31 18:06_

---

_Merged by @zanieb on 2023-08-31 18:11_

---

_Closed by @zanieb on 2023-08-31 18:11_

---

_Branch deleted on 2023-08-31 18:11_

---
