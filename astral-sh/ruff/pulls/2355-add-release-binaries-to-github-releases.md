```yaml
number: 2355
title: Add release binaries to GitHub releases
type: pull_request
state: closed
author: rahul-theorem
labels: []
assignees: []
draft: true
base: main
head: rm/release-binary
created_at: 2023-01-30T15:41:21Z
updated_at: 2023-02-15T17:07:50Z
url: https://github.com/astral-sh/ruff/pull/2355
synced_at: 2026-01-12T15:55:08Z
```

# Add release binaries to GitHub releases

---

_@rahul-theorem_

Closes #2330

Adds release binaries to the corresponding GH release.

---

_Review comment by @rahul-theorem on `.github/workflows/ruff.yaml`:51 on 2023-01-30 15:56_

@messense if you have any pointers how to properly name the release binary, let me know

---

_@rahul-theorem reviewed on 2023-01-30 15:56_

---

_Review comment by @rahul-theorem on `.github/workflows/ruff.yaml`:360 on 2023-01-30 15:57_

@charliermarsh since the action itself isn't creating the release (ie. after a tag is pushed by some other automation), this seemed like the easiest way to get the latest release name

---

_@rahul-theorem reviewed on 2023-01-30 15:57_

---

_@charliermarsh reviewed on 2023-01-30 18:19_

---

_Review comment by @charliermarsh on `.github/workflows/ruff.yaml`:360 on 2023-01-30 18:19_

Can we somehow get this from `github.ref`, similar to how the `if: "startsWith(github.ref, 'refs/tags/')"` operation works above?

---

_@messense reviewed on 2023-01-31 01:43_

---

_Review comment by @messense on `.github/workflows/ruff.yaml`:51 on 2023-01-31 01:43_

Upload the binary directly will lose its file permission: https://github.com/actions/upload-artifact#permission-loss

I'd create an `tar.gz`/`zip` archive for each platform, and please also include a `sha256` checksum file. For reference: https://github.com/rust-cross/cargo-zigbuild/blob/a079e8e11235064a9ee27d44ef21128fd5a23ce2/.github/workflows/Release.yml#L34-L44

---

_Comment by @charliermarsh on 2023-02-01 21:50_

Possibly relevant: https://github.com/axodotdev/cargo-dist

---

_Comment by @charliermarsh on 2023-02-04 14:32_

I'm now thinking that we should just copy [ripgrep's release workflow](https://github.com/BurntSushi/ripgrep/blob/master/.github/workflows/release.yml), except instead of creating the GitHub Release, it should be triggered by the creation of a GitHub Release.

---

_Closed by @charliermarsh on 2023-02-15 17:07_

---
