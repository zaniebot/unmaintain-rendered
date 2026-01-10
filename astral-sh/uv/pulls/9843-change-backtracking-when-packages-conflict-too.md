```yaml
number: 9843
title: Change backtracking when packages conflict too much
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - performance
  - resolver
assignees: []
merged: true
base: main
head: konsti/backtrack
created_at: 2024-12-12T15:44:12Z
updated_at: 2024-12-16T10:39:52Z
url: https://github.com/astral-sh/uv/pull/9843
synced_at: 2026-01-10T12:00:01Z
```

# Change backtracking when packages conflict too much

---

_Pull request opened by @konstin on 2024-12-12 15:44_

Background reading: https://github.com/astral-sh/uv/issues/8157
Companion PR: https://github.com/astral-sh/pubgrub/pull/36
Requires for test coverage: https://github.com/astral-sh/packse/pull/230

When two packages A and B conflict, we have the option to choose a lower version of A, or a lower version of B. Currently, we determine this by the order we saw a package (assuming equal specificity of the requirement): If we saw A before B, we pin A until all versions of B are exhausted. This can lead to undesirable outcomes, from cases where it's just slow (sentry) to others cases without lower bounds where be backtrack to a very old version of B. This old version may fail to build (terminating the resolution), or it's a version so old that it doesn't depend on A (or the shared conflicting package) anymore - but also is too old for the user's application (fastapi). #8157 collects such cases, and the `wrong-backtracking` packse scenario contains a minimized example.  

We try to solve this by tracking which packages are "A"s, culprits, and "B"s, affected, and manually interfering with project selection and backtracking. Whenever a version we just chose is rejected, we give the current package a counter for being affected, and the package it conflicted with a counter for being a culprit. If a package accumulates more counts than a threshold, we reprioritize: Undecided after the culprits, after the affected, after packages that only have a single version (URLs, `==<version>`). We then ask pubgrub to backtrack just before the culprit. Due to the changed priorities, we now select package B, the affected, instead of package A, the culprit.

To do this efficiently, we ask pubgrub for the incompatibility that caused backtracking, or just the last version to be discarded (due to its dependencies). For backtracking, we use the last incompatibility from unit propagation as a heuristic. When a version is discarded because one of its dependencies conflicts with the partial solution, the incompatibility tells us the package in the partial solution that conflicted.

We only backtrack once per package, on the first time it passes the threshold. This prevents backtracking loops in which we make the same decisions over and over again. But we also changed the priority, so that we shouldn't take the same path even after the one time we backtrack (it would defeat the purpose of this change).

There are some parameters that can be tweaked: Currently, the threshold is set to 5, which feels not too eager with so me of the conflicts that we want to tolerate but also changes strategies quickly. The relative order of the new priorities can also be changed, as for each (A, B) pair the priority of B is afterwards lower than that for A. Currently, culprits capture conflict for the whole package, but we could limit that to a specific version. We could discard conflict counters after backtracking instead of keeping them eternally as we do now. Note that we're always taking about pairs (A, B), but in practice we track individual packages, not pairs.

A case that we wouldn't capture is when B is only introduced to the dependency graph after A, but I think that would require cyclical dependency for A and B to conflict? There may also be cases where looking at the last incompatibility is insufficient.

Another example that we can't repair with prioritization is urllib3/boto3/botocore: We actually have to check all the newer versions of boto3 and botocore to identify the version that allows with the older urllib3, no shortcuts allowed.

```
urllib3<1.25.4
boto3
```

All examples I tested were cases with two packages where we only had to switch the order, so I've abstracted them into a single packse case.

This PR changes the resolution for certain paths, and there is the risk for regressions.

Fixes #8157

---

All tested examples improved.

Input fastapi:
```text
starlette<=0.36.0
fastapi<=0.115.2
```

```
# BEFORE
$ uv pip --no-progress compile -p 3.11 --exclude-newer 2024-10-01 --no-annotate debug/fastapi.txt
annotated-types==0.7.0
anyio==4.6.0
fastapi==0.1.17
idna==3.10
pydantic==2.9.2
pydantic-core==2.23.4
sniffio==1.3.1
starlette==0.36.0
typing-extensions==4.12.2

# AFTER
$ cargo run --profile fast-build --no-default-features pip compile -p 3.11 --no-progress --exclude-newer 2024-10-01 --no-annotate debug/fastapi.txt 
annotated-types==0.7.0
anyio==4.6.0
fastapi==0.109.1
idna==3.10
pydantic==2.9.2
pydantic-core==2.23.4
sniffio==1.3.1
starlette==0.35.1
typing-extensions==4.12.2
```


Input xarray:
```text
xarray[accel]
```

