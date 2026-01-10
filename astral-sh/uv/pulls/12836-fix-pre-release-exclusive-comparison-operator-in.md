```yaml
number: 12836
title: Fix pre-release exclusive comparison operator in uv-pep440
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/pep440-prelease-exclude-matching
created_at: 2025-04-11T12:45:28Z
updated_at: 2025-04-14T08:02:50Z
url: https://github.com/astral-sh/uv/pull/12836
synced_at: 2026-01-10T11:10:40Z
```

# Fix pre-release exclusive comparison operator in uv-pep440

---

_Pull request opened by @konstin on 2025-04-11 12:45_

From PEP 440:

> The exclusive ordered comparison <V MUST NOT allow a pre-release of the specified version unless the specified version is itself a pre-release. Allowing pre-releases that are earlier than, but not equal to a specific pre-release may be accomplished by using <V.rc1 or similar.

We had an additional check that would block this even if the specifier did have a pre-release.

This likely didn't show up earlier because `Ranges` uses different code in the resolver.

I checked these changes against `packaging` to verify their behavior:

```python
print(SpecifierSet("<1").contains("1a1", prereleases=True)) # False
print(SpecifierSet("<1a2").contains("1a1", prereleases=True)) # True
print(SpecifierSet("<1").contains("1dev1", prereleases=True)) # False
print(SpecifierSet("<1dev2").contains("1dev1", prereleases=True)) # True
print(SpecifierSet("<1a2").contains("1dev1", prereleases=True)) # True
```

Closes #12834

---

_Label `bug` added by @konstin on 2025-04-11 12:45_

---

_Review requested from @BurntSushi by @konstin on 2025-04-11 12:45_

---

_Renamed from "Fix `0.1a1<0.1a2`" to "Fix prerelease comparisons in uv-pep440" by @konstin on 2025-04-11 12:45_

---

_Renamed from "Fix prerelease comparisons in uv-pep440" to "Fix pre-release exclusive comparison operator in uv-pep440" by @konstin on 2025-04-11 12:46_

---

_@Gankra reviewed on 2025-04-11 14:10_

---

_Review comment by @Gankra on `crates/uv-pep440/src/version_specifier.rs`:558 on 2025-04-11 14:10_

Is there a Reason some of these are Proper operators and others are named methods?

---

_@Gankra approved on 2025-04-11 14:12_

This took me way too long to review because my brain could not accept that it was actually intended for <3.1 to *not* match 3.1.dev0 but still match 3.0.dev0... wild!

---

_@BurntSushi approved on 2025-04-11 15:33_

Nice!

---

_Comment by @notatallshaw on 2025-04-11 17:37_

> This took me way too long to review because my brain could not accept that it was actually intended for <3.1 to _not_ match 3.1.dev0 but still match 3.0.dev0... wild!

In the Python version spec `<` does not preserve ordinality, the version numbers themselves are ordinal, e.g. 3.1a1 is less than 3.1b1 is less than 3.1, but while `< 3.1b1` contains 3.1a1, is is not true that  `< 3.1` contains 3.1a1.

You can think of `< 3.1` being the equivalent of `< 3.1dev0` but where prereleases are not implied.

I would be careful testing against packaging, it does not have all of this prerelease spec correctly implemented for SpecifierSet yet: https://github.com/pypa/packaging/pull/872

---

_Merged by @charliermarsh on 2025-04-12 20:57_

---

_Closed by @charliermarsh on 2025-04-12 20:57_

---

_Branch deleted on 2025-04-12 20:57_

---

_@konstin reviewed on 2025-04-14 08:02_

---

_Review comment by @konstin on `crates/uv-pep440/src/version_specifier.rs`:558 on 2025-04-14 08:02_

Addressed in #12873

---
