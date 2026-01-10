---
number: 1061
title: Store symlinked directories under their SHA
type: issue
state: open
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-01-23T14:31:14Z
updated_at: 2024-04-04T04:00:53Z
url: https://github.com/astral-sh/uv/issues/1061
synced_at: 2026-01-10T01:23:05Z
---

# Store symlinked directories under their SHA

---

_Issue opened by @charliermarsh on 2024-01-23 14:31_

Make the archive bucket content-addressed for artifacts that have them.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-23 14:31_

---

_Label `enhancement` added by @charliermarsh on 2024-01-23 14:31_

---

_Comment by @charliermarsh on 2024-01-25 03:16_

So I hooked up our download-and-unzip thing to also compute the SeaHash as it goes. It turns out that it doesn't affect the benchmarks at all:

```
❯ python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/trio.txt --min-runs 50 --benchmark install-cold
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     627.8 ms ±  40.3 ms    [User: 303.0 ms, System: 675.3 ms]
  Range (min … max):   557.7 ms … 799.8 ms    50 runs

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     627.5 ms ±  31.9 ms    [User: 300.3 ms, System: 677.3 ms]
  Range (min … max):   579.2 ms … 736.5 ms    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.00 ± 0.08 times faster than './target/release/puffin (install-cold)'
puffin on  charlie/sha:main [$!⇡] is 📦 v0.0.3 via 🐍 v3.12.0 (.venv) via 🦀 v1.75.0 took 2m7s
❯ python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/black.txt --min-runs 50 --benchmark install-cold
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     218.3 ms ±  37.9 ms    [User: 61.1 ms, System: 78.3 ms]
  Range (min … max):   193.4 ms … 472.6 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     222.8 ms ±  28.0 ms    [User: 63.2 ms, System: 82.4 ms]
  Range (min … max):   198.6 ms … 332.3 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  './target/release/puffin (install-cold)' ran
    1.02 ± 0.22 times faster than './target/release/main (install-cold)'
```

