```yaml
number: 13762
title: "Move Python upgrade functionality behind `--preview` flag"
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/upgrade-behind-preview
created_at: 2025-06-01T20:58:03Z
updated_at: 2025-06-10T18:22:29Z
url: https://github.com/astral-sh/uv/pull/13762
synced_at: 2026-01-10T11:10:42Z
```

# Move Python upgrade functionality behind `--preview` flag

---

_Pull request opened by @jtfmumm on 2025-06-01 20:58_

Because the required changes for transparent Python upgrades (#13312, #13712, and #13531) have a significant impact on how uv installs and executes managed Python executables, this PR moves these changes behind a `--preview` flag and updates the relevant docs. 

Most of the changes here are threading `PreviewMode` through new places in the code. The core of the change is to check the `PreviewMode` in `DirectorySymlink::from_executable`. When preview is disabled, this method will return `None`, causing calling code to use the existing behavior instead of the new behavior supporting transparent upgrades. 

Once an installation has been made upgradeable through `uv python install --preview` or `uv python upgrade --preview`, future operations do not require the `--preview` flag. For example, `uv venv -p 3.10` will create an upgradeable venv and `uv python install 3.10.18` will upgrade it if it had started on a lower patch version. This is accomplished by checking for the existence of the relevant symlink directory when executing operations.

Any tests that check transparent upgrade behavior in `python_upgrade.rs` and `python_install.rs` have been updated to make use of the `--preview` flag.


---

_@jtfmumm reviewed on 2025-06-01 20:59_

---

_Review comment by @jtfmumm on `crates/uv-dev/src/compile.rs`:29 on 2025-06-01 20:59_

It looks like in `uv-dev` we don't currently parse the preview flag. At the moment I am passing `PreviewMode::Disabled` directly here. How do we want to handle the preview flag in this context?

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_list.rs`:452 on 2025-06-01 21:07_

We no longer list a minor version (corresponding to a symlink directory) without the `--preview` flag.

---

_@jtfmumm reviewed on 2025-06-01 21:07_

---

_@zanieb reviewed on 2025-06-02 13:06_

---

_Review comment by @zanieb on `crates/uv-dev/src/compile.rs`:29 on 2025-06-02 13:06_

I don't think we really need it. I don't use the `uv-dev` commands anymore, personally.

---

_Marked ready for review by @jtfmumm on 2025-06-02 13:13_

---

_Review comment by @konstin on `docs/guides/install-python.md`:125 on 2025-06-05 14:18_

Can you move the preview hint out to a `!!! note`?

---

_Review comment by @konstin on `crates/uv-virtualenv/src/virtualenv.rs`:181 on 2025-06-05 14:39_

We need to make sure that we update those comments when stabilizing the feature

---

_Review comment by @konstin on `crates/uv/tests/it/python_upgrade.rs`:223 on 2025-06-05 14:40_

Where did that test case go?

---

_@konstin approved on 2025-06-05 14:44_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_upgrade.rs`:223 on 2025-06-05 15:01_

I thought I had a comment on this one. This test no longer tests what we want because now that I've put upgrades behind preview, a `--preview` installation uses the new behavior from the upgrade changes, which is to install the latest patch version when a minor version is installed. However, in the `python_install_preview` test, we now check that the preview install installs a symlink to the upgradeable symlink directory.

---

_@jtfmumm reviewed on 2025-06-05 15:01_

---

_@jtfmumm reviewed on 2025-06-05 15:26_

---

_Review comment by @jtfmumm on `docs/guides/install-python.md`:125 on 2025-06-05 15:26_

Done

---

_Comment by @zanieb on 2025-06-05 16:11_

(Before we iterate too much here I want to review and make sure we're aligned that this is the right approach for rollout)

---

_Comment by @jtfmumm on 2025-06-05 17:31_

> (Before we iterate too much here I want to review and make sure we're aligned that this is the right approach for rollout)

Sure. There haven’t been any changes yet except a minor one to the docs

---

_Comment by @jtfmumm on 2025-06-09 19:10_

I've updated the code, tests, and PR description to only require the `--preview` flag on `uv python upgrade` and `uv python install` to create an upgradeable installation. Subsequent operations, such as `uv python uninstall` or `uv venv -p 3.10`, will check for and use the symlink directory if it exists.

---

_Comment by @jtfmumm on 2025-06-10 18:07_

Merging into feature branch as part of the PR stack (as described in [this comment](https://github.com/astral-sh/uv/pull/13312#issuecomment-2960012218)).

---

_Merged by @jtfmumm on 2025-06-10 18:08_

---

_Closed by @jtfmumm on 2025-06-10 18:08_

---

_Branch deleted on 2025-06-10 18:08_

---

_Comment by @codspeed-hq[bot] on 2025-06-10 18:22_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Fupgrade-behind-preview)

### Merging #13762 will **improve performances by 16.67%**

<sub>Comparing <code>jtfm/upgrade-behind-preview</code> (3e01986) with <code>main</code> (f20a25f)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 112 ns | 96 ns | +16.67% |


---
