```yaml
number: 13116
title: "Format `PYI` examples in docs as `.pyi`-file snippets"
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: format-pyi-in-docs
created_at: 2024-08-26T21:57:07Z
updated_at: 2024-08-28T12:29:09Z
url: https://github.com/astral-sh/ruff/pull/13116
synced_at: 2026-01-10T21:38:32Z
```

# Format `PYI` examples in docs as `.pyi`-file snippets

---

_Pull request opened by @calumy on 2024-08-26 21:57_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Now that docs are formatted by the Ruff formatter, https://github.com/astral-sh/ruff/pull/13087. It is also possible to format `pyi`-style examples with the `pyi`-style format fixing https://github.com/astral-sh/ruff/issues/11568.

One concern I have with this update is that syntax highlighting is currently not implemented in MK docs for PYI examples. Therefore, we need to decide if it is better that the docs are formatted correctly or if syntax highlighting is more important. If the latter is the case, I can investigate this further and draft this PR for now. 

## Test Plan

- CI to check that docs are formatted correctly

CC @AlexWaygood, I would appreciate it if you could review this PR as you opened the issue that prompted this update. I think it would be useful to sense-check the examples that I updated to PYI, as well as let me know and further examples that would benefit from `pyi` style formatting and I can update as required. 


---

_Review requested from @AlexWaygood by @calumy on 2024-08-26 21:57_

---

_Converted to draft by @calumy on 2024-08-26 21:59_

---

_Marked ready for review by @calumy on 2024-08-26 22:01_

---

_Comment by @github-actions[bot] on 2024-08-26 22:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
```

</p>
</details>




---

_Review comment by @AlexWaygood on `scripts/check_docs_formatted.py`:161 on 2024-08-27 13:05_

Could consider using pattern-matching here:

```suggestion
        match language:
            case "python":
                extension = "py"
            case "pyi":
                extension = "pyi"
            case _:
                # We are only interested in checking the formatting of py or pyi code blocks
                # so we can return early if the language is not one of these.
                return f'{match["before"]}{match["code"]}{match["after"]}'
```

---

_@AlexWaygood reviewed on 2024-08-27 13:12_

Nice, this looks great!

>One concern I have with this update is that syntax highlighting is currently not implemented in MK docs for PYI examples.

Hmm, that is a shame (and might indeed be a blocker here, sadly ☹️). Did you find this out from reading the docs, or by experimenting? I can't immediately find anywhere where this is documented :/

---

_Comment by @calumy on 2024-08-27 21:49_

> Nice, this looks great!
> 
> > One concern I have with this update is that syntax highlighting is currently not implemented in MK docs for PYI examples.
> 
> Hmm, that is a shame (and might indeed be a blocker here, sadly ☹️). Did you find this out from reading the docs, or by experimenting? I can't immediately find anywhere where this is documented :/

This was found with experimentation. Hopefully, the lack of highlighting of `.pyi` files should be fixed with [this PR](https://github.com/pygments/pygments/pull/2773) and a subsequent release of pygments. 

I tested this by installing locally with `pip install git+https://github.com/calumy/pygments.git@add-pyi-short-name` (sorry, I have not had a chance to use UV yet). Then running `mkdocs serve -f mkdocs.public.yml` and checking one of the updated pyi docs, e.g. http://127.0.0.1:8000/ruff/rules/unprefixed-type-param/. Which resulted in the following: 
![image](https://github.com/user-attachments/assets/84a59f62-6682-42a6-bbde-a6e62093fd70)

Do you know of any other places where `.pyi` file syntax highlighting is missing that would be a blocker?

Should I draft until the next release of pygments or would you consider pinning to a commit of pygments in the requirements? If so, which requirements file would be best for this, as there appear to be a couple in the docs directory?

---

_Comment by @AlexWaygood on 2024-08-27 21:57_

> Hopefully, the lack of highlighting of `.pyi` files should be fixed with [this PR](https://github.com/pygments/pygments/pull/2773) and a subsequent release of pygments.

Woah, nice! Thanks so much for figuring out the root cause there and filing a PR to solve it — that's awesome 😃

(It's late here so I'll get to your other questions tomorrow :)

---

_Label `documentation` added by @dhruvmanila on 2024-08-28 05:20_

---

_Comment by @calumy on 2024-08-28 11:54_

https://github.com/pygments/pygments/pull/2773 has been merged and added to the next release milestone. I am unsure of the schedule of the next release of pygments.

---

_Comment by @AlexWaygood on 2024-08-28 11:57_

> [pygments/pygments#2773](https://github.com/pygments/pygments/pull/2773) has been merged and added to the next release milestone.

I saw! I'm currently looking into whether it's feasible for us to pin pygments to a commit on their `main` branch -- if so, we can land this immediately. If not, we'll need to wait. I'll get back to you soon.

If we do need to wait, we should pick up the latest release of pygments in CI as soon as it's released. The depdendency comes from `mkdocs-material`'s `pyproject.toml` file here: https://github.com/squidfunk/mkdocs-material/blob/6f3c05b6fa7e42279ea7604aedc4e4333131fe57/requirements.txt#L26. And the dependency is permissive enough that it should allow `pygments==2.19.0` as soon as it's released.

---

_Comment by @AlexWaygood on 2024-08-28 12:11_

I think pinning to a `main` branch commit works -- I pushed the new pins to this branch, so we should be good to go. Thanks again for this -- really great contribution 😃

---

_Renamed from "Format `PYI` files in docs correctly" to "Format `PYI` examples in docs as `.pyi`-file snippets" by @AlexWaygood on 2024-08-28 12:20_

---

_Merged by @AlexWaygood on 2024-08-28 12:20_

---

_Closed by @AlexWaygood on 2024-08-28 12:20_

---
