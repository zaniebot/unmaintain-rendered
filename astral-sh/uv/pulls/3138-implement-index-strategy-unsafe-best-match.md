```yaml
number: 3138
title: "Implement `--index-strategy unsafe-best-match`"
type: pull_request
state: merged
author: yorickvP
labels:
  - compatibility
assignees: []
merged: true
base: main
head: yorickvp/unsafe-any-match
created_at: 2024-04-19T10:24:07Z
updated_at: 2024-07-22T14:08:56Z
url: https://github.com/astral-sh/uv/pull/3138
synced_at: 2026-01-12T16:05:27Z
```

# Implement `--index-strategy unsafe-best-match`

---

_@yorickvP_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This index strategy resolves every package to the latest possible version across indexes. If a version is in multiple indexes, the first available index is selected.

Implements #3137 

This closely matches pip.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Good question. I'm hesitant to use my certifi example here, since that would inevitably break when torch removes this package. Please comment!
<!-- How was it tested? -->


---

_Comment by @zanieb on 2024-04-19 21:09_

I don't mind adding this behavior, but we'll need to reach consensus as a team. Thanks for contributing!

---

_Comment by @charliermarsh on 2024-04-20 01:32_

I suppose I'm open to the behavior itself, but I think the current behavior will be quadratic? Since for every version we select, we're now iterating over all versions to create the merged version map.

---

_Label `compatibility` added by @charliermarsh on 2024-04-20 01:33_

---

_Comment by @sergeykolosov on 2024-04-20 17:57_

In use-cases of my concern, this one is the last missing piece to be able to switch to uv.

There's ≈10 internal indexes with pretty much every possible package contained in more than one of those, versions randomly scattered across indexes (so currently the `--index-strategy unsafe-any-match` is being used).

So for a requirement of `some-internal-package>=0.1.0` in every production environment previously provisioned by `pip install -U`, after running `uv pip install -U --index-strategy unsafe-any-match` both the package and its dependencies get downgraded to some “random”-ish versions found in the available indexes:
```sh
Installed 3 packages in 5ms
 - other-internal-package==0.9.0.post2
 + other-internal-package==0.9.0a1
 - six==1.16.0
 + six==1.11.0
 - some-internal-package==0.1.22
 + some-internal-package==0.1.20
```

---

_Comment by @charliermarsh on 2024-04-24 18:15_

I need to think on how to implement this.

---

_Comment by @charliermarsh on 2024-04-26 01:59_

I'm working on this now.

---

_Renamed from "Implement --index-strategy unsafe-highest" to "Implement `--index-strategy unsafe-closest-match`" by @charliermarsh on 2024-04-26 02:37_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-26 02:37_

---

_Review requested from @konstin by @charliermarsh on 2024-04-26 02:37_

---

_Comment by @charliermarsh on 2024-04-26 02:44_

Okay, this seems reasonable to me now. Assuming we consider the number of indexes to be a small constant factor, this should still be linear.

---

_Comment by @charliermarsh on 2024-04-26 02:44_

Adding some other reviewers.

---

_Review requested from @zanieb by @charliermarsh on 2024-04-26 02:45_

---

_@charliermarsh reviewed on 2024-04-26 02:45_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/iterators.rs`:104 on 2024-04-26 02:45_

I think we could make this generic but I kind of gave up. It felt like a lot of work for minimal gain. (It might be nice to have one iterator that can take `Version` or `Reverse<Version>`, but that again felt like a lot of work to save ~30 lines of code. Open to it though!)

---

_Comment by @konstin on 2024-04-26 08:33_

Benchmarks:

Jupyter, an easy but very common use case:
```
Benchmark 1: target/profiling/uv-branch pip compile scripts/requirements/jupyter.in
  Time (mean ± σ):      14.0 ms ±   0.6 ms    [User: 14.4 ms, System: 16.0 ms]
  Range (min … max):    12.6 ms …  16.4 ms    202 runs
 
