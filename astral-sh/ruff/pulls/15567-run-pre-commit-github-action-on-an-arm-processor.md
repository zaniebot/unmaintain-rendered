```yaml
number: 15567
title: Run pre-commit GitHub Action on an ARM processor
type: pull_request
state: closed
author: cclauss
labels:
  - ci
assignees: []
base: main
head: patch-1
created_at: 2025-01-18T07:30:39Z
updated_at: 2025-01-18T15:21:44Z
url: https://github.com/astral-sh/ruff/pull/15567
synced_at: 2026-01-12T15:55:51Z
```

# Run pre-commit GitHub Action on an ARM processor

---

_@cclauss_

Speed up a ~3 minute GitHub Action by running it on an ARM processor. https://docs.github.com/en/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources

@dhruvmanila Your review, please.
Blocked by:
* rhysd/actionlint#507
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-01-18 07:39_

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
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
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
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_Comment by @MichaReiser on 2025-01-18 14:08_

Thanks for looking into this. The pre-commit currently fails and I don't see a speedup. E.g. https://github.com/astral-sh/ruff/pull/15566 completed after 3 min where the run on this job failed after 4. 

---

_Label `ci` added by @MichaReiser on 2025-01-18 14:08_

---

_Comment by @cclauss on 2025-01-18 15:07_

Can you please rerun the failing test because rhysd/actionlint#507 seems to pass.

The hooks seem to be recreated with each run so perhaps the network io time dwarfs the faster CPU.

---

_Comment by @MichaReiser on 2025-01-18 15:15_

I restarted the job but I don't think it will succeed because it seems to error because our github workflow linter doesn't recognize the new runner yet. We probably need to update it first.

---

_Comment by @cclauss on 2025-01-18 15:21_

Understood.  Thanks for the re-try.  We will need to wait for the ecosystem to catch up.

---

_Closed by @cclauss on 2025-01-18 15:21_

---

_Branch deleted on 2025-01-18 15:21_

---
