```yaml
number: 1206
title: Test to prevent continious reformatting when used together with black
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: black-compatibility-test
created_at: 2022-12-12T07:20:55Z
updated_at: 2022-12-27T05:43:00Z
url: https://github.com/astral-sh/ruff/pull/1206
synced_at: 2026-01-12T05:36:31Z
```

# Test to prevent continious reformatting when used together with black

---

_Pull request opened by @squiddy on 2022-12-12 07:20_

Runs the sequence `ruff -> black -> ruff` against all test fixtures and fails when that sequence is not stable.

Pretty slow right now and it should probably run against a specific version of black. Would like to get some guidance here on how best to approach that.

- [x] run against specific version of black
- [x] skip test cases that (intentionally) trigger parser issues (E999.py)
- [x] make fast enough?
- [x] setup black to make CI work

---

This did turn up a bug:

When running ruff against 

```python
_ = lambda x: setattr(x, "bar", 1)
``` 
with everything enabled, this produces

```python
def _(x):
    return x.bar = 1
```

which is invalid syntax.

---

_Comment by @andersk on 2022-12-12 08:07_

Repeated invocations of the `black` script spend most of their time starting up the Python interpreter and importing things. So you can probably make this significantly faster using [`blackd`](https://black.readthedocs.io/en/stable/usage_and_configuration/black_as_a_server.html).

---

_@JonathanPlasse reviewed on 2022-12-12 13:18_

---

_Review comment by @JonathanPlasse on `tests/integration_test.rs`:138 on 2022-12-12 13:18_

Stub files (`.pyi`) should also be checked.

---

_Review comment by @JonathanPlasse on `tests/integration_test.rs`:152 on 2022-12-12 13:34_

Maybe use this instead.
```suggestion
        CheckCategory::iter()
            .map(|category| category.codes().iter().map(AsRef::as_ref).join(", "))
            .join(", ")
```

---

_@JonathanPlasse reviewed on 2022-12-12 13:34_

---

_Review comment by @squiddy on `tests/integration_test.rs`:138 on 2022-12-12 18:37_

Thanks.

---

_@squiddy reviewed on 2022-12-12 18:37_

---

_Comment by @squiddy on 2022-12-12 18:59_

In a hurry, so I'll just put it here (another bug):

```
cargo run resources/test/fixtures/isort/leading_prefix.py --select F,E,W,C90,I,D,UP,N,YTT,ANN,S,BLE,FBT,B,A,C4,T10,ICN,T20,Q,RET,S
IM,TID,ARG,ERA,PGH,PLC,PLE,PLR,PLW --fix
```

Produces

```

if True:

```

*edit* see #1226 

---

_@squiddy reviewed on 2022-12-12 19:00_

---

_Review comment by @squiddy on `tests/integration_test.rs`:152 on 2022-12-12 19:00_

This is fantastic, thank you. :)

---

_@squiddy reviewed on 2022-12-12 19:00_

---

_Review comment by @squiddy on `tests/integration_test.rs`:152 on 2022-12-12 19:00_

I do exclude RUF codes currently, because those add `# noqa`, but not always at the same run apparantely?!. Need to test that again.

---

_Review comment by @squiddy on `tests/integration_test.rs`:152 on 2022-12-13 17:24_

It's `W292` (no end of newline) that was causing that. Ruff add a `noqa` it in the first run, black adds a newline, ruff removes the `noqa`.

For now I keep excluding RUF.

---

_@squiddy reviewed on 2022-12-13 17:24_

---

_Marked ready for review by @squiddy on 2022-12-13 17:25_

---

_Comment by @squiddy on 2022-12-13 17:26_

Putting this up for final review.

The test surfaced some bugs which are reported. For CI's sake I ignore those fixture files explicitely.

I'm missing some alignment on how to setup the local development environment. Currently the code just assumes `blackd` is somewhere in `$PATH`.

---

_Review comment by @andersk on `tests/black_compatibility_test.rs`:27 on 2022-12-13 19:24_

`Ipv4Addr::LOCALHOST` is safer.

---

_Review comment by @andersk on `tests/black_compatibility_test.rs`:105 on 2022-12-13 19:27_

Do we need to invoke Ruff as an external process here?

---

_@andersk reviewed on 2022-12-13 19:31_

Given that this test requires an external dependency and takes about a minute, maybe we should `#[ignore]` it by default and run it only in CI?

---

_Comment by @charliermarsh on 2022-12-13 20:58_

Yeah this is gonna be great to have! It's already surfaced a bunch of bugs. But I agree that it should probably only be included on an opt-in (on CI, and manually when desired).

---

_@squiddy reviewed on 2022-12-15 04:05_

---

_Review comment by @squiddy on `tests/black_compatibility_test.rs`:27 on 2022-12-15 04:05_

Done, thanks.

---

_@squiddy reviewed on 2022-12-15 04:07_

---

_Review comment by @squiddy on `tests/black_compatibility_test.rs`:105 on 2022-12-15 04:07_

The initial ticket was mentioning integration test so I put it next to the others. As far as I understand that means I do not have access to the crate itself.

Nothing is really stopping us from moving this into the ruff tests though.

---

_Comment by @squiddy on 2022-12-15 05:55_

Test now only runs on CI by default.

---

_Comment by @charliermarsh on 2022-12-15 17:39_

Sweet, I'll read through and merge today!

---

_@charliermarsh reviewed on 2022-12-15 19:04_

---

_Review comment by @charliermarsh on `tests/black_compatibility_test.rs`:25 on 2022-12-15 19:04_

Do you have a sense for how much of a difference `blackd` vs. `black` makes?

---

_@squiddy reviewed on 2022-12-15 20:01_

---

_Review comment by @squiddy on `tests/black_compatibility_test.rs`:25 on 2022-12-15 20:01_

Did a couple runs with blackd vs. black on a Ryzen 5800X. ~44s with blackd, 64s with black.

---

_@charliermarsh reviewed on 2022-12-15 20:17_

---

_Review comment by @charliermarsh on `tests/black_compatibility_test.rs`:25 on 2022-12-15 20:17_

Ok cool. My only hesitation is around robustness. If we see flakiness on CI (e.g., from waiting for `blackd` to startup, or other daemon-related issues), I may ask that we convert back to `black`, since I'd rather CI be reliable than fast (at least, on that order of magnitude).

---

_Merged by @charliermarsh on 2022-12-15 20:26_

---

_Closed by @charliermarsh on 2022-12-15 20:26_

---

_Comment by @charliermarsh on 2022-12-15 20:26_

Awesome, thank you for this @squiddy!

---

_Branch deleted on 2022-12-27 05:43_

---
