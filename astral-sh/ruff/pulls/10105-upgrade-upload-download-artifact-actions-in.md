```yaml
number: 10105
title: Upgrade upload/download artifact actions in release workflow
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: release-upload-download-artifact
created_at: 2024-02-23T20:26:29Z
updated_at: 2024-02-26T07:23:11Z
url: https://github.com/astral-sh/ruff/pull/10105
synced_at: 2026-01-10T22:57:10Z
```

# Upgrade upload/download artifact actions in release workflow

---

_Pull request opened by @MichaReiser on 2024-02-23 20:26_

## Summary

This PR upgrades the upload and download artifact GitHub actions to v4 for the release workflow. 

Upgrading the release workflow isn't as straightforward because it relies on the fact that the v3 action automatically merges artifacts with the same name. 

The existing release workflow merges all `wheels` and `binaries` into two artifacts and downloads these in the publish step.  There are two ways of how we can implement this with the new actions where one is safer and the other avoids storing the artifacts twice:

We can either be bold and use 

```yaml
      - uses: actions/download-artifact@v4
        with:
          pattern: binaries-*
          path: binaries
          merge-multiple: true
```

This should download all `binaries` artifacts and merge them in the `binaries` path. See [the action example](https://github.com/actions/download-artifact/tree/main?tab=readme-ov-file#download-multiple-filtered-artifacts-to-the-same-directory). 

That's what the current implementation uses, but I can't test the workflow without shipping a release... 

The safer approach (testable) is to use one additional step that merges all artifacts and reuploads them as a single archive:

```yaml
  merge-artifact:
    name: Merge Wheel and Bianaries
    runs-on: ubuntu-latest
    needs:
      - macos-universal
      - macos-x86_64
      - windows
      - linux
      - linux-cross
      - musllinux
      - musllinux-cross
    steps:
      - name: Merge Wheels
        uses: actions/upload-artifact/merge@v4
        with:
          name: wheels
          pattern: wheels-*
      - name: Merge Binaries
        uses: actions/upload-artifact/merge@v4
        with:
          name: binaries
          pattern: binaries-*
```

I verified that the step produces the same artifact as the existing workflow. However, it means we upload each artifact twice: Once as its own archive and once as a merged archive. 


I went with the first approach.... With the risk that we have to fix it during the next release.


Closes of https://github.com/astral-sh/ruff/issues/9527

## Test Plan



---

_Renamed from "Upgrade upload/download artifact actions in CI.yaml workflow" to "Upgrade upload/download artifact actions in release workflow" by @MichaReiser on 2024-02-23 20:26_

---

_@charliermarsh approved on 2024-02-23 20:28_

Nice, thanks for doing this.

---

_Comment by @MichaReiser on 2024-02-23 20:30_

Lol wait no... I'm not done yet and there's some discussion to be had. 

---

_Comment by @charliermarsh on 2024-02-23 20:32_

Ship it

---

_Comment by @zanieb on 2024-02-23 20:50_

Also https://github.com/astral-sh/uv/issues/1943

---

_Comment by @github-actions[bot] on 2024-02-23 20:54_

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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-23 20:57_

---

_Review requested from @zanieb by @MichaReiser on 2024-02-23 20:57_

---

_Review requested from @konstin by @MichaReiser on 2024-02-23 20:57_

---

_Label `ci` added by @MichaReiser on 2024-02-23 20:57_

---

_Comment by @MichaReiser on 2024-02-23 20:57_

> Also [astral-sh/uv#1943](https://github.com/astral-sh/uv/issues/1943)

Let's see how this goes first :laughing: 

---

_Marked ready for review by @MichaReiser on 2024-02-23 20:57_

---

_@charliermarsh approved on 2024-02-25 22:00_

If the release breaks, so be it -- we'll debug it live.

---

_Merged by @MichaReiser on 2024-02-26 07:23_

---

_Closed by @MichaReiser on 2024-02-26 07:23_

---

_Branch deleted on 2024-02-26 07:23_

---
