```yaml
number: 16895
title: Isolating test from accessing global git credential helper config
type: pull_request
state: merged
author: sid-38
labels:
  - internal
assignees: []
merged: true
base: main
head: sid/unset-git-cred-helper
created_at: 2025-11-30T01:31:47Z
updated_at: 2025-12-01T11:41:22Z
url: https://github.com/astral-sh/uv/pull/16895
synced_at: 2026-01-10T05:49:14Z
```

# Isolating test from accessing global git credential helper config

---

_Pull request opened by @sid-38 on 2025-11-30 01:31_

## Summary
Resolves: https://github.com/astral-sh/uv/issues/1980

Added a utility function to TestContext called **with_git_credential_helper_blocked** that isolates the test from accessing the credential helper value defined in global/system git config. It does so, by writing to a file .gitconfig in the temporary home_dir that is created as part of the TestContext. 

## Test Plan
Tested it by running the test pip_install::install_git_private_https_pat_and_username, and making sure it doesn't affect the keyring. 

## Note:
The commit hash for the uv-private-package seems to have changed. Kindly, ensure that the modification related to that  is correct.



---

_@samypr100 reviewed on 2025-11-30 04:28_

---

_Review comment by @samypr100 on `crates/uv/tests/it/common/mod.rs`:580 on 2025-11-30 04:28_

We may be able to leverage the existing assert_fs support, e.g.

```suggestion
    pub fn with_unset_git_credential_helper(self) -> Self {
        let git_config = self.home_dir.child(".gitconfig");
        git_config
            .write_str(indoc! {r"
                [credential]
                    helper =
            "})
            .expect("Failed to unset git credential helper");
        self
    }
```

---

_@sid-38 reviewed on 2025-12-01 04:35_

---

_Review comment by @sid-38 on `crates/uv/tests/it/common/mod.rs`:580 on 2025-12-01 04:35_

Yup that works. Made the change

---

_Comment by @codspeed-hq[bot] on 2025-12-01 04:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/sid-38%3Asid%2Funset-git-cred-helper?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16895 will **degrade performances by 22.34%**

<sub>Comparing <code>sid-38:sid/unset-git-cred-helper</code> (edca76d) with <code>main</code> (6d8866a)</sub>



### Summary

`❌ 1` regression  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/sid-38%3Asid%2Funset-git-cred-helper?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/sid-38%3Asid%2Funset-git-cred-helper?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 770.4 ms | 992.1 ms | -22.34% |


---

_@konstin approved on 2025-12-01 11:40_

I tested this both on Windows and Linux.

Thank you!

---

_Label `internal` added by @konstin on 2025-12-01 11:41_

---

_Merged by @konstin on 2025-12-01 11:41_

---

_Closed by @konstin on 2025-12-01 11:41_

---
