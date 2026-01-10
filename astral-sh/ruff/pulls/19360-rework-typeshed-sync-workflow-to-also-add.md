```yaml
number: 19360
title: Rework typeshed-sync workflow to also add docstrings for Windows- and MacOS-specific APIs
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/platform-api-docs
created_at: 2025-07-15T16:08:43Z
updated_at: 2025-07-15T17:21:21Z
url: https://github.com/astral-sh/ruff/pull/19360
synced_at: 2026-01-10T17:58:13Z
```

# Rework typeshed-sync workflow to also add docstrings for Windows- and MacOS-specific APIs

---

_Pull request opened by @AlexWaygood on 2025-07-15 16:08_

## Summary

The docstring-adder codemod can only add docstrings for APIs that exist at runtime. I.e., if the codemod is run on a Linux machine, it can't add any docstrings for APIs that don't exist at runtime on a Linux machine. The Python stdlib has some APIs that only exist on Windows/MacOS, and it seems a shame not to provide docstrings for those too.

This PR reworks the `sync_typeshed.yaml` workflow so that:
1. A Linux worker:
   a. Syncs the typeshed stubs
   b. Syncs the docstrings available on Linux
   c. Commits the changes and pushes them to an upstream branch
2. Once the Linux worker is done, a Windows worker:
   a. Checks out the branch created by the Linux worker
   b. Syncs all docstrings available on Windows that are not available on Linux
   c. Commits the changes and pushes them to the same upstream branch
3. Once the Windows worker is done, a MacOS worker:
   a. Checks out the branch created by the Linux worker
   b. Syncs all docstrings available on MacOS that are not available on Linux or Windows
   c. Commits the changes and pushes them to the same upstream branch
   d. Creates the typeshed-sync PR

This PR also updates the pinned commit of docstring-adder to https://github.com/astral-sh/docstring-adder/commit/7f350b03ee83dd44ebd8010228ad3dfca34a7887, which includes https://github.com/astral-sh/docstring-adder/pull/2

## Test Plan

`¯\_(ツ)_/¯`

I ran pre-commit, and ran the new `codemod_docstrings.sh` script through shellcheck. Not sure how else to test this other than by merging it and seeing if it works?


---

_Review requested from @MichaReiser by @AlexWaygood on 2025-07-15 16:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-15 16:08_

---

_Label `ci` added by @AlexWaygood on 2025-07-15 16:08_

---

_Label `ty` added by @AlexWaygood on 2025-07-15 16:08_

---

_Comment by @github-actions[bot] on 2025-07-15 16:18_

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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Review comment by @MichaReiser on `.github/workflows/sync_typeshed.yaml`:100 on 2025-07-15 16:25_

Will this fail if the changes are empty?

---

_@MichaReiser reviewed on 2025-07-15 16:25_

---

_@AlexWaygood reviewed on 2025-07-15 16:28_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:100 on 2025-07-15 16:28_

Yes. I don't expect that to ever be the case, though, due to modules like https://github.com/python/cpython/blob/main/Lib/asyncio/windows_events.py that can't be imported at all on non-Windows (but which this job will add docstrings for). So I sort-of feel like it would be a feature if this step failed because no changes had been made -- it indicates something weird is going on.

I could add a comment?

---

_@AlexWaygood reviewed on 2025-07-15 16:32_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:100 on 2025-07-15 16:32_

or I could just add `--allow-empty`, I don't have a strong opinion

---

_Review comment by @MichaReiser on `.github/workflows/sync_typeshed.yaml`:100 on 2025-07-15 16:37_

It's not quiet clear how this workflow works. To which repository is it commiting to? If it's the ruff repository: Are these commits squashed later?


For testing: Have you considered just triggering a typeshed run. Shouldn't that the updated typeshed files

---

_@MichaReiser reviewed on 2025-07-15 16:38_

---

_@MichaReiser reviewed on 2025-07-15 16:40_

---

_Review comment by @MichaReiser on `.github/workflows/sync_typeshed.yaml`:100 on 2025-07-15 16:40_

Okay I think I understand how it works. I think I would primarily make it consistent to `git commit -am "Sync macOS docstrings" --allow-empty`

It might also be helpful to add some comment at the top of the workflow file explaining what's going on in this workflow :) It's not very obvious hehe

---

_@AlexWaygood reviewed on 2025-07-15 16:42_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:100 on 2025-07-15 16:42_

> It's not quiet clear how this workflow works. To which repository is it commiting to?

It creates a branch on the upstream Ruff repository and pushes the commits to that branch. It then creates a PR from that branch to the `main` branch. That's what we already do on `main`; the new bit is that workers on Windows and MacOS now pick up where the Linux worker left off before a PR is made

> Are these commits squashed later?

The commits are not squashed but we squash-merge all PRs, so I don't think that matters. If anything, I'd quite like to see the separate steps as individual commits; it would make it easier to verify that the workflow is working as expected.

> For testing: Have you considered just triggering a typeshed run. Shouldn't that the updated typeshed files

Yes, absolutely. But I think I can only do that once this is merged?

---

_@AlexWaygood reviewed on 2025-07-15 16:43_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:100 on 2025-07-15 16:43_

> It might also be helpful to add some comment at the top of the workflow file explaining what's going on in this workflow :) It's not very obvious hehe

yeah for sure!

---

_@MichaReiser approved on 2025-07-15 16:50_

The git thingy where you make commits to carry over the docstrings is a bit hard to understand. I think using something like the upload files action might have been slightly easier. But I don't think it's worth changing, but some documentation at the top (e.g. take the PR summary) would be great


```
      - name: Upload generated files
        uses: actions/upload-artifact@v4
        with:
          name: files-${{ matrix.os }}
          path: |
            path/to/generated/files/
            !path/to/exclude/
          retention-days: 1
```


Don't dare to break the typeshed sync and then be off for a week ;)

---

_Merged by @AlexWaygood on 2025-07-15 17:14_

---

_Closed by @AlexWaygood on 2025-07-15 17:14_

---

_Branch deleted on 2025-07-15 17:14_

---

_Comment by @MichaReiser on 2025-07-15 17:20_


I think you should have been able to run it like this:
<img width="2040" height="892" alt="Screenshot 2025-07-15 at 19 19 36" src="https://github.com/user-attachments/assets/66182898-c551-46e2-9c36-367b640a145e" />


---

_Comment by @AlexWaygood on 2025-07-15 17:21_

Hmm, I'll try that on #19367

---
