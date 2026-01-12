```yaml
number: 3831
title: add initial support for universal lock files
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/forking
created_at: 2024-05-24T19:03:42Z
updated_at: 2024-05-30T18:23:15Z
url: https://github.com/astral-sh/uv/pull/3831
synced_at: 2026-01-12T16:05:52Z
```

# add initial support for universal lock files

---

_@BurntSushi_

This PR implements "resolver forking" in a way that permits us to
generate "universal" lock files. The concrete manifestation of this
requires omitting a `MarkerEnvironment` when calling the resolver, and
as a result, the resolution returned may include multiple versions for
the same package.

As a very basic example, consider this `requirements.in` file:

```
anyio>=4.3.0 ; sys_platform == "linux"
anyio<4 ; sys_platform == "darwin"
```

And while not the final intended command, one can generate a `uv.lock`
with the following:

```
$ cargo run -p uv -- pip compile -p3.10 ~/astral/tmp/reqs/forks/basic.in --unstable-uv-lock-file
```

And the resulting `uv.lock`:

```
version = 1

[[distribution]]
name = "anyio"
version = "3.7.1"
source = "registry+https://pypi.org/simple"
marker = "sys_platform == 'darwin'"

[distribution.sdist]
url = "https://files.pythonhosted.org/packages/28/99/2dfd53fd55ce9838e6ff2d4dac20ce58263798bd1a0dbe18b3a9af3fcfce/anyio-3.7.1.tar.gz"
hash = "sha256:44a3c9aba0f5defa43261a8b3efb97891f2bd7d804e0e1f56419befa1adfc780"
size = 142927

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/19/24/44299477fe7dcc9cb58d0a57d5a7588d6af2ff403fdd2d47a246c91a3246/anyio-3.7.1-py3-none-any.whl"
hash = "sha256:91dee416e570e92c64041bd18b900d1d6fa78dff7048769ce5ac5ddad004fbb5"
size = 80896

[[distribution.dependencies]]
name = "exceptiongroup"
version = "1.2.1"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "idna"
version = "3.7"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "sniffio"
version = "1.3.1"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "typing-extensions"
version = "4.12.0"
source = "registry+https://pypi.org/simple"

[[distribution]]
name = "anyio"
version = "4.3.0"
source = "registry+https://pypi.org/simple"
marker = "sys_platform == 'linux'"

[distribution.sdist]
url = "https://files.pythonhosted.org/packages/db/4d/3970183622f0330d3c23d9b8a5f52e365e50381fd484d08e3285104333d3/anyio-4.3.0.tar.gz"
hash = "sha256:f75253795a87df48568485fd18cdd2a3fa5c4f7c5be8e5e36637733fce06fed6"
size = 159642

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl"
hash = "sha256:048e05d0f6caeed70d731f3db756d35dcc1f35747c8c403364a8332c630441b8"
size = 85584

[[distribution.dependencies]]
name = "exceptiongroup"
version = "1.2.1"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "idna"
version = "3.7"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "sniffio"
version = "1.3.1"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "typing-extensions"
version = "4.12.0"
source = "registry+https://pypi.org/simple"

[[distribution]]
name = "exceptiongroup"
version = "1.2.1"
source = "registry+https://pypi.org/simple"
marker = "python_version < '3.11'"

[distribution.sdist]
url = "https://files.pythonhosted.org/packages/a0/65/d66b7fbaef021b3c954b3bbb196d21d8a4b97918ea524f82cfae474215af/exceptiongroup-1.2.1.tar.gz"
hash = "sha256:a4785e48b045528f5bfe627b6ad554ff32def154f42372786903b7abcfe1aa16"
size = 28717

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/01/90/79fe92dd413a9cab314ef5c591b5aa9b9ba787ae4cadab75055b0ae00b33/exceptiongroup-1.2.1-py3-none-any.whl"
hash = "sha256:5258b9ed329c5bbdd31a309f53cbfb0b155341807f6ff7606a1e801a891b29ad"
size = 16458

[[distribution]]
name = "idna"
version = "3.7"
source = "registry+https://pypi.org/simple"

[distribution.sdist]
url = "https://files.pythonhosted.org/packages/21/ed/f86a79a07470cb07819390452f178b3bef1d375f2ec021ecfc709fc7cf07/idna-3.7.tar.gz"
hash = "sha256:028ff3aadf0609c1fd278d8ea3089299412a7a8b9bd005dd08b9f8285bcb5cfc"
size = 189575

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl"
hash = "sha256:82fee1fc78add43492d3a1898bfa6d8a904cc97d8427f683ed8e798d07761aa0"
size = 66836

[[distribution]]
name = "sniffio"
version = "1.3.1"
source = "registry+https://pypi.org/simple"

[distribution.sdist]
url = "https://files.pythonhosted.org/packages/a2/87/a6771e1546d97e7e041b6ae58d80074f81b7d5121207425c964ddf5cfdbd/sniffio-1.3.1.tar.gz"
hash = "sha256:f4324edc670a0f49750a81b895f35c3adb843cca46f0530f79fc1babb23789dc"
size = 20372

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl"
hash = "sha256:2f6da418d1f1e0fddd844478f41680e794e6051915791a034ff65e5f100525a2"
size = 10235

[[distribution]]
name = "typing-extensions"
version = "4.12.0"
source = "registry+https://pypi.org/simple"
marker = "python_version < '3.8' or python_version < '3.11'"

[distribution.sdist]
url = "https://files.pythonhosted.org/packages/ce/6a/aa0a40b0889ec2eb81a02ee0daa6a34c6697a605cf62e6e857eead9e4f85/typing_extensions-4.12.0.tar.gz"
hash = "sha256:8cbcdc8606ebcb0d95453ad7dc5065e6237b6aa230a31e81d0f440c30fed5fd8"
size = 84291

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/e1/4d/d612de852a0bc64a64418e1cef25fe1914c5b1611e34cc271ed7e36174c8/typing_extensions-4.12.0-py3-none-any.whl"
hash = "sha256:b349c66bea9016ac22978d800cfff206d5f9816951f12a7d0ec5578b0a819594"
size = 37104
```

