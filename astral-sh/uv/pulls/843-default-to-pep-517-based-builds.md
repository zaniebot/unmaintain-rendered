```yaml
number: 843
title: Default to PEP 517-based builds
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/legacy-setup
created_at: 2024-01-09T01:23:52Z
updated_at: 2024-01-11T14:23:46Z
url: https://github.com/astral-sh/uv/pull/843
synced_at: 2026-01-10T15:39:02Z
```

# Default to PEP 517-based builds

---

_Pull request opened by @charliermarsh on 2024-01-09 01:23_

## Summary

Our current setup uses the legacy `setup.py`-based builds if a `pyproject.toml` file isn't present. This matches pip's behavior. However, `pypa/build` uses PEP 517-based builds in such cases, and it looks like pip plans to make that the default (https://github.com/pypa/pip/issues/9175), with the limiting factor being performance issues related to isolated builds.

This is now the default behavior, but the `--legacy-setup-py` flag allows users to opt-in to using `setup.py` directly for distributions that lack a `pyproject.toml`.

---

_Comment by @charliermarsh on 2024-01-09 01:59_

So it looks like resolution is meaningfully faster:

```
Benchmark 1: ./target/release/main (resolve-cold)
  Time (mean ± σ):     566.4 ms ±  34.8 ms    [User: 213.1 ms, System: 613.3 ms]
  Range (min … max):   528.0 ms … 715.2 ms    50 runs

Benchmark 2: ./target/release/puffin (resolve-cold)
  Time (mean ± σ):     518.0 ms ±  31.1 ms    [User: 217.3 ms, System: 293.8 ms]
  Range (min … max):   482.7 ms … 630.0 ms    50 runs
```

But installation is meaningfully slower:

```
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):     683.2 ms ±  23.5 ms    [User: 276.4 ms, System: 678.9 ms]
  Range (min … max):   646.0 ms … 765.9 ms    50 runs

Benchmark 2: ./target/release/puffin (install-cold)
  Time (mean ± σ):     712.1 ms ±  28.2 ms    [User: 328.3 ms, System: 358.7 ms]
  Range (min … max):   673.3 ms … 814.9 ms    50 runs
```

---

_Comment by @charliermarsh on 2024-01-09 01:59_

This using `django-ipware==3.0.0` somewhat arbitrarily, as a simple package that uses legacy `setup.py`.

---

_Comment by @charliermarsh on 2024-01-09 02:19_

Okay, skipping `get_requires_for_build_wheel` in this case makes both much faster:

```
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):     692.8 ms ±  52.3 ms    [User: 280.0 ms, System: 644.6 ms]
  Range (min … max):   633.7 ms … 970.7 ms    40 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 2: ./target/release/puffin (install-cold)
  Time (mean ± σ):     604.4 ms ±  29.5 ms    [User: 247.7 ms, System: 337.3 ms]
  Range (min … max):   555.3 ms … 677.1 ms    40 runs

Summary
  './target/release/puffin (install-cold)' ran
    1.15 ± 0.10 times faster than './target/release/main (install-cold)'
Benchmark 1: ./target/release/main (resolve-cold)
  Time (mean ± σ):     589.5 ms ±  72.2 ms    [User: 215.3 ms, System: 610.5 ms]
  Range (min … max):   525.1 ms … 870.2 ms    40 runs

Benchmark 2: ./target/release/puffin (resolve-cold)
  Time (mean ± σ):     461.1 ms ±  35.0 ms    [User: 164.1 ms, System: 275.6 ms]
  Range (min … max):   426.1 ms … 564.6 ms    40 runs
```

I think that's fine to do, honestly?

---

_Review requested from @konstin by @charliermarsh on 2024-01-09 02:19_

---

_Marked ready for review by @charliermarsh on 2024-01-09 02:20_

---

_@konstin approved on 2024-01-09 09:54_

I'd love it if we could get away with this!

The perf improvement is impressive!

---

_Comment by @charliermarsh on 2024-01-09 13:56_

@konstin - Two questions: (1) should we add a flag to disable this? I kind of think we should. And (2) why do you think cold-install got faster?

---

_Comment by @konstin on 2024-01-09 13:57_

> should we add a flag to disable this? I kind of think we should.

Agreed

> why do you think cold-install got faster?

Good question, should i look into it?

---

_Comment by @charliermarsh on 2024-01-09 13:59_

Sounds good, I'll add the flag.

Yeah, if you can, that would be great. I was comparing `main` to this branch via `bench` script on `django-ipware==3.0.0`.

---

_@charliermarsh reviewed on 2024-01-09 14:02_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:362 on 2024-01-09 14:02_

I realized... we can actually skip this if we _know_ the build backend doesn't implement it? Like `poetry-core` always returns `[]`.

---

_Label `enhancement` added by @charliermarsh on 2024-01-10 01:20_

---

_Merged by @charliermarsh on 2024-01-10 01:27_

---

_Closed by @charliermarsh on 2024-01-10 01:27_

---

_Branch deleted on 2024-01-10 01:27_

---

_Comment by @konstin on 2024-01-11 14:13_

What requirements and python version did you use for the benchmarking numbers? I get no difference when building `
django-ipware-3.0.0.tar.gz` by hand:

```
$ hyperfine --warmup 1 --prepare "rm -rf a dist" "python setup.py bdist_wheel" "python -c \"from setuptools.build_meta import __legacy__; __legacy__.build_wheel('a')\""
Benchmark 1: python setup.py bdist_wheel
  Time (mean ± σ):      88.8 ms ±   1.1 ms    [User: 79.8 ms, System: 9.2 ms]
  Range (min … max):    87.0 ms …  92.5 ms    32 runs
 
Benchmark 2: python -c "from setuptools.build_meta import __legacy__; __legacy__.build_wheel('a')"
  Time (mean ± σ):      89.8 ms ±   2.1 ms    [User: 80.6 ms, System: 9.1 ms]
  Range (min … max):    87.7 ms …  97.6 ms    32 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.
 
Summary
  python setup.py bdist_wheel ran
    1.01 ± 0.03 times faster than python -c "from setuptools.build_meta import __legacy__; __legacy__.build_wheel('a')"
```

---

_Comment by @charliermarsh on 2024-01-11 14:23_

I suspect that there is no difference when building by hand (there shouldn't be, right? the PEP 517 interface is just a thin wrapper). Instead I suspect the difference is from something we're doing in `puffin-build`.

---