Benchmark 2: target/profiling/uv-branch pip compile scripts/requirements/jupyter.in --index-strategy unsafe-closest-match
  Time (mean ± σ):      14.0 ms ±   0.7 ms    [User: 14.2 ms, System: 16.2 ms]
  Range (min … max):    12.5 ms …  17.5 ms    211 runs
 
Summary
  target/profiling/uv-branch pip compile scripts/requirements/jupyter.in --index-strategy unsafe-closest-match ran
    1.00 ± 0.07 times faster than target/profiling/uv-branch pip compile scripts/requirements/jupyter.in
```

Airflow with all extra, a hard case with a bunch of backtracking:
```
Benchmark 1: target/profiling/uv-branch pip compile airflow.in
  Time (mean ± σ):     153.4 ms ±   2.8 ms    [User: 142.2 ms, System: 129.7 ms]
  Range (min … max):   149.1 ms … 159.5 ms    19 runs
 
Benchmark 2: target/profiling/uv-branch pip compile airflow.in --index-strategy unsafe-closest-match
  Time (mean ± σ):     153.1 ms ±   2.2 ms    [User: 141.6 ms, System: 132.6 ms]
  Range (min … max):   148.4 ms … 158.0 ms    19 runs
 
Summary
  target/profiling/uv-branch pip compile airflow.in --index-strategy unsafe-closest-match ran
    1.00 ± 0.02 times faster than target/profiling/uv-branch pip compile airflow.in
```

Boto3, the hardest pathological use case we have user hit:
```
Benchmark 1: target/profiling/uv-branch pip compile scripts/requirements/boto3.in
  Time (mean ± σ):     359.2 ms ±   1.6 ms    [User: 316.3 ms, System: 54.2 ms]
  Range (min … max):   357.5 ms … 362.3 ms    10 runs
 
Benchmark 2: target/profiling/uv-branch pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match
  Time (mean ± σ):     391.2 ms ±   3.4 ms    [User: 358.5 ms, System: 43.9 ms]
  Range (min … max):   387.1 ms … 396.4 ms    10 runs
 
Summary
  target/profiling/uv-branch pip compile scripts/requirements/boto3.in ran
    1.09 ± 0.01 times faster than target/profiling/uv-branch pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match
```

This looks much better than i had expected

---

_@konstin reviewed on 2024-04-26 08:53_

---

_Review comment by @konstin on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 08:53_

What about:

```rust
/// An iterator that returns the maximum version from a set of iterators.
pub(crate) struct MaxIterator<'a> {
    heap: Box<dyn Iterator<Item = (&'a Version, VersionMapDistHandle<'a>)> + 'a>,
}

impl<'a> MaxIterator<'a> {
    pub(crate) fn new<T: Iterator<Item = (&'a Version, VersionMapDistHandle<'a>)> + 'a>(
        iterators: Vec<T>,
    ) -> Self {
        let mut heap: Box<dyn Iterator<Item = (&'a Version, VersionMapDistHandle<'a>)>> =
            Box::new(iter::empty());

        // TODO(konsti): The rev is the correct sorting order, isn't it?
        for iterator in iterators.into_iter().rev() {
            heap = Box::new(heap.merge_by(iterator, |a, b| a.0 >= b.0));
        }

        Self { heap }
    }
}

impl<'a> Iterator for MaxIterator<'a> {
    type Item = (&'a Version, VersionMapDistHandle<'a>);

    fn next(&mut self) -> Option<Self::Item> {
        self.heap.next()
    }
}
```

That's fast even in the pathological boto3 case:
```
Benchmark 1: target/profiling/uv-branch pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match
  Time (mean ± σ):     391.2 ms ±   2.3 ms    [User: 350.0 ms, System: 53.1 ms]
  Range (min … max):   388.2 ms … 395.5 ms    10 runs
 
