```yaml
number: 3466
title: add support for resolving without a marker environment
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/partial-marker-eval
created_at: 2024-05-08T18:10:47Z
updated_at: 2024-05-09T13:24:38Z
url: https://github.com/astral-sh/uv/pull/3466
synced_at: 2026-01-10T14:37:54Z
```

# add support for resolving without a marker environment

---

_Pull request opened by @BurntSushi on 2024-05-08 18:10_

This PR touches a lot of code, but the conceptual change here is
pretty simple: make it so we can run the resolver without providing a
`MarkerEnvironment`. This also indicates that the resolver should run in
universal mode. That is, the effect of a missing marker environment is
that all marker expressions that reference the marker environment are
evaluated to `true`. That is, they are ignored. (The only markers we
evaluate in that context are extras, which are the only markers that
aren't dependent on the environment.)

One interesting change here is that a `Resolver` no longer needs an
`Interpreter`. Previously, it had only been using it to construct a
`PythonRequirement`, by filling in the installed version from the
`Interpreter` state. But we now construct a `PythonRequirement`
explicitly since its `target` Python version should no longer be tied to
the `MarkerEnvironment`. (Currently, the marker environment is mutated
such that its `python_full_version` is derived from multiple sources,
including the CLI, which I found a touch confusing.)

The change in behavior can now be observed through the
`--unstable-uv-lock-file` flag. First, without it:

```
$ cat requirements.in
anyio>=4.3.0 ; sys_platform == "linux"
anyio<4 ; sys_platform == "darwin"
$ cargo run -qp uv -- pip compile -p3.10 requirements.in
anyio==4.3.0
exceptiongroup==1.2.1
    # via anyio
idna==3.7
    # via anyio
sniffio==1.3.1
    # via anyio
typing-extensions==4.11.0
    # via anyio
```

And now with it:

```
$ cargo run -qp uv -- pip compile -p3.10 requirements.in --unstable-uv-lock-file
  x No solution found when resolving dependencies:
  `-> Because you require anyio>=4.3.0 and anyio<4, we can conclude that the requirements are unsatisfiable.
```

This is expected at this point because the marker expressions are being
explicitly ignored, *and* there is no forking done yet to account for
the conflict.

Closes #3352, Closes #3353


---

_Label `internal` added by @BurntSushi on 2024-05-08 18:11_

---

_Review requested from @konstin by @BurntSushi on 2024-05-08 18:11_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-08 18:11_

---

_@charliermarsh reviewed on 2024-05-09 05:14_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:565 on 2024-05-09 05:14_

Is the key thing here that we _do_ need to evaluate extras? Otherwise, I could imagine just doing something like `env.map(...).unwrap_or(true)` instead of encoding this in the API.

---

_@charliermarsh approved on 2024-05-09 05:16_

This looks right to me.

---

_@charliermarsh reviewed on 2024-05-09 05:18_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_compile.rs`:244 on 2024-05-09 05:18_

Yeah this seems right. Or, at least, equivalent to our existing behavior.

---

_@charliermarsh reviewed on 2024-05-09 05:19_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_compile.rs`:244 on 2024-05-09 05:19_

Is there any way this method could take `&Interpreter` and `&MarkerEnvironment`, so we abstract away the use of `markers.python_full_version`? (I know this is no different than on `main`, don't spend much time on it if difficult.)

---

_@BurntSushi reviewed on 2024-05-09 11:41_

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker.rs`:565 on 2024-05-09 11:41_

Yes. We specifically want to evaluate extras.

---

_@BurntSushi reviewed on 2024-05-09 11:45_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/pip_compile.rs`:244 on 2024-05-09 11:45_

Yeah I agree the way I did things isn't quite optimal. Maybe the best thing here is to add an alternate constructor that takes a `MarkerEnvironment`. But I don't think we make it the only constructor because we fundamentally need a way of constructing a `PythonRequirement` without a `MarkerEnvironment`.

---

_@BurntSushi reviewed on 2024-05-09 11:51_

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker.rs`:565 on 2024-05-09 11:51_

I believe the basic issue here is that our [optional dependencies manifest as additions to a requirement's markers](https://github.com/astral-sh/uv/blob/d3c72896f9a344296b594ad3b0f2c945430a365e/crates/pypi-types/src/metadata.rs#L224-L235). So when we do resolution, we very specifically want to evaluate the "extra" marker expressions because we're _only_ looking for universality with respect to platform ("marker environment"), and _not_ with respect to the set of possible optional dependencies.

In my prototype, I [dropped optional dependencies altogether](https://github.com/astral-sh/uv/blob/7f2b401260e6231bebb867c034b27d2955210fe0/crates/pypi-types/src/metadata.rs#L246-L257). I couldn't move forward without that because it was too easy for, e.g., packages to bring in huge trees of dev dependencies.

---

_Merged by @BurntSushi on 2024-05-09 13:24_

---

_Closed by @BurntSushi on 2024-05-09 13:24_

---

_Branch deleted on 2024-05-09 13:24_

---
