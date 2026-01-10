```yaml
number: 8843
title: "[`pylint`] Add missing return doc rule (`W9011`)"
type: pull_request
state: closed
author: johannes-graner
labels: []
assignees: []
base: main
head: pylint-w9011-missing-return-doc
created_at: 2023-11-26T19:40:19Z
updated_at: 2024-03-28T15:24:41Z
url: https://github.com/astral-sh/ruff/pull/8843
synced_at: 2026-01-10T22:47:01Z
```

# [`pylint`] Add missing return doc rule (`W9011`)

---

_Pull request opened by @johannes-graner on 2023-11-26 19:40_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is part of #970, it adds the `W9011` error rule.

The rule checks that a return documentation is part of the docstring for functions that return something.
Exactly how the docstring is checked depends on the setting `pylint.convention`, which can take the same values as `pydocstyle.convention` (Google, NumPy, or Pep257). The Pep257 option checks for reST style docstrings.

## Test Plan

The example from [pylint documentation](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/missing-return-doc.html) is used, and extended to cover more cases.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-11-26 19:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb reviewed on 2023-11-27 15:45_

---

_Review comment by @zanieb on `crates/ruff_linter/src/codes.rs`:288 on 2023-11-27 15:45_

We add new rules in preview first

```suggestion
        (Pylint, "W9011") => (RuleGroup::Preview, rules::pylint::rules::MissingReturnDoc),
```

---

_Comment by @zanieb on 2023-11-27 15:46_

Thanks for contributing!

~Should we avoid raising this for code with a return of `None`? There are a lot of cases in the ecosystem checks where a return would be meaningless.~ It looks like you implemented this, actually. Could you take a look at the ecosystem checks and see if they are false positives or correct? This rule is introducing _a lot_ of violations.

---

_@johannes-graner reviewed on 2023-11-27 17:56_

---

_Review comment by @johannes-graner on `crates/ruff_linter/src/codes.rs`:288 on 2023-11-27 17:56_

That makes a lot of sense, I missed it when reading `CONTRIBUTING.md`. Thanks for catching!

---

_Comment by @johannes-graner on 2023-11-27 18:07_

> Thanks for contributing!
> 
> ~Should we avoid raising this for code with a return of `None`? There are a lot of cases in the ecosystem checks where a return would be meaningless.~ It looks like you implemented this, actually. Could you take a look at the ecosystem checks and see if they are false positives or correct? This rule is introducing _a lot_ of violations.

I looked through the ecosystem checks and most violations are true positives. Since this rule only checks for docstrings following reST, some violations are for [following other documentation styles](https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/abc.py#L1803).

The only case I would consider potentially problematic is that [private functions are not ignored](https://github.com/apache/airflow/blob/4495de73bc8ede5717e631942477384655a4ce0d/airflow/api/auth/backend/kerberos_auth.py#L105). I'm not quite sure what the best behavior would be in that case, on one hand it makes sense to document private functions too, but on the other hand, private functions are internal affairs where a bit more leeway can be given.

Do you have an opinion on what would be preferable in this case?

---

_Comment by @zanieb on 2023-11-27 18:14_

Unless pylint handles private functions differently, I think we can treat them the same for now. I could imagine a setting to enable or disable documentation rules for private functions in the future.

---

_Comment by @zanieb on 2023-11-27 18:32_

Given the number of projects that opt-in to PLW without using reST docstrings, I'm a little worried about adding this rule. Perhaps there's a way we can shrink the scope to only effect projects using that docstring style? Maybe there's a pattern in https://github.com/astral-sh/ruff/blob/017e82911595bab746172b7976f993b715be4d79/crates/ruff_linter/src/checkers/ast/analyze/definitions.rs that we should follow?

@charliermarsh may have more concrete advice or opinions.

---

_Comment by @johannes-graner on 2023-11-27 18:54_

Turns out the pylint rule ignores private functions, so let's ignore them here as well.

Regarding this only applying to reST docstrings:
An easier solution might be to include support for Google and Numpy styles as well. Pylint does support those docstring styles, but the examples are only with reST docstrings.

---

_Comment by @johannes-graner on 2023-11-30 18:41_

@zanieb I don't think the CI triggered on my last push (there were some issues with GitHub around that time). Can you or someone else trigger the workflow manually, or should I push an empty commit to trigger the workflow?

---

_Comment by @charliermarsh on 2023-11-30 18:43_

@johannes-graner - I went ahead and merged in main + re-pushed :)

---

_Comment by @codspeed-hq[bot] on 2023-11-30 18:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/johannes-graner:pylint-w9011-missing-return-doc)

