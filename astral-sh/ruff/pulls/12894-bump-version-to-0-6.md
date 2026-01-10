```yaml
number: 12894
title: Bump version to 0.6
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: bump-version-ruff-0.6
created_at: 2024-08-14T16:19:02Z
updated_at: 2024-08-15T12:17:24Z
url: https://github.com/astral-sh/ruff/pull/12894
synced_at: 2026-01-10T21:38:32Z
```

# Bump version to 0.6

---

_Pull request opened by @MichaReiser on 2024-08-14 16:19_

## Summary

This PR now does the *actual* release of Ruff 0.6


* [x] Write Changelog
* [x] Copy breaking changes into `BREAKING_CHANGES.md`
* [ ] Run the release workflow
* [ ] Update the schemastore
* [ ] Bump ruff version in ruff-vscode, release extension



---

_Label `release` added by @MichaReiser on 2024-08-14 16:19_

---

_Comment by @github-actions[bot] on 2024-08-14 16:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-15 09:31_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:27 on 2024-08-15 09:35_

I would put these two sections above the first bullet point you included in the "Breaking changes" section -- and maybe also above the section about Jupyter notebooks now being linted/formatted by default. I think they're more breaking than the change to the default for `PT001` and `PT023`, since these changes might require you to make changes to your Ruff config file to keep your CI green and free of warnings.

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2024-08-15 09:35_

```suggestion
- Detect imports in `src` layouts by default for `isort` rules ([#12848](https://github.com/astral-sh/ruff/pull/12848))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:117 on 2024-08-15 09:38_

```suggestion
- Evaluate default parameter values for a function in that function's enclosing scope ([#12852](https://github.com/astral-sh/ruff/pull/12852))
```

---

_@AlexWaygood approved on 2024-08-15 09:39_

---

_@MichaReiser reviewed on 2024-08-15 09:42_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:27 on 2024-08-15 09:42_

I prefer keeping it as is because it is more consistent with past releases. We can shorten the first bullet points by removing the lengthy Jupyter Notebook section and instead link to the configuration (and blog post). We can keep the lengthy explanation in the `BREAKING_CHANGES.md`

---

_@AlexWaygood reviewed on 2024-08-15 09:44_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:27 on 2024-08-15 09:44_

Yes, shortening the Jupyter notebook section might be good. I would also at least swap the order of the two bullets you have in the "Breaking changes" section

---

_@MichaReiser reviewed on 2024-08-15 10:04_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:27 on 2024-08-15 10:04_

Can you expand on which bullets you would swap? Is it the `src` change and the change of the rule defaults?

---

_@AlexWaygood reviewed on 2024-08-15 10:06_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:27 on 2024-08-15 10:06_

Ah, sorry! I suppose I would probably put the (shortened) Jupyter notebook bullet first, then the `isort` `src` bullet, and the bullet about the `PT` rules last.

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2024-08-15 10:25_

I don't think this is the right link

---

_@AlexWaygood reviewed on 2024-08-15 10:25_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:122 on 2024-08-15 10:26_

This is actually in the notebook metadata and is used to detect the preferred language for the entire notebook (not just cells). I would phrase it like:

"Respect kernelspec in the notebook metadata when detecting the preferred language for a Jupyter Notebook."

---

_@dhruvmanila reviewed on 2024-08-15 10:26_

---

_@AlexWaygood reviewed on 2024-08-15 10:39_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:82 on 2024-08-15 10:39_

```suggestion
- [`long-sleep-not-forever`](https://docs.astral.sh/ruff/rules/long-sleep-not-forever/) (`ASYNC116`): Support `anyio` context mangers.
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:38 on 2024-08-15 11:09_

are you sure?

---

_@MichaReiser reviewed on 2024-08-15 11:09_

---

_Marked ready for review by @MichaReiser on 2024-08-15 11:15_

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:5 on 2024-08-15 11:19_

Here, too, I'd personally put this bullet point last out of the three, since these rules have safe autofixes. But just a nit.

---

_@AlexWaygood approved on 2024-08-15 11:19_

---

_@MichaReiser reviewed on 2024-08-15 11:23_

---

_Review comment by @MichaReiser on `BREAKING_CHANGES.md`:5 on 2024-08-15 11:23_

I prefer to keep the long notebook section last. But I can move it further down if you insist ;)

---

_Merged by @AlexWaygood on 2024-08-15 12:17_

---

_Closed by @AlexWaygood on 2024-08-15 12:17_

---

_Branch deleted on 2024-08-15 12:17_

---
