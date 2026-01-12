```yaml
number: 1828
title: "Implement `PLR2004` (`MagicValueComparison`)"
type: pull_request
state: merged
author: max0x53
labels: []
assignees: []
merged: true
base: main
head: plr2004
created_at: 2023-01-12T18:48:18Z
updated_at: 2023-01-18T06:07:55Z
url: https://github.com/astral-sh/ruff/pull/1828
synced_at: 2026-01-12T05:25:13Z
```

# Implement `PLR2004` (`MagicValueComparison`)

---

_Pull request opened by @max0x53 on 2023-01-12 18:48_

This PR adds [Pylint `R2004`](https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/magic-value-comparison.html#magic-value-comparison-r2004)

Feel free to suggest changes and additions, I have tried to maintain parity with the Pylint implementation [`magic_value.py`](https://github.com/PyCQA/pylint/blob/main/pylint/extensions/magic_value.py)

See #970 

---

_@not-my-profile reviewed on 2023-01-12 19:21_

---

_Review comment by @not-my-profile on `src/pylint/rules/magic_value_comparison.rs`:17 on 2023-01-12 19:21_

I think the following would be more idiomatic:

```rust
if matches!(value.try_into(), Ok(-1 | 0 | 1)) {
    None
}
```

---

_@max0x53 reviewed on 2023-01-12 19:34_

---

_Review comment by @max0x53 on `src/pylint/rules/magic_value_comparison.rs`:17 on 2023-01-12 19:34_

Agreed! 

---

_@charliermarsh reviewed on 2023-01-12 21:08_

---

_Review comment by @charliermarsh on `src/pylint/rules/magic_value_comparison.rs`:17 on 2023-01-12 21:08_

It looks like Pylint flags on any constant, but allows strings `""` and `"__main__"`.

https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#valid-magic-values

---

_@charliermarsh reviewed on 2023-01-12 21:09_

---

_Review comment by @charliermarsh on `src/pylint/rules/magic_value_comparison.rs`:40 on 2023-01-12 21:09_

I think we may need to avoid comparisons between _two_ constants?

https://github.com/PyCQA/pylint/blob/0c5ab132e262280da98742e5cf73ecc1cbfa7377/pylint/extensions/magic_value.py#L81

---

_Comment by @charliermarsh on 2023-01-12 21:09_

This looks great! Couple comments to get it fully aligned with the Pylint implementation.

Speaking of: how do you enable this check with Pylint? I notice it's part of an "extension". Trying to figure out how to test it locally...

---

_Comment by @max0x53 on 2023-01-12 22:35_

To test R2004 with pylint you will need to follow their instructions for [Contributor installation](https://pylint.pycqa.org/en/latest/development_guide/contributor_guide/tests/install.html).

Once the basic installation is complete you can run the following to confirm you are on the main branch.

```bash
$ pylint --version
pylint 2.16.0-dev
```

Once installed you can run the following command to test R2004 (with the flag [documented here](https://pylint.pycqa.org/en/latest/whatsnew/2/2.16/index.html))

```bash
$ pylint --load-plugins=pylint.extensions.magic_value ./magic_value_comparison.py | grep R2004
<PATH>/ruff/resources/test/fixtures/pylint/magic_value_comparison.py:6:3: R2004: Consider using a named constant or an enum instead of '10'. (magic-value-comparison)
<PATH>/ruff/resources/test/fixtures/pylint/magic_value_comparison.py:29:3: R2004: Consider using a named constant or an enum instead of '2'. (magic-value-comparison)
<PATH>/ruff/resources/test/fixtures/pylint/magic_value_comparison.py:44:3: R2004: Consider using a named constant or an enum instead of 'Hunter2'. (magic-value-comparison)
<PATH>/ruff/resources/test/fixtures/pylint/magic_value_comparison.py:50:3: R2004: Consider using a named constant or an enum instead of '3.141592653589793'. (magic-value-comparison)
<PATH>/ruff/resources/test/fixtures/pylint/magic_value_comparison.py:59:3: R2004: Consider using a named constant or an enum instead of 'b'something''. (magic-value-comparison)
```

Hope this helps!

Current `ruff` output for completeness

```bash
resources/test/fixtures/pylint/magic_value_comparison.py:6:4: PLR2004 Magic number used in comparison, consider replacing 10 with a constant variable
resources/test/fixtures/pylint/magic_value_comparison.py:29:4: PLR2004 Magic number used in comparison, consider replacing 2 with a constant variable
resources/test/fixtures/pylint/magic_value_comparison.py:44:4: PLR2004 Magic number used in comparison, consider replacing 'Hunter2' with a constant variable
resources/test/fixtures/pylint/magic_value_comparison.py:50:4: PLR2004 Magic number used in comparison, consider replacing 3.141592653589793 with a constant variable
resources/test/fixtures/pylint/magic_value_comparison.py:59:4: PLR2004 Magic number used in comparison, consider replacing b'something' with a constant variable
Found 5 error(s).
```

---

_@max0x53 reviewed on 2023-01-12 22:38_

---

_Review comment by @max0x53 on `src/pylint/rules/magic_value_comparison.rs`:40 on 2023-01-12 22:38_

Good catch, have added a comment to hopefully better explain why the rule is skipped.

---

_Review comment by @max0x53 on `src/pylint/rules/magic_value_comparison.rs`:17 on 2023-01-12 22:39_

Have updated to reflect this, required a refactor of some of the workings but I believe for the better.

---

_@max0x53 reviewed on 2023-01-12 22:39_

---

_Review comment by @charliermarsh on `src/pylint/rules/magic_value_comparison.rs`:53 on 2023-01-12 22:48_

I think it'll be simpler to do something like:

```
for (a, b) in std::iter::once(left).chain(comparators.iter()).tuple_windows() {
  if matches!(a.node, ExprKind::Constant { ... }) && matches!(b.node, ExprKind::Constant { .. }) {
    return;
  }
}
```

At the top of the function.

---

_@charliermarsh reviewed on 2023-01-12 22:49_

Oh, thank you! So helpful.

I think there are still a few false-positives here:

```py
if x == 3:
    pass

if 1 == 3:
    pass

if 4 == 3 == x:
    pass
```

Ruff gives:

```
❯ cargo run foo.py --select PLR2004 -n
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/ruff foo.py --select PLR2004 -n`
foo.py:1:4: PLR2004 Magic number used in comparison, consider replacing 3 with a constant variable
foo.py:4:4: PLR2004 Magic number used in comparison, consider replacing 3 with a constant variable
foo.py:7:4: PLR2004 Magic number used in comparison, consider replacing 3 with a constant variable
foo.py:7:4: PLR2004 Magic number used in comparison, consider replacing 4 with a constant variable
Found 4 error(s).
```

Whereas Pylint gives:

```
❯ pylint --load-plugins=pylint.extensions.magic_value foo.py | grep R2004
foo.py:1:3: R2004: Consider using a named constant or an enum instead of '3'. (magic-value-comparison)
```

I think this comes down to: (1) for the purpose of _skipping_, Pylint considers `1` and such to be constants, even if they don't get flagged as magic, whereas the current implementation here does not (since it relies on the number of diagnostics generated); and (2) it looks like Pylint actually skips if there's _any_ constant comparison, not just if they're all constant.


---

_Comment by @max0x53 on 2023-01-12 23:23_

Let me know if you would prefer the 2nd for loop to be refactored into an iterator, have left as is for now.

---

_Renamed from "Implement PLR2004" to "Implement `PLR2004` (`MagicValueComparison`)" by @charliermarsh on 2023-01-12 23:29_

---

_Comment by @charliermarsh on 2023-01-12 23:29_

Thank you! Running CI now. Will merge on pass.

---

_Comment by @max0x53 on 2023-01-12 23:31_

Cheers! Appreciate you being so responsive to PRs and a credit to `ruff` for being as easy as it was to add new linting rules (docs and code were great)!

---

_Comment by @charliermarsh on 2023-01-12 23:37_

Thank you, that's awesome to hear! (\cc @not-my-profile as he's made some refactors lately that, IMO, made adding rules a lot more accessible.)

---

_Merged by @charliermarsh on 2023-01-13 00:44_

---

_Closed by @charliermarsh on 2023-01-13 00:44_

---

_Branch deleted on 2023-01-13 00:47_

---

_Comment by @martinkozle on 2023-01-16 11:49_

Now how do I ignore this lint? Because when I put `ignore = ["PLR2004"]` in `pyproject.toml` it says that it is not a valid option.

Edit: nvm, it does have an effect, `Better TOML` VSCode extension just says that it is not a valid option, I don't know if the problem is on my end

Edit 2: The linting was based on the version of ruff that comes with the VSCode extension where this lint wasn't implemented yet. So yes, it was a problem on my end

---

_Comment by @charliermarsh on 2023-01-16 16:28_

(We have to update the JSON Schema manually in the Schema Store, unfortunately, so it sometimes gets a little out-of-date with latest Ruff.)

---

_Comment by @ofek on 2023-01-18 06:07_

Feature request: https://github.com/charliermarsh/ruff/issues/1949

---