Which is good (it's dominated by IO).

I'll try SHA256 too.

---

_Comment by @charliermarsh on 2024-01-25 04:13_

With SHA256, there _is_ a penalty:

```text
❯ python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/trio.txt --min-runs 50 --benchmark install-cold && python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/black.txt --min-runs 50 --benchmark install-cold && python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/pdm_2193.txt --min-runs 50 --benchmark install-cold
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     686.9 ms ± 151.0 ms    [User: 359.8 ms, System: 678.2 ms]
  Range (min … max):   620.8 ms … 1542.2 ms    50 runs

  Warning: The first benchmarking run for this command was significantly slower than the rest (1.250 s). This could be caused by (filesystem) caches that were not filled until after the first run. You should consider using the '--warmup' option to fill those caches before the actual benchmark. Alternatively, use the '--prepare' option to clear the caches before each timing run.

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     639.7 ms ±  32.1 ms    [User: 300.4 ms, System: 687.3 ms]
  Range (min … max):   583.1 ms … 735.8 ms    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.07 ± 0.24 times faster than './target/release/puffin (install-cold)'
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     214.3 ms ±  20.4 ms    [User: 64.8 ms, System: 77.9 ms]
  Range (min … max):   190.3 ms … 332.8 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     211.1 ms ±   9.3 ms    [User: 60.3 ms, System: 78.7 ms]
  Range (min … max):   191.2 ms … 238.6 ms    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.02 ± 0.11 times faster than './target/release/puffin (install-cold)'
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     880.6 ms ±  76.2 ms    [User: 639.7 ms, System: 645.6 ms]
  Range (min … max):   816.0 ms … 1184.7 ms    50 runs

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     856.2 ms ±  46.7 ms    [User: 560.9 ms, System: 644.1 ms]
  Range (min … max):   789.3 ms … 993.6 ms    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.03 ± 0.11 times faster than './target/release/puffin (install-cold)'
```

---

_Comment by @charliermarsh on 2024-01-25 06:07_

The gap is smaller with Blake3:

```text
❯ python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/trio.txt --min-runs 50 --benchmark install-cold && python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/black.txt --min-runs 50 --benchmark install-cold && python -m scripts.bench \
 --puffin-path ./target/release/puffin        --puffin-path ./target/release/main \
        scripts/requirements/compiled/pdm_2193.txt --min-runs 50 --benchmark install-cold
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     641.6 ms ±  29.1 ms    [User: 319.2 ms, System: 690.2 ms]
  Range (min … max):   583.3 ms … 731.3 ms    50 runs

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     622.3 ms ±  22.5 ms    [User: 298.6 ms, System: 697.5 ms]
  Range (min … max):   575.5 ms … 674.9 ms    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.03 ± 0.06 times faster than './target/release/puffin (install-cold)'
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     212.1 ms ±  14.9 ms    [User: 61.2 ms, System: 75.2 ms]
  Range (min … max):   194.9 ms … 275.1 ms    50 runs

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     211.2 ms ±  12.6 ms    [User: 59.5 ms, System: 74.6 ms]
  Range (min … max):   194.7 ms … 258.4 ms    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.00 ± 0.09 times faster than './target/release/puffin (install-cold)'
Benchmark 1: ./target/release/puffin (install-cold)
  Time (mean ± σ):     852.7 ms ±  52.2 ms    [User: 582.8 ms, System: 630.7 ms]
  Range (min … max):   775.0 ms … 997.8 ms    50 runs

Benchmark 2: ./target/release/main (install-cold)
  Time (mean ± σ):     844.4 ms ±  55.5 ms    [User: 558.2 ms, System: 632.6 ms]
  Range (min … max):   763.2 ms … 1060.5 ms    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.01 ± 0.09 times faster than './target/release/puffin (install-cold)'
```

---

_Comment by @charliermarsh on 2024-01-25 14:59_

I ran the "hash this entire wheel on-disk" benchmark on my Windows machine:

```
Benchmarking blake3_mmap_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collecting 100 samples iblake3_mmap_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [7.5332 ms 7.6284 ms 7.7240 ms]
Found 2 outliers among 100 measurements (2.00%)
  2 (2.00%) high mild

Benchmarking blake3_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collecting 100 samples in estblake3_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [9.1883 ms 9.2754 ms 9.3824 ms]
Found 4 outliers among 100 measurements (4.00%)
  3 (3.00%) high mild
  1 (1.00%) high severe

Benchmarking xxhash_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collecting 100 samples in estxxhash_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [6.6886 ms 6.7697 ms 6.8605 ms]
Found 7 outliers among 100 measurements (7.00%)
  2 (2.00%) high mild
  5 (5.00%) high severe

Benchmarking seahash_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collecting 100 samples in esseahash_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [7.0987 ms 7.1567 ms 7.2178 ms]
Found 3 outliers among 100 measurements (3.00%)
  3 (3.00%) high mild

Benchmarking metrohash_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collecting 100 samples in metrohash_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [6.7346 ms 6.7968 ms 6.8621 ms]
Found 2 outliers among 100 measurements (2.00%)
  1 (1.00%) high mild
  1 (1.00%) high severe

Benchmarking sha256_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collecting 100 samples in estsha256_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [13.016 ms 13.077 ms 13.137 ms]
Found 2 outliers among 100 measurements (2.00%)
  2 (2.00%) low mild

Benchmarking crc32_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collecting 100 samples in esticrc32_wheel/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [6.6738 ms 6.7619 ms 6.8561 ms]
Found 10 outliers among 100 measurements (10.00%)
  3 (3.00%) high mild
  7 (7.00%) high severe
```

---

_Comment by @charliermarsh on 2024-01-26 16:31_

Running with 40mbps throttled connection:

```
unzip_blake3/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [3.2989 s 3.3518 s 3.4520 s]
                        change: [+500.79% +514.89% +538.00%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 9 outliers among 100 measurements (9.00%)
  4 (4.00%) high mild
  5 (5.00%) high severe

Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 333.3s, or reduce sample count to 10.
Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Co
Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: An
unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [3.2947 s 3.2981 s 3.3024 s]
                        change: [+499.11% +505.00% +510.54%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 11 outliers among 100 measurements (11.00%)
  7 (7.00%) high mild
  4 (4.00%) high severe

Benchmarking unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 329.6s, or reduce sample count to 10.
Benchmarking unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collectin
unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [3.2965 s 3.3005 s 3.3063 s]
                        change: [+494.88% +504.25% +512.35%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 8 outliers among 100 measurements (8.00%)
  5 (5.00%) high mild
  3 (3.00%) high severe
```

---

_Comment by @charliermarsh on 2024-01-26 16:45_

At 250 mbps:

```
unzip_blake3/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [699.88 ms 708.84 ms 720.46 ms]
                        change: [-2.3002% -0.4638% +1.4485%] (p = 0.64 > 0.05)
                        No change in performance detected.
Found 4 outliers among 100 measurements (4.00%)
  2 (2.00%) high mild
  2 (2.00%) high severe

Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 70.5s, or reduce sample count to 10.
Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Co
Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: An
unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [694.00 ms 702.67 ms 713.61 ms]
                        change: [-7.5463% -5.2398% -3.0022%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 4 outliers among 100 measurements (4.00%)
  2 (2.00%) high mild
  2 (2.00%) high severe

Benchmarking unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 70.8s, or reduce sample count to 10.
Benchmarking unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collectin
unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [703.23 ms 709.96 ms 717.26 ms]
                        change: [-6.1191% -3.8563% -1.7464%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 6 outliers among 100 measurements (6.00%)
  5 (5.00%) high mild
  1 (1.00%) high severe
```

---

_Comment by @charliermarsh on 2024-01-26 17:06_

At 500 mbps I start to see an effect:

```text
unzip_blake3/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [523.80 ms 532.78 ms 543.63 ms]
                        change: [-2.7715% +0.1003% +2.7441%] (p = 0.95 > 0.05)
                        No change in performance detected.
Found 7 outliers among 100 measurements (7.00%)
  4 (4.00%) high mild
  3 (3.00%) high severe

Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 51.7s, or reduce sample count to 10.
Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Co
Benchmarking unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: An
unzip_sha256/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [554.54 ms 600.65 ms 654.34 ms]
                        change: [+4.7013% +13.569% +24.015%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 16 outliers among 100 measurements (16.00%)
  7 (7.00%) high mild
  9 (9.00%) high severe

Benchmarking unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 51.7s, or reduce sample count to 10.
Benchmarking unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl: Collectin
unzip/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [512.06 ms 517.42 ms 523.46 ms]
                        change: [+0.3987% +1.9507% +3.5672%] (p = 0.02 < 0.05)
                        Change within noise threshold.
Found 8 outliers among 100 measurements (8.00%)
  7 (7.00%) high mild
```

---

_Unassigned @charliermarsh by @charliermarsh on 2024-04-04 04:00_

---

_Referenced in [astral-sh/uv#16816](../../astral-sh/uv/pulls/16816.md) on 2025-11-22 03:05_

---