```
# BEFORE
$ uv pip --no-progress compile -p 3.11 --exclude-newer 2024-10-01 --no-annotate debug/xarray-accel.txt
bottleneck==1.4.0
flox==0.9.13
llvmlite==0.36.0
numba==0.53.1
numbagg==0.8.2
numpy==2.1.1
numpy-groupies==0.11.2
opt-einsum==3.4.0
packaging==24.1
pandas==2.2.3
python-dateutil==2.9.0.post0
pytz==2024.2
scipy==1.14.1
setuptools==75.1.0
six==1.16.0
toolz==0.12.1
tzdata==2024.2
xarray==2024.9.0

# AFTER
$ cargo run --profile fast-build --no-default-features pip compile -p 3.11 --no-progress --exclude-newer 2024-10-01 --no-annotate debug/xarray-accel.txt
bottleneck==1.4.0
flox==0.9.13
llvmlite==0.43.0
numba==0.60.0
numbagg==0.8.2
numpy==2.0.2
numpy-groupies==0.11.2
opt-einsum==3.4.0
packaging==24.1
pandas==2.2.3
python-dateutil==2.9.0.post0
pytz==2024.2
scipy==1.14.1
six==1.16.0
toolz==0.12.1
tzdata==2024.2
xarray==2024.9.0
```


Input sentry: The resolution is identical, but arrived at much faster: main tries 69 versions (sentry-kafka-schemas: 63), PR tries 12 versions (sentry-kafka-schemas: 6; 5 times conflicting, then once the right version).

```text
python-rapidjson<=1.20,>=1.4
sentry-kafka-schemas<=0.1.113,>=0.1.50
```

```
# BEFORE
$ uv pip --no-progress compile -p 3.11 --exclude-newer 2024-10-01 --no-annotate debug/sentry.txt
fastjsonschema==2.20.0
msgpack==1.1.0
python-rapidjson==1.8
pyyaml==6.0.2
sentry-kafka-schemas==0.1.111
typing-extensions==4.12.2

# AFTER
$ cargo run --profile fast-build --no-default-features pip compile -p 3.11 --no-progress --exclude-newer 2024-10-01 --no-annotate debug/sentry.txt
fastjsonschema==2.20.0
msgpack==1.1.0
python-rapidjson==1.8
pyyaml==6.0.2
sentry-kafka-schemas==0.1.111
typing-extensions==4.12.2
```


Input apache-beam
```text
# Run on Python 3.10
dill<0.3.9,>=0.2.2
apache-beam<=2.49.0
```

```
# BEFORE
$ uv pip --no-progress compile -p 3.10 --exclude-newer 2024-10-01 --no-annotate debug/apache-beam.txt
  × Failed to download and build `apache-beam==2.0.0`
  ╰─▶ Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

# AFTER
$ cargo run --profile fast-build --no-default-features pip compile -p 3.10 --no-progress --exclude-newer 2024-10-01 --no-annotate debug/apache-beam.txt
apache-beam==2.49.0
certifi==2024.8.30
charset-normalizer==3.3.2
cloudpickle==2.2.1
crcmod==1.7
dill==0.3.1.1
dnspython==2.6.1
docopt==0.6.2
fastavro==1.9.7
fasteners==0.19
grpcio==1.66.2
hdfs==2.7.3
httplib2==0.22.0
idna==3.10
numpy==1.24.4
objsize==0.6.1
orjson==3.10.7
proto-plus==1.24.0
protobuf==4.23.4
pyarrow==11.0.0
pydot==1.4.2
pymongo==4.10.0
pyparsing==3.1.4
python-dateutil==2.9.0.post0
pytz==2024.2
regex==2024.9.11
requests==2.32.3
six==1.16.0
typing-extensions==4.12.2
urllib3==2.2.3
zstandard==0.23.0
```


---

_Label `enhancement` added by @konstin on 2024-12-12 15:44_

---

_Label `resolver` added by @konstin on 2024-12-12 15:44_

---

_Renamed from "Change backtracking when packages conflict a lot" to "Change backtracking when packages conflict too much" by @konstin on 2024-12-12 15:44_

---

_Comment by @codspeed-hq[bot] on 2024-12-12 15:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fbacktrack)

### Merging #9843 will **degrade performances by 23.19%**

<sub>Comparing <code>konsti/backtrack</code> (bc074f0) with <code>main</code> (2b61a67)</sub>



### Summary

`⚡ 1` improvements  
`❌ 1` regressions  
`✅ 12` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/konsti%2Fbacktrack)._

### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/backtrack` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_airflow` | 1.1 s | 1.4 s | -23.19% |
| ⚡ | `resolve_warm_jupyter_universal` | 340.9 ms | 279.6 ms | +21.92% |