In particular, notice that there are two `anyio` distribution entries:

```
[[distribution]]
name = "anyio"
version = "3.7.1"
source = "registry+https://pypi.org/simple"
marker = "sys_platform == 'darwin'"

[[distribution]]
name = "anyio"
version = "4.3.0"
source = "registry+https://pypi.org/simple"
marker = "sys_platform == 'linux'"
```

The intent here is that, on install, a `MarkerEnvironment` would be
used to do a graph traversal while filtering out distributions whose
marker expressions don't match the marker environment. In order to
work, this does require that we only permit multiple versions of the
same package when their marker expressions have an empty intersection.

This is a draft because there is still some work to be done:

* There are some test failures that need to be investigated.
* This doesn't yet limit forking to cases where the marker expressions
have an empty intersection.
* We should include tests, although I might do those in a follow-up PR
since this one is already pretty big.

I'm also a little uneasy with how we're handling marker expressions in
the `PubGrubPackage`. Careful scrutiny there would be appreciated!

Closes #3358, Closes #3360

---

_Comment by @codspeed-hq[bot] on 2024-05-24 19:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ag/forking)

### Merging #3831 will **degrade performances by 7.66%**

<sub>Comparing <code>ag/forking</code> (94b9c3b) with <code>main</code> (1445669)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ag/forking)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ag/forking` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter` | 82 ms | 88.8 ms | -7.66% |


---

_Marked ready for review by @BurntSushi on 2024-05-29 19:04_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-29 19:04_

---

_Comment by @charliermarsh on 2024-05-29 19:09_

Will review this today.

---

_@charliermarsh reviewed on 2024-05-29 19:15_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:46 on 2024-05-29 19:15_

This might need to be keyed on `(PackageName, Option<ExtraName>)`?

---

_@charliermarsh reviewed on 2024-05-29 19:17_

Random question but one that's on my mind from my own work: in `crates/uv-resolver/src/pubgrub/dependencies.rs#add_requirements`, we call `requirement.evaluate_markers(env, std::slice::from_ref(source_extra))`. That now always evaluates to `true` when the marker environment is omitted, so how do we collect different dependencies based on the extra?

---

_@charliermarsh reviewed on 2024-05-29 19:26_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:852 on 2024-05-29 19:26_

Can you include an example here?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1071 on 2024-05-29 19:26_

Why is this necessary? Can you refresh my memory?

---

_@charliermarsh reviewed on 2024-05-29 19:26_

---

_@charliermarsh reviewed on 2024-05-29 19:32_

I think there's another area that isn't covered yet -- something like:

```
anyio @ https://files.pythonhosted.org/packages/7b/a2/10639a79341f6c019dedc95bd48a4928eed9f1d1197f4c04f546fc7ae0ff/anyio-4.4.0-py3-none-any.whl ; python_version > '3.10'

anyio @ https://files.pythonhosted.org/packages/85/4f/d010eca6914703d8e6be222165d02c3e708ed909cdb2b7af3743667f302e/anyio-4.1.0-py3-none-any.whl ; python_version <= '3.10'
```

These are two different URLs, but I assume `urls::Urls` will reject these as conflicting upfront? We might need to make all of those "do work upfront" abstractions _also_ track markers in some way.


---

_@charliermarsh reviewed on 2024-05-29 19:36_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:852 on 2024-05-29 19:36_

Separately, what _do_ we want to do? Take the intersection? What if we can't solve the intersection...? Is that possible?

---

_@charliermarsh reviewed on 2024-05-29 19:37_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1071 on 2024-05-29 19:37_

To ensure they resolve to the same version, or something?

---

_@BurntSushi reviewed on 2024-05-30 17:41_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/graph.rs`:46 on 2024-05-30 17:41_

Yup, nice catch. Fixed!

---

_Comment by @BurntSushi on 2024-05-30 17:44_

> Random question but one that's on my mind from my own work: in `crates/uv-resolver/src/pubgrub/dependencies.rs#add_requirements`, we call `requirement.evaluate_markers(env, std::slice::from_ref(source_extra))`. That now always evaluates to `true` when the marker environment is omitted, so how do we collect different dependencies based on the extra?

