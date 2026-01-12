```yaml
number: 2865
title: "Use `std::path::absolute` for hyperlinks on Windows"
type: pull_request
state: closed
author: ltrzesniewski
labels:
  - rollup
assignees: []
base: master
head: abs-path-pr
created_at: 2024-08-03T19:56:39Z
updated_at: 2025-09-20T11:39:46Z
url: https://github.com/BurntSushi/ripgrep/pull/2865
synced_at: 2026-01-12T18:23:14Z
```

# Use `std::path::absolute` for hyperlinks on Windows

---

_@ltrzesniewski_

Now that `std::path::absolute` is stable, this PR uses it instead of canonicalization for hyperlinks on Windows, the main goal being to avoid unnecessary access to the file system. It bumps the MSRV to 1.79.

I ran a couple benchmarks: ripgrep is a bit faster in the usual case, but the gains can be much higher when generating hyperlinks for many files, such as when using `--files`.

## Usual case

Here's a simple usage example: looking for `hello` in the ripgrep source code:

```powershell
hyperfine --warmup 3 --export-markdown bench.md `
    -n Baseline 'rg hello C:\Dev\GitHub\ripgrep --no-config --hyperlink-format=vscode --color=always' `
    -n PR 'C:\Dev\GitHub\ripgrep\target\release\rg.exe hello C:\Dev\GitHub\ripgrep --no-config --hyperlink-format=vscode --color=always'
```

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `Baseline` | 26.1 ± 1.7 | 22.1 | 32.4 | 1.11 ± 0.09 |
| `PR` | 23.6 ± 1.2 | 21.4 | 28.3 | 1.00 |

## Using `--files`

And here's a more extreme example: using `--files` on a [big repo](https://github.com/dotnet/runtime):

```powershell
hyperfine --warmup 3 --export-markdown bench.md `
    -n Baseline 'rg --files C:\Dev\dotnet\runtime --no-config --hyperlink-format=vscode --color=always' `
    -n PR 'C:\Dev\GitHub\ripgrep\target\release\rg.exe --files C:\Dev\dotnet\runtime --no-config --hyperlink-format=vscode --color=always'
```

| Command | Mean [s] | Min [s] | Max [s] | Relative |
|:---|---:|---:|---:|---:|
| `Baseline` | 1.625 ± 0.004 | 1.619 | 1.633 | 10.73 ± 0.17 |
| `PR` | 0.151 ± 0.002 | 0.148 | 0.156 | 1.00 |

What's worth noting here is that this PR reduces the system time (certainly spent on accessing the file system for canonicalization):

```
Benchmark 1: Baseline
  Time (mean ± σ):      1.625 s ±  0.004 s    [User: 0.092 s, System: 0.511 s]
  Range (min … max):    1.619 s …  1.633 s    10 runs

Benchmark 2: PR
  Time (mean ± σ):     151.4 ms ±   2.3 ms    [User: 58.4 ms, System: 129.9 ms]
  Range (min … max):   147.8 ms … 156.0 ms    19 runs

Summary
  PR ran
   10.73 ± 0.17 times faster than Baseline
```

> [!NOTE]
> The baseline and PR binaries have been compiled with the same Rust v1.80.



---

_Label `rollup` added by @BurntSushi on 2025-07-26 14:50_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

_Branch deleted on 2025-09-20 11:39_

---