Benchmark 2: target/profiling/uv pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match
  Time (mean ± σ):     357.9 ms ±   0.9 ms    [User: 324.2 ms, System: 44.9 ms]
  Range (min … max):   356.4 ms … 359.0 ms    10 runs
 
Benchmark 3: target/profiling/uv pip compile scripts/requirements/boto3.in
  Time (mean ± σ):     354.6 ms ±   1.3 ms    [User: 320.7 ms, System: 45.7 ms]
  Range (min … max):   353.3 ms … 356.6 ms    10 runs
 
Summary
  target/profiling/uv pip compile scripts/requirements/boto3.in ran
    1.01 ± 0.00 times faster than target/profiling/uv pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match
    1.10 ± 0.01 times faster than target/profiling/uv-branch pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match
```

I'm not sure how readable you consider this, but we can also inline max iterator as:
```rust
iterators.into_iter().rev().fold(
    Box::new(iter::empty()) as Box<dyn Iterator<Item = _>>,
    |acc, iterator| Box::new(acc.merge_by(iterator, |a, b| a.0 >= b.0)),
)
```

---

_@konstin reviewed on 2024-04-26 08:55_

---

_Review comment by @konstin on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 08:55_

I tried these benchmarks with extra index urls but got resolver or build errors, so the benchmark is a bit limited

---

_@charliermarsh reviewed on 2024-04-26 13:10_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 13:10_

I think `next` in that case would be `O(N)` in the number of indexes rather than `O(log(N))` as with the binary heap, right? I guess it's a small constant factor in each case.

---

_@charliermarsh reviewed on 2024-04-26 13:11_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 13:11_

The number of indexes is typically two (if you're using the setting), and ~always less than ten I would guess, so maybe it doesn't matter.

---

_@charliermarsh reviewed on 2024-04-26 13:12_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 13:12_

`itertools` also has a `kmerge_by` which might be exactly what we want? It also uses a binary heap internally (though its own minimal implementation).

---

_@konstin reviewed on 2024-04-26 13:37_

---

_Review comment by @konstin on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 13:37_

I'd expect peeking and comparing versions is faster than the allocations and memcpy in a heap.

For the theory, the head building is expected `O(num versions total)` to build, `O(num versions total * log(num versions total))` worst case, the peeking is also worst case of `O(num versions total * num indexes)`.

We can try `kmerge_by` and benchmark too if we want.

---

_@charliermarsh reviewed on 2024-04-26 13:48_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 13:48_

Sure, `kmerge_by` looks most straightforward to me since we don't need any `Box` or `dyn`, only allocates one vec upfront, etc.

But if we benchmark, shouldn't we include at least one other index...?

---

_@konstin reviewed on 2024-04-26 14:17_

---

_Review comment by @konstin on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 14:17_

Some quick benchmarks with more indexes, not sure why they are so noisy:

```
Benchmark 1: target/profiling/uv-pr pip compile scripts/requirements/jupyter.in
  Time (mean ± σ):      14.7 ms ±   0.7 ms    [User: 13.0 ms, System: 18.2 ms]
  Range (min … max):    13.5 ms …  15.8 ms    10 runs
 
Benchmark 2: target/profiling/uv-pr pip compile scripts/requirements/jupyter.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
  Time (mean ± σ):      9.953 s ± 13.549 s    [User: 0.041 s, System: 0.043 s]
  Range (min … max):    2.838 s … 41.590 s    10 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Benchmark 3: target/profiling/uv-mine pip compile scripts/requirements/jupyter.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
  Time (mean ± σ):      9.418 s ± 14.120 s    [User: 0.041 s, System: 0.044 s]
  Range (min … max):    1.626 s … 47.047 s    10 runs
 
  Warning: The first benchmarking run for this command was significantly slower than the rest (47.047 s). This could be caused by (filesystem) caches that were not filled until after the first run. You are already using the '--warmup' option which helps to fill these caches before the actual benchmark. You can either try to increase the warmup count further or re-run this benchmark on a quiet system in case it was a random outlier. Alternatively, consider using the '--prepare' option to clear the caches before each timing run.
 