---

_Marked ready for review by @konstin on 2024-12-12 16:02_

---

_Comment by @notatallshaw on 2024-12-12 16:18_

> We only backtrack once per package, on the first time it passes the threshold

I'm curious why you decided to choose a threshold for this rather than some other heuristic, such as the nature of the version specifier (e.g. triggering if the conflict was driven by one of `<`, `<=`, `!=`, `==N.*`)?

In particular, as an end user I would find it surprising that I would get problematic behavior on a new release of a package, and then without any of its dependencies changing it just fixed itself once that package was released 4 more times.


---

_Comment by @konstin on 2024-12-12 17:26_

These pathological behaviors can occur independent of the version specifiers used, tracking conflicts gives us a stronger heuristic. In the sentry example, we have both lower and upper bounds, so we can only avoid the wrong backtracking with conflict tracking. In all cases tested, the problematic behavior was resolved by current heuristic.

While we're technically using a SAT solver and each solution that fulfills all constraints is correct to emit, we also have a goal to emit desirable resolutions, where desirable is a vague notion of using newer packages and avoiding old packages. In that sense we're somewhat more like integer linear programming because we're also optimizing for a (vague) target function. (There are approaches for a globally optimal solution to resolution that use a real target function for the most desirable solution, but they are prohibitively expensive.)

In one framing, this switching priorities is trying to avoid a branch in which we would get a bad score. For the fastapi and apache-beam cases this is working: We're avoiding the bad solution with the very old fastapi and the apache-beam version that doesn't build anymore.

> In particular, as an end user I would find it surprising that I would get problematic behavior on a new release of a package, and then without any of its dependencies changing it just fixed itself once that package was released 4 more times.

While i agree that this would be annoying, it's a rare case that needs very specific circumstances. It requires that there are already two possible solutions, one with the older and one with the newer version of the package, that are only decided by prioritization, i.e. we consider both as acceptable solutions.

---

_Review requested from @BurntSushi by @konstin on 2024-12-12 17:33_

---

_Review requested from @charliermarsh by @konstin on 2024-12-12 17:33_

---

_Review request for @charliermarsh removed by @konstin on 2024-12-12 17:33_

---

_Review requested from @zanieb by @konstin on 2024-12-12 17:33_

---

_Review requested from @charliermarsh by @konstin on 2024-12-12 17:33_

---

_Comment by @notatallshaw on 2024-12-12 18:27_

Thanks for the explanation, I'm eager to see the results in the real world, i.e. once it's unleashed on to the many users of uv, I think I can adapt this optimization to the pip-resolvelib stack (I have a lot more fixing of basic issues first, but I'm putting it on my to-do list to investigate).

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/priority.rs`:83 on 2024-12-12 19:14_

I think this can accept a `&OccupiedEntry<...>`.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/priority.rs`:115 on 2024-12-12 19:17_

Personally, I'd do this:

```rust
#[cfg(debug_assertions)]
{
    panic!("yadda yadda yadda");
}
```

It took me a second to read what this assert was doing, but I think the above is a little easier to read.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/priority.rs`:142 on 2024-12-12 19:17_

Same as above.

---

_@BurntSushi approved on 2024-12-12 19:20_

I think this makes sense to me!

---

_@charliermarsh reviewed on 2024-12-14 02:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:135 on 2024-12-14 02:07_

Can you add a comment here, and clarify how it's different from `make_conflict_early`?

---

_@charliermarsh reviewed on 2024-12-14 02:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:341 on 2024-12-14 02:08_

"the is"

---

_@charliermarsh reviewed on 2024-12-14 02:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:3344 on 2024-12-14 02:08_

One thing to note here is that `Id<PubGrubPackage>` is different for proxies, like `Package::Extra` and `Package::Package` have different IDs.

---

_@charliermarsh reviewed on 2024-12-14 02:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:13913 on 2024-12-14 02:09_

Should we use the display representation for the incompatibility at the end?

---

_@charliermarsh approved on 2024-12-14 02:09_

Excellent.

---

_@konstin reviewed on 2024-12-16 09:23_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:3344 on 2024-12-16 09:23_

Surprisingly, keeping them separate showed better performance than conflict counts over the proxies of a package.

---

_Comment by @konstin on 2024-12-16 10:02_

Packse publishing broke (https://github.com/astral-sh/packse/actions/runs/12349693707/job/34461019415), I'll add the test case later.

---

_Label `performance` added by @konstin on 2024-12-16 10:02_

---

_Merged by @konstin on 2024-12-16 10:39_

---

_Closed by @konstin on 2024-12-16 10:39_

---

_Branch deleted on 2024-12-16 10:39_

---
