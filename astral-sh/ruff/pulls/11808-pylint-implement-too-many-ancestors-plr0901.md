```yaml
number: 11808
title: "[`pylint`] Implement `too-many-ancestors` (`PLR0901`)"
type: pull_request
state: closed
author: Peiffap
labels:
  - rule
assignees: []
base: main
head: PLR0901-too-many-ancestors
created_at: 2024-06-09T01:27:35Z
updated_at: 2024-06-21T17:04:29Z
url: https://github.com/astral-sh/ruff/pull/11808
synced_at: 2026-01-10T21:56:00Z
```

# [`pylint`] Implement `too-many-ancestors` (`PLR0901`)

---

_Pull request opened by @Peiffap on 2024-06-09 01:27_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR adds [`pylint` rule `R0901`](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/too-many-ancestors.html), which restricts the maximum number of parent classes a class can have (to a default value of 7).

## Test Plan

<!-- How was it tested? -->
This PR was tested using the workflow discussed [here](https://docs.astral.sh/ruff/contributing/).

Running tests with `cargo test` gives no failures.

## Related Issues
This is part of the greater effort of #970 (and if merged, should be checkmarked as "done" there).

---

_@Peiffap reviewed on 2024-06-09 01:42_

---

_Review comment by @Peiffap on `crates/ruff_workspace/src/configuration.rs`:1505 on 2024-06-09 01:42_

I'm assuming this wasn't meant to be a duplicate, but for visibility I'm adding this comment.

---

_@Peiffap reviewed on 2024-06-09 01:44_

---

_Review comment by @Peiffap on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__too_many_ancestors.snap`:1 on 2024-06-09 01:44_

Currently, the error message covers the entirety of the class, including the class body. Would it be desirable/easily feasible to only have it cover the "header" instead?

---

_Comment by @github-actions[bot] on 2024-06-09 01:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/81a44faf5c188546cb8e949b233135ac9855df1f/pandas/tests/extension/base/__init__.py#L68'>pandas/tests/extension/base/__init__.py:68:1:</a> PLR0901 Too many ancestors (19 > 7)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0901 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-06-10 06:54_

---

_Label `needs-design` added by @MichaReiser on 2024-06-20 07:46_

---

_Comment by @MichaReiser on 2024-06-20 07:54_

Thanks for contributing this rule. 

I worry that merging the rule as is today will be confusing to users and give them a wrong sense of "safety" where they think the rule would catch all inheritance chains with more than `x` parent classes. However, that's not the case because Ruff doesn't support multifile analysis, meaning Ruff can only reason about the ancestor chain in a single file. 

I think it's more likely that such deep hierarchy chains are split across many files, meaning the rule will miss almost all violations in real projects, such that it has limited value today.

I'll close this PR for now but I think we can add this rule once multifile analysis is supported. 

---

_Closed by @MichaReiser on 2024-06-20 07:54_

---

_Label `multifile-analysis` added by @MichaReiser on 2024-06-20 07:55_

---

_Label `needs-design` removed by @MichaReiser on 2024-06-20 07:55_

---

_Comment by @Peiffap on 2024-06-21 10:03_

@MichaReiser Thanks for taking the time to look over this!

If I'm allowed to push back a little on this... I don't understand your comment at all :(. This PR (and to my understanding, the `pylint` rule associated with it) has nothing to do with the _depth_ of the inheritance chain (which would definitely be an interesting rule to implement once multifile analysis is possible!), but about the _width_ (if you can call it that) of the inheritance, i.e. the number of _direct_ parent classes of a given class.
I'm by no means an expert on object-oriented Python, but to my knowledge this is information that is solely contained in a single file, where the class is defined.

Edit: @Pierre-Sassoulas I see you've looked at this PR, so perhaps you can comment on whether my understanding of the `pylint` rule is correct?

---

_Comment by @MichaReiser on 2024-06-21 10:52_

I'm sorry that I failed to communicate the decision clearly or may even be mistaken. 

I re-read the rule documentation and I think there's some confusion because the documentation sometimes uses the term parents and sometimes ancestors:

* `parents`: The direct parents of a class. This maps to `class_def.bases`
* `ancestors`: The transitive inheritance chain of a class. This requires traversing the entire inheritance chain. 

I took a look at the Pylint implementation, and my understanding is that it asserts that `ancestors` is less than the configured minimum, whereas this implementation implements `parents`. 

https://github.com/pylint-dev/pylint/blob/087d77adcd8455dbbbf6b43631d93abb2d1c5d67/pylint/checkers/design_analysis.py#L245-L279

We'll need multifile analysis if we want to implement Pylint's behavior (assuming I understand it correctly)

---

_Comment by @Peiffap on 2024-06-21 12:41_

Thanks for giving some more detail! Apologies if I came across as overly defensive.

I agree with you that the documentation is a little confusing and mixes terms, and I wasn't even aware myself that "parent" and "ancestor" refer to different things!
I'm not familiar enough with the ruff codebase to know whether multifile analysis is required to check all ancestors, so I'll take your word for it.
How would you feel about slightly refactoring this PR to implement a new "`too-many-parents`" rule which clearly explains the difference, and adding a comment to #970 specifying that `R0901` shouldn't be worked on until multifile analysis is available?

---

_Comment by @MichaReiser on 2024-06-21 12:47_

> Apologies if I came across as overly defensive.

No worries. It was good that you asked for clarification because I honestly wasn't aware of what specific behavior PyLInt implements (I just assumed ancestor without checking). 

> I'm not familiar enough with the ruff codebase to know whether multifile analysis is required to check all ancestors, so I'll take your word for it.

We could check all ancestors already today, but only if all classes are defined in the same file. That's unlikely to be the case for many classes and significantly reduces the added value of the rule.

> How would you feel about slightly refactoring this PR to implement a new "too-many-parents" rule which clearly explains the difference, 

I prefer to wait until Ruff supports multifile-analysis to avoid that we have to deprecate this rule just in a few months because it overlaps with an other rule. I also think that only checking direct parents isn't that useful, at least not in the spirit the rule is defined today (you could argue that inheriting from more than one base is considered bad). 

> and adding a comment to https://github.com/astral-sh/ruff/issues/970 specifying that R0901 shouldn't be worked on until multifile analysis is available?

Yeah, let's do that

---

_Comment by @Peiffap on 2024-06-21 13:00_

That's all reasonable; thanks for getting back to me so swiftly! I'll add the comment as discussed.

---

_Comment by @Pierre-Sassoulas on 2024-06-21 17:04_

I agree with @MichaReiser about pylint behavior. I reacted on the comment because I wasn't aware that ruff had this single file limitation (same than flake8 ?), so it was interesting to me. 

---