Summary
  target/profiling/uv-pr pip compile scripts/requirements/jupyter.in ran
  640.31 ± 960.55 times faster than target/profiling/uv-mine pip compile scripts/requirements/jupyter.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
  676.71 ± 921.83 times faster than target/profiling/uv-pr pip compile scripts/requirements/jupyter.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
```  

```
Benchmark 1: target/profiling/uv-pr pip compile scripts/requirements/airflow.in
  Time (mean ± σ):     218.6 ms ±   2.6 ms    [User: 205.9 ms, System: 151.8 ms]
  Range (min … max):   214.4 ms … 223.3 ms    10 runs
 
Benchmark 2: target/profiling/uv-pr pip compile scripts/requirements/airflow.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
  Time (mean ± σ):      8.727 s ±  1.060 s    [User: 0.394 s, System: 0.223 s]
  Range (min … max):    7.264 s … 10.827 s    10 runs
 
Benchmark 3: target/profiling/uv-mine pip compile scripts/requirements/airflow.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
  Time (mean ± σ):      9.036 s ±  1.217 s    [User: 0.378 s, System: 0.235 s]
  Range (min … max):    7.355 s … 11.375 s    10 runs
 
Summary
  target/profiling/uv-pr pip compile scripts/requirements/airflow.in ran
   39.91 ± 4.87 times faster than target/profiling/uv-pr pip compile scripts/requirements/airflow.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
   41.32 ± 5.59 times faster than target/profiling/uv-mine pip compile scripts/requirements/airflow.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
```

```
Benchmark 1: target/profiling/uv-pr pip compile scripts/requirements/boto3.in
  Time (mean ± σ):     360.4 ms ±   0.8 ms    [User: 321.0 ms, System: 50.9 ms]
  Range (min … max):   359.4 ms … 362.3 ms    10 runs
 
Benchmark 2: target/profiling/uv-pr pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
  Time (mean ± σ):      1.558 s ±  0.580 s    [User: 0.361 s, System: 0.052 s]
  Range (min … max):    0.840 s …  2.441 s    10 runs
 
Benchmark 3: target/profiling/uv-mine pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
  Time (mean ± σ):      1.750 s ±  0.538 s    [User: 0.363 s, System: 0.052 s]
  Range (min … max):    0.921 s …  2.470 s    10 runs
 
Summary
  target/profiling/uv-pr pip compile scripts/requirements/boto3.in ran
    4.32 ± 1.61 times faster than target/profiling/uv-pr pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
    4.86 ± 1.49 times faster than target/profiling/uv-mine pip compile scripts/requirements/boto3.in --index-strategy unsafe-closest-match --extra-index-url https://test.pypi.org/simple --extra-index-url https://download.pytorch.org/whl/cu118
```

---

_@konstin reviewed on 2024-04-26 14:19_

---

_Review comment by @konstin on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 14:19_

I'm not sure if it's worth optimizing this case to perfection, i'm fine with merging with whatever is easiest

---

_@charliermarsh reviewed on 2024-04-26 14:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/iterators.rs`:10 on 2024-04-26 14:29_

I think you need _way_ more than 10 runs and a large warmup for this kind of thing. But I'll just merge whatever looks easiest in the end.

---

_Comment by @charliermarsh on 2024-04-26 14:29_

@zanieb @BurntSushi - any feedback on the name of the option, the docs, exposing this? (As opposed to the implementation which seems ok.)

---

_Comment by @zanieb on 2024-04-26 14:54_

+1 to exposing, I can review the docs too.

For naming, we have...

- First match: Only versions from the first index with the package name are considered (makes sense to me)
- Any match: Versions from the indexes are exhausted in-order (not very clear?)
- Closest match: The version is taken from the index with the closest version (makes sense to me)

I think this name makes sense in the context of the others. Although "any match" confuses the naming scheme a bit.


---