### Merging #8843 will **not alter performance**

<sub>Comparing <code>johannes-graner:pylint-w9011-missing-return-doc</code> (537aeaa) with <code>main</code> (073eddb)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @Mr-Pepe on 2023-12-04 02:11_

> Perhaps there's a way we can shrink the scope to only effect projects using that docstring style?

The pydocstyle checks have a `convention` setting. Could/should that setting be promoted to a general Ruff setting so other docstring-related checks can account for it?

---

_Comment by @johannes-graner on 2023-12-06 19:19_

> > Perhaps there's a way we can shrink the scope to only effect projects using that docstring style?
> 
> The pydocstyle checks have a `convention` setting. Could/should that setting be promoted to a general Ruff setting so other docstring-related checks can account for it?

That sounds reasonable. However, I think that is out of scope for this PR (and should probably be handled by someone with more contributing experience than me).

Would it be reasonable to simply re-use the pydocstyle convention setting with an additional way to specify it in `[tool.ruff.lint.pylint]`? If I understand the code correctly, one could still specify different conventions in pydocstyle and pylint (although I don't see why anyone would do that!)

---

_Comment by @johannes-graner on 2023-12-06 20:26_

Implemented an option `pylint.convention` that allows setting the docstring convention. If unset, the rule doesn't check anything, which might be a more sane default than checking all conventions.

Should eliminate any ecosystem violations since they would have to opt-in to the rule by setting the convention. This also brings the behavior slightly closer to that of pylint, which requires an explicit flag to check docstrings.

I still think the convention should be a general setting, but since that would involve deprecation of the current pydocstyle convention setting, it's out of scope here and probably should have it's own issue with discussion first.

---

_Comment by @Mr-Pepe on 2023-12-07 07:02_

I opened https://github.com/astral-sh/ruff/issues/9043 to discuss the convention setting. Depending on whether there's quick feedback, the settings should probably be updated before this PR gets merged. Otherwise, the settings introduced here would get deprecated straight away.

---

_Comment by @zanieb on 2024-03-12 18:44_

Sorry for the delay here. It looks like a global setting would be reasonable. @Mr-Pepe are you interested in working on that? We'd probably want to do it separately / before this pull request is merged.

---

_Assigned to @zanieb by @zanieb on 2024-03-12 18:44_

---

_Comment by @Mr-Pepe on 2024-03-13 05:44_

I'm not using Python that much anymore, so I can't really justify the effort, sorry 😔

---

_Comment by @MichaReiser on 2024-03-28 15:24_

Thanks for creating this PR and sorry for the long time it has taken us to move this forward.

We are holding off from adding any new Pylint extension rules until #1774 . The reason for is that using pylint extension rules in pylint requires explicit opt-in. This would be different when using Ruff where the rule would automatically be enabled when you use the `PYL` selector (or `ALL` for what it matters). The alternative is to mirror Pylint's approach, where we create a new group for each extension, but we would rather not do that because it doesn't reflect what rule groups are intended for.

That means we can't accept the rule right now, even though I think the rule itself would be useful. Our goal is to have a better place for this kind of rules once #1774. 

I'm sorry for giving that feedback so late, and I plan to go through open issues (especially the pylint one) to flag rules ahead of time. Thank you again or taking the time to contribute to Ruff. 

---

_Closed by @MichaReiser on 2024-03-28 15:24_

---