Good question! When a `MarkerEnvironment` is not provided, the only the marker expressions referencing the environment always evaluate to `true`. But any `extra` marker expressions are still left in tact:

```rust
    /// Evaluates this marker tree against an optional environment and a
    /// possibly empty sequence of extras.
    ///
    /// When an environment is not provided, all marker expressions based on
    /// the environment evaluate to `true`. That is, this provides environment
    /// independent marker evaluation. In practice, this means only the extras
    /// are evaluated when an environment is not provided.
    pub fn evaluate_optional_environment(
        &self,
        env: Option<&MarkerEnvironment>,
        extras: &[ExtraName],
    ) -> bool {
```

So that means `requirement.evaluate_markers(env, std::slice::from_ref(source_extra))` doesn't always evaluate to true when `env` is `None`, because the marker expression might reference a different extra than `source_extra`.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:852 on 2024-05-30 17:48_

I was thinking that if we took the intersection and couldn't solve it, then the resolution overall would fail.

But now that I think about this more, I'm no longer convinced we should do this. And that checking that the markers are disjoint should be enough. I think what I had thought previously was that if the versions overlapped, then it would be possible to emit a lock file whose graph traversal would result in selecting two different versions of the same package. But I don't think that's true _given_ that we require an empty intersection for the corresponding marker expressions (which I have not added yet).

I'll remove this comment. Thanks for flagging it.

---

_@BurntSushi reviewed on 2024-05-30 17:48_

---

_@BurntSushi reviewed on 2024-05-30 18:10_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1071 on 2024-05-30 18:10_

I can't quite explain this yet. If I don't do this (and adjust some of the code in `resolution/graph.rs` to not require `marker: None`), then I get a few test failures. One of them is this:

```
Snapshot: compile_constraints_markers
Source: crates/uv/tests/pip_compile.rs:322
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    5     5 │ anyio==4.3.0
    6     6 │     # via -r requirements.in
    7     7 │ idna==3.6
    8     8 │     # via anyio
    9       │-sniffio==1.3.0
          9 │+sniffio==1.3.1
   10    10 │     # via
   11    11 │     #   -c constraints.txt
   12    12 │     #   anyio
   13    13 │
   14    14 │ ----- stderr -----
   15       │-Resolved 3 packages in [TIME]
         15 │+Resolved 4 packages in [TIME]
```

And there are 3 more failures that look like this:

```
Snapshot: install_executable
Source: crates/uv/tests/pip_install.rs:1802
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3     3 │
    4     4 │ ----- stderr -----
    5       │-Resolved 7 packages in [TIME]
          5 │+Resolved 8 packages in [TIME]
    6     6 │ Downloaded 7 packages in [TIME]
    7     7 │ Installed 7 packages in [TIME]
    8     8 │  + astroid==3.0.3
    9     9 │  + dill==0.3.8
```

I think I originally added this because I was trying to follow in the footsteps of how extras were handled. But markers aren't exactly like extras, and I'm not sure this sort of dependency makes sense.

Looking at the `compile_constraints_markers` test more closely, I see that it looks like there is probably something relevant here:

```
    // If constraints are ignored, these will conflict
    constraints_txt.write_str("sniffio==1.2.0;python_version<='3.7'")?;
    constraints_txt.write_str("sniffio==1.3.0;python_version>'3.7'")?;
```

So if there is no `marker: None` "dependency on itself" being added (as is done here in this PR), then I believe the above two constraints will end up being two _distinct_ `PubGrubPackage` values due to the markers being distinct. Which in turn means the pubgrub constraints won't unify and I think leads to a different resolution? Although I can't fully explain why `1.3.1` is selected. With the dependency on itself, I think that lets pubgrub unify the constraints, which I think _is_ similar to the same kind of effect we achieve with extras?

I'm like 80% confident here. I've updated the comment here to hopefully expose some of that uncertainty:

```rust
                // If a package has a marker, add a dependency from it to the
                // same package without markers.
                //
                // At time of writing, AG doesn't fully understand why we need
                // this, but one explanation is that without it, there is no
                // way to connect two different `PubGrubPackage` values with
                // the same package name but different markers. With different
                // markers, they would be considered wholly distinct packages.
                // But this dependency-on-itself-without-markers forces PubGrub
                // to unify the constraints across what would otherwise be two
                // distinct packages.
```

---

_@charliermarsh approved on 2024-05-30 18:15_

Assuming this passes tests, and you feel it's ready, good by me!

---

_Comment by @charliermarsh on 2024-05-30 18:17_

> ```rust
> evaluate_optional_environment
> ```

Ohh, I was misreading `evaluate_markers`. The `if`-`let` is on `self.marker`, not `env`. Thanks.

---

_Merged by @BurntSushi on 2024-05-30 18:23_

---

_Closed by @BurntSushi on 2024-05-30 18:23_

---

_Branch deleted on 2024-05-30 18:23_

---