_Comment by @BurntSushi on 2024-04-26 15:19_

Yeah this LGTM. Another possible name is `unsafe-best-match`. In particular, the docs use the word "best" instead of "closest" to describe its behavior. I don't have a strong opinion on what the right word to choose here, but I loosely prefer "best." In my view, "closest" raises the question of "closest to what?" Where as "best" I think can be cast as "best matching version for the chosen resolution strategy." But I think this is probably a quibble.

---

_Comment by @charliermarsh on 2024-04-26 17:04_

What about...

- `single-index` (only consider versions from a single index for each package)
- `unsafe-first-match` (use the first index with a compatible version)
- `unsafe-best-match` (use the index with the "best" version)

---

_Comment by @zanieb on 2024-04-26 17:13_

That's an improvement.

Should `single-index` be `first-index` — i.e. use the first index with the package regardless of whether or not a version match exists?

---

_Comment by @charliermarsh on 2024-04-26 18:32_

I will make these changes (and add aliases for the existing values), revisit the merge implementation, then ship it.

---

_@charliermarsh approved on 2024-04-27 01:08_

---

_Renamed from "Implement `--index-strategy unsafe-closest-match`" to "Implement `--index-strategy unsafe-best-match`" by @charliermarsh on 2024-04-27 01:19_

---

_Merged by @charliermarsh on 2024-04-27 01:24_

---

_Closed by @charliermarsh on 2024-04-27 01:24_

---

_Comment by @zanieb on 2024-04-27 01:51_

Thanks @yorickvP !

---

_Comment by @yorickvP on 2024-07-22 12:54_

Looking closer, the behaviour implemented in the PR is subtly different from the behaviour I intended. I bisected it to the `kmerge_by` commit. It seems to be picking the package from pypi instead of my index if they have the same version. In this case, the pypi package fails to build.

```console
$ git checkout 67b8389aa7579befaa7e6cc64afba58b89d9556b
$ cargo build
$ echo 'tensorrt-llm==0.11.0' | ./target/debug/uv pip compile - --extra-index-url https://pypi.nvidia.com --python-version=3.10 --index-strategy=unsafe-best-match --annotation-style=line
[..]
tensorrt-llm==0.11.0
```
vs

```console
$ git checkout 275f1503721f4a04639279d6f065de86c5075c6e
$ cargo build
$ echo 'tensorrt-llm==0.11.0' | ./target/debug/uv pip compile - --extra-index-url https://pypi.nvidia.com --python-version=3.10 --index-strategy=unsafe-best-match --annotation-style=line
error: Failed to download and build `tensorrt-llm==0.11.0`
  Caused by: Failed to build: `tensorrt-llm==0.11.0`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/yorick/.cache/uv/.tmpZbNSch/.venv/lib/python3.11/site-packages/nvidia_stub/buildapi.py", line 29, in build_wheel
    return download_wheel(pathlib.Path(wheel_directory), config_settings)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/yorick/.cache/uv/.tmpZbNSch/.venv/lib/python3.11/site-packages/nvidia_stub/wheel.py", line 175, in download_wheel
    report_install_failure(distribution, version, None)
  File "/home/yorick/.cache/uv/.tmpZbNSch/.venv/lib/python3.11/site-packages/nvidia_stub/error.py", line 63, in report_install_failure
    raise InstallFailedError(
nvidia_stub.error.InstallFailedError:
*******************************************************************************

The installation of tensorrt-llm for version 0.11.0 failed.

This is a special placeholder package which downloads a real wheel package
from https://pypi.nvidia.com. If https://pypi.nvidia.com is not reachable, we
cannot download the real wheel file to install.
```

---

_Comment by @charliermarsh on 2024-07-22 13:02_

Sounds like a problem with breaking ties / stable ordering. Want to open an issue? Interested in working on it?

---

_Comment by @yorickvP on 2024-07-22 14:08_

https://github.com/astral-sh/uv/pull/5288

---
