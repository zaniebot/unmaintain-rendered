```yaml
number: 1436
title: "Implement TID251 (banning modules & module members)"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: banned-api
created_at: 2022-12-29T08:41:08Z
updated_at: 2022-12-30T03:11:13Z
url: https://github.com/astral-sh/ruff/pull/1436
synced_at: 2026-01-12T15:55:06Z
```

# Implement TID251 (banning modules & module members)

---

_@not-my-profile_

Resolves  #1422.

---

_@charliermarsh reviewed on 2022-12-29 12:17_

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/settings.rs`:89 on 2022-12-29 12:17_

I think we may need to sort this by key to ensure a stable hash. Or use a `BTreeMap`.

---

_@charliermarsh reviewed on 2022-12-29 12:18_

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/settings.rs`:65 on 2022-12-29 12:18_

I know you explicitly preferred `banned_api`, but given the minor difference, it's worth it to me to maintain compatibility with `flake8-tidy-imports` and keep this as `banned-modules`. How do you feel about that?

---

_@charliermarsh reviewed on 2022-12-29 12:19_

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/checks.rs`:79 on 2022-12-29 12:19_

Typically, if methods take `checker`, then we just call `checker.add_check` rather than returning an `Option<Check>`.

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/checks.rs`:32 on 2022-12-29 12:19_

Nit: can this take `&str`?

---

_@charliermarsh reviewed on 2022-12-29 12:19_

---

_@charliermarsh reviewed on 2022-12-29 12:21_

---

_Review comment by @charliermarsh on `src/checks.rs`:2454 on 2022-12-29 12:21_

I'd probably vote to nix this property and custom message, I think it might be confusing to users (and the false-positive cases here are rare enough that the tradeoff seems not-worth-it).

---

_Comment by @charliermarsh on 2022-12-29 12:22_

This looks great! Some minor comments. Thank you for the PR!

---

_@not-my-profile reviewed on 2022-12-29 14:50_

---

_Review comment by @not-my-profile on `src/flake8_tidy_imports/checks.rs`:32 on 2022-12-29 14:50_

Changing it to take `&str` would result in a needless allocation since the string is firstly formatted in order to be passed to the function and then later has to be owned in order to be part of the CheckKind.

We could change it to instead take two `&str` and and move the `format!` into the function but I don't really see the point.

---

_@not-my-profile reviewed on 2022-12-29 14:56_

---

_Review comment by @not-my-profile on `src/checks.rs`:2454 on 2022-12-29 14:56_

I think claiming that something is the case when it is not is much more confusing than having a disclaimer that the check result may be wrong. Whether or not false-positive cases are rare depends entirely on the code base. The check is not authoritative, it's just an educated guess ... I think the wording of the message should reflect that.

---

_@not-my-profile reviewed on 2022-12-29 15:04_

---

_Review comment by @not-my-profile on `src/flake8_tidy_imports/settings.rs`:65 on 2022-12-29 15:04_

Yes I don't like `banned-modules` because it's misleading since the setting can also be used for module members. I do think correct semantics make a big difference in terms of usability. And I don't get your point about compatibility given that you cannot copy'n'paste `flake8-tidy-imports` config into the ruff config anyway because:

1. flake8-tidy-imports does not use TOML but .cfg which uses a different syntax.
2. You have to wrap the names in double quotes and appent `.msg`.

If we still want both settings to have the same name despite having different formats, I think it would make more sense if `flake8-tidy-imports` updated its setting name to `banned-api` since it's the more correct terminology.

---

_@charliermarsh reviewed on 2022-12-29 18:19_

---

_Review comment by @charliermarsh on `src/checks.rs`:2454 on 2022-12-29 18:19_

I hear you but I just don't think this is a tenable position. There are so many ways to avoid static detection in Python. By this logic, we should be adding a disclaimer like this to almost every check.

---

_@charliermarsh reviewed on 2022-12-29 18:25_

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/checks.rs`:32 on 2022-12-29 18:25_

I think the benefit would be that you could then pass in `module: &str` and `name: Alias`, unless I'm misreading, rather than having to extract part of the alias (`let name = &loc_name.node.name`) and pass that in + format it beforehand.

---

_@charliermarsh reviewed on 2022-12-29 18:31_

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/settings.rs`:65 on 2022-12-29 18:31_

I guess this is fine. There is some mental overhead to deviating (folks that migrate their config have to know to change the setting name) but I understand your point, and I don't feel strongly.

What's your reaction to `banned import`? There was some discussion [here](https://github.com/adamchainz/flake8-tidy-imports/issues/38) and it looks like that's the verbiage that `flake8-tidy-imports` uses in their [messages](https://github.com/adamchainz/flake8-tidy-imports/pull/40).

---

_@charliermarsh reviewed on 2022-12-29 21:43_

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/settings.rs`:65 on 2022-12-29 21:43_

I was thinking about this a bit more, and I prefer `banned-imports` over `banned-api`. The issue with `banned-api` is that it's _too_ general, and actually implies things that go beyond what the tool can do. For example, to me, `banned-api` would also include things like method calls.

---

_Review comment by @not-my-profile on `src/checks.rs`:2454 on 2022-12-29 21:46_

Fair enough, I removed it :)

---

_@not-my-profile reviewed on 2022-12-29 21:46_

---

_@not-my-profile reviewed on 2022-12-29 21:56_

---

_Review comment by @not-my-profile on `src/flake8_tidy_imports/checks.rs`:32 on 2022-12-29 21:56_

Ah yes that makes sense; changed.

---

_Review comment by @charliermarsh on `src/flake8_tidy_imports/settings.rs`:65 on 2022-12-29 22:02_

(If you agree, I'm also happy to do the work of renaming, then merge the PR.)

---

_@charliermarsh reviewed on 2022-12-29 22:02_

---

_Review comment by @not-my-profile on `src/flake8_tidy_imports/settings.rs`:65 on 2022-12-29 22:04_

Yes "banned import" would be accurate for `flake8-tidy-imports` but only because they don't yet implement the attribute access check (adamchainz/flake8-tidy-imports/issues/63), which this PR implements. Since the check also checks for attribute access "banned import" isn't accurate, for example:

```python
import foo
foo.bar # TIDI201 Banned import 'foo.bar' used
```

`foo.bar` here is not an import.

---

_@not-my-profile reviewed on 2022-12-29 22:04_

---

_Merged by @charliermarsh on 2022-12-30 03:11_

---

_Closed by @charliermarsh on 2022-12-30 03:11_

---
