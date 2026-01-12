```yaml
number: 1391
title: Now replace typing.Text with str
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: txt_str_alias
created_at: 2022-12-26T19:44:12Z
updated_at: 2022-12-27T00:55:42Z
url: https://github.com/astral-sh/ruff/pull/1391
synced_at: 2026-01-12T05:36:31Z
```

# Now replace typing.Text with str

---

_Pull request opened by @colin99d on 2022-12-26 19:44_

This PR allows typing.Text with str, as requested on #827. This upgrade will NOT remove the import of typing.Text (I checked and pyupgrade does not remove the import, so I kept with what they did).

I had one blocker on this PR, I could not get the fixer to return true, so currently it only tells you what is wrong, but the test performed as expected so I am confident it works. If you can point me in the direction of how to fix it, I would love to do that real quick.

---

_@charliermarsh reviewed on 2022-12-26 19:47_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/typing_text_str_alias.rs`:17 on 2022-12-26 19:47_

If you run `cargo run resources/test/fixtures/pyupgrade/UP002.py --fix --no-cache`, does that do what you want?

---

_@charliermarsh reviewed on 2022-12-26 19:48_

---

_Review comment by @charliermarsh on `src/checks.rs`:1621 on 2022-12-26 19:48_

Sorry for the inconvenience, but do you mind changing this to `UP019`? `UP002` used to be mapped to a different check, which we had to remove (`pyupgrade` removed it too, it was "wrong"), and so using `UP002` would do the wrong thing here if users upgrade from old to new versions.

---

_@charliermarsh reviewed on 2022-12-26 19:48_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/UP002.py`:4 on 2022-12-26 19:48_

Nit: add a test case for `def print_word(word: typing.Text) -> None: print(word)`?

---

_@charliermarsh reviewed on 2022-12-26 19:49_

---

_Review comment by @charliermarsh on `src/checks.rs`:2541 on 2022-12-26 19:49_

Nit: mind changing `depreciated use str` to `deprecated, use str instead to match  the format of `DeprecatedUnittestAlias` below?

---

_@colin99d reviewed on 2022-12-26 22:14_

---

_Review comment by @colin99d on `src/checks.rs`:1621 on 2022-12-26 22:14_

I can definitely do that, how do I know which code to use, I tried looking on pyupgrade but didn't see (for future reference)?

---

_@charliermarsh reviewed on 2022-12-26 22:16_

---

_Review comment by @charliermarsh on `src/checks.rs`:1621 on 2022-12-26 22:16_

They’re just made up, since pyupgrade doesn’t have codes. So we use the next available slot each time (skipping UP002).

---

_@colin99d reviewed on 2022-12-26 22:48_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/typing_text_str_alias.rs`:17 on 2022-12-26 22:48_

Not sure what changed, but it works now. Thank you.

---

_@colin99d reviewed on 2022-12-26 22:51_

---

_Review comment by @colin99d on `resources/test/fixtures/pyupgrade/UP002.py`:4 on 2022-12-26 22:51_

Good catch, this does not work.

---

_@colin99d reviewed on 2022-12-26 23:12_

---

_Review comment by @colin99d on `resources/test/fixtures/pyupgrade/UP002.py`:4 on 2022-12-26 23:12_

Fixed!

---

_@colin99d reviewed on 2022-12-26 23:33_

---

_Review comment by @colin99d on `src/checks.rs`:1621 on 2022-12-26 23:33_

Perfect. Got this fixed!

---

_Comment by @charliermarsh on 2022-12-27 00:37_

(You can ignore the `[Playground]` failure.)

---

_Merged by @charliermarsh on 2022-12-27 00:55_

---

_Closed by @charliermarsh on 2022-12-27 00:55_

---
