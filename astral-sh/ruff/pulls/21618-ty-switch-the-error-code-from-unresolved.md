```yaml
number: 21618
title: "[ty] Switch the error code from `unresolved-attribute` to `possibly-missing-attribute` for submodules that may not be available"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/submodule-attr-code
created_at: 2025-11-24T19:04:45Z
updated_at: 2025-11-25T07:54:09Z
url: https://github.com/astral-sh/ruff/pull/21618
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Switch the error code from `unresolved-attribute` to `possibly-missing-attribute` for submodules that may not be available

---

_Pull request opened by @AlexWaygood on 2025-11-24 19:04_

## Summary

This is one of the two suggestions in https://github.com/astral-sh/ty/issues/1623. Empirically, these attributes often _are_ available even when they "morally" shouldn't be, and it's confusing to users when we report diagnostics on things that "work fine" at runtime. `possibly-missing-attribute` therefore seems a better fit than `unresolved-attribute`, since it better conveys that this diagnostic isn't one we're certain about, and since that error code has `"warn"` level by default rather than `"error"` level by default.

## Test Plan

mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-11-24 19:04_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-24 19:04_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 19:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-24 19:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- kornia/utils/download.py:99:20: error[unresolved-attribute] Submodule `error` may not be available as an attribute on module `urllib`
+ kornia/utils/download.py:99:20: warning[possibly-missing-attribute] Submodule `error` may not be available as an attribute on module `urllib`

isort (https://github.com/pycqa/isort)
- isort/place.py:104:31: error[unresolved-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`
+ isort/place.py:104:31: warning[possibly-missing-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/tests/test_main.py:87:31: error[unresolved-attribute] Submodule `master` may not be available as an attribute on module `bandersnatch`
+ src/bandersnatch/tests/test_main.py:87:31: warning[possibly-missing-attribute] Submodule `master` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:169:25: error[unresolved-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
+ src/bandersnatch/tests/test_verify.py:169:25: warning[possibly-missing-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:237:25: error[unresolved-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
+ src/bandersnatch/tests/test_verify.py:237:25: warning[possibly-missing-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:238:25: error[unresolved-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
+ src/bandersnatch/tests/test_verify.py:238:25: warning[possibly-missing-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`

asynq (https://github.com/quora/asynq)
- asynq/tests/test_mock.py:242:28: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `asynq`
+ asynq/tests/test_mock.py:242:28: warning[possibly-missing-attribute] Submodule `tests` may not be available as an attribute on module `asynq`
- asynq/tests/test_mock.py:244:23: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `asynq`
+ asynq/tests/test_mock.py:244:23: warning[possibly-missing-attribute] Submodule `tests` may not be available as an attribute on module `asynq`
- asynq/tests/test_mock.py:246:28: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `asynq`
+ asynq/tests/test_mock.py:246:28: warning[possibly-missing-attribute] Submodule `tests` may not be available as an attribute on module `asynq`

spack (https://github.com/spack/spack)
- lib/spack/spack/ci/gitlab.py:148:22: error[unresolved-attribute] Submodule `parse` may not be available as an attribute on module `urllib`
+ lib/spack/spack/ci/gitlab.py:148:22: warning[possibly-missing-attribute] Submodule `parse` may not be available as an attribute on module `urllib`

pip (https://github.com/pypa/pip)
- src/pip/_internal/models/link.py:134:11: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
- src/pip/_internal/models/link.py:134:39: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ src/pip/_internal/models/link.py:134:11: warning[possibly-missing-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ src/pip/_internal/models/link.py:134:39: warning[possibly-missing-attribute] Submodule `request` may not be available as an attribute on module `urllib`
- src/pip/_internal/operations/install/wheel.py:613:16: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ src/pip/_internal/operations/install/wheel.py:613:16: warning[possibly-missing-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- src/pip/_internal/utils/logging.py:294:5: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `logging`
+ src/pip/_internal/utils/logging.py:294:5: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `logging`
- src/pip/_vendor/urllib3/util/retry.py:378:32: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `email`
- src/pip/_vendor/urllib3/util/retry.py:388:26: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `email`
+ src/pip/_vendor/urllib3/util/retry.py:378:32: warning[possibly-missing-attribute] Submodule `utils` may not be available as an attribute on module `email`
+ src/pip/_vendor/urllib3/util/retry.py:388:26: warning[possibly-missing-attribute] Submodule `utils` may not be available as an attribute on module `email`

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/mark_for_deployment.py:1413:15: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
- paasta_tools/cli/cmds/mark_for_deployment.py:1462:15: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ paasta_tools/cli/cmds/mark_for_deployment.py:1413:15: warning[possibly-missing-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ paasta_tools/cli/cmds/mark_for_deployment.py:1462:15: warning[possibly-missing-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
- paasta_tools/cli/cmds/mark_for_deployment.py:1856:14: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ paasta_tools/cli/cmds/mark_for_deployment.py:1856:14: warning[possibly-missing-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
- paasta_tools/envoy_tools.py:118:42: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
- paasta_tools/envoy_tools.py:119:43: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
+ paasta_tools/envoy_tools.py:118:42: warning[possibly-missing-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
+ paasta_tools/envoy_tools.py:119:43: warning[possibly-missing-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
- paasta_tools/smartstack_tools.py:84:38: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
- paasta_tools/smartstack_tools.py:85:39: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
+ paasta_tools/smartstack_tools.py:84:38: warning[possibly-missing-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
+ paasta_tools/smartstack_tools.py:85:39: warning[possibly-missing-attribute] Submodule `adapters` may not be available as an attribute on module `requests`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_commands.py:46:59: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `scrapy`
+ tests/test_commands.py:46:59: warning[possibly-missing-attribute] Submodule `settings` may not be available as an attribute on module `scrapy`
- tests/test_feedexport.py:1348:29: error[unresolved-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`
- tests/test_feedexport.py:1352:29: error[unresolved-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`
+ tests/test_feedexport.py:1348:29: warning[possibly-missing-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`
+ tests/test_feedexport.py:1352:29: warning[possibly-missing-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:144:31: error[unresolved-attribute] Submodule `main` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:144:31: warning[possibly-missing-attribute] Submodule `main` may not be available as an attribute on module `_pytest`
- src/_pytest/python.py:1692:20: error[unresolved-attribute] Submodule `_code` may not be available as an attribute on module `_pytest`
+ src/_pytest/python.py:1692:20: warning[possibly-missing-attribute] Submodule `_code` may not be available as an attribute on module `_pytest`
- testing/test_assertrewrite.py:1246:18: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- testing/test_assertrewrite.py:1396:17: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- testing/test_assertrewrite.py:1904:14: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ testing/test_assertrewrite.py:1246:18: warning[possibly-missing-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ testing/test_assertrewrite.py:1396:17: warning[possibly-missing-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ testing/test_assertrewrite.py:1904:14: warning[possibly-missing-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- testing/test_assertrewrite.py:2084:13: error[unresolved-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
+ testing/test_assertrewrite.py:2084:13: warning[possibly-missing-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
- testing/test_assertrewrite.py:2111:13: error[unresolved-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
+ testing/test_assertrewrite.py:2111:13: warning[possibly-missing-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2313:30: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ testing/test_config.py:2313:30: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2496:18: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ testing/test_config.py:2496:18: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2497:21: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ testing/test_config.py:2497:21: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `_pytest`

porcupine (https://github.com/Akuli/porcupine)
- porcupine/settings.py:732:72: error[unresolved-attribute] Submodule `ttk` may not be available as an attribute on module `tkinter`
+ porcupine/settings.py:732:72: warning[possibly-missing-attribute] Submodule `ttk` may not be available as an attribute on module `tkinter`
- porcupine/utils.py:570:22: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`
+ porcupine/utils.py:570:22: warning[possibly-missing-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`
- porcupine/utils.py:571:6: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`
+ porcupine/utils.py:571:6: warning[possibly-missing-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`
- porcupine/utils.py:638:12: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`
+ porcupine/utils.py:638:12: warning[possibly-missing-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/utils.py:627:16: error[unresolved-attribute] Submodule `pool` may not be available as an attribute on module `multiprocessing`
+ sockeye/utils.py:627:16: warning[possibly-missing-attribute] Submodule `pool` may not be available as an attribute on module `multiprocessing`
- test/unit/test_knn.py:67:20: error[unresolved-attribute] Submodule `encoder` may not be available as an attribute on module `sockeye`
+ test/unit/test_knn.py:67:20: warning[possibly-missing-attribute] Submodule `encoder` may not be available as an attribute on module `sockeye`
- test/unit/test_knn.py:68:22: error[unresolved-attribute] Submodule `encoder` may not be available as an attribute on module `sockeye`
+ test/unit/test_knn.py:68:22: warning[possibly-missing-attribute] Submodule `encoder` may not be available as an attribute on module `sockeye`
- test/unit/test_knn.py:75:19: error[unresolved-attribute] Submodule `data_io` may not be available as an attribute on module `sockeye`
+ test/unit/test_knn.py:75:19: warning[possibly-missing-attribute] Submodule `data_io` may not be available as an attribute on module `sockeye`
- test/unit/test_knn.py:79:14: error[unresolved-attribute] Submodule `model` may not be available as an attribute on module `sockeye`
+ test/unit/test_knn.py:79:14: warning[possibly-missing-attribute] Submodule `model` may not be available as an attribute on module `sockeye`
- test/unit/test_knn.py:88:17: error[unresolved-attribute] Submodule `model` may not be available as an attribute on module `sockeye`
+ test/unit/test_knn.py:88:17: warning[possibly-missing-attribute] Submodule `model` may not be available as an attribute on module `sockeye`
- test/unit/test_utils.py:434:2: error[unresolved-attribute] Submodule `mock` may not be available as an attribute on module `unittest`
+ test/unit/test_utils.py:434:2: warning[possibly-missing-attribute] Submodule `mock` may not be available as an attribute on module `unittest`

PyGithub (https://github.com/PyGithub/PyGithub)
- github/AuthenticatedUser.py:609:16: error[unresolved-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:774:41: error[unresolved-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:807:41: error[unresolved-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:955:30: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:968:13: error[unresolved-attribute] Submodule `Installation` may not be available as an attribute on module `github`
+ github/AuthenticatedUser.py:609:16: warning[possibly-missing-attribute] Submodule `Project` may not be available as an attribute on module `github`
+ github/AuthenticatedUser.py:774:41: warning[possibly-missing-attribute] Submodule `Label` may not be available as an attribute on module `github`
+ github/AuthenticatedUser.py:807:41: warning[possibly-missing-attribute] Submodule `Label` may not be available as an attribute on module `github`
+ github/AuthenticatedUser.py:955:30: warning[possibly-missing-attribute] Submodule `Team` may not be available as an attribute on module `github`
+ github/AuthenticatedUser.py:968:13: warning[possibly-missing-attribute] Submodule `Installation` may not be available as an attribute on module `github`
- github/Branch.py:100:43: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/Branch.py:100:43: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:101:38: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/Branch.py:101:38: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:103:44: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/Branch.py:103:44: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:105:48: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/Branch.py:105:48: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:500:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
+ github/Branch.py:500:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/Branch.py:501:13: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
+ github/Branch.py:501:13: warning[possibly-missing-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- github/Branch.py:511:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
+ github/Branch.py:511:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/Branch.py:512:13: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
+ github/Branch.py:512:13: warning[possibly-missing-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/BranchProtection.py:176:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
+ github/BranchProtection.py:176:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/Commit.py:289:13: error[unresolved-attribute] Submodule `PullRequest` may not be available as an attribute on module `github`
+ github/Commit.py:289:13: warning[possibly-missing-attribute] Submodule `PullRequest` may not be available as an attribute on module `github`
- github/CommitComment.py:191:13: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/CommitComment.py:213:16: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
+ github/CommitComment.py:191:13: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
+ github/CommitComment.py:213:16: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/Enterprise.py:140:16: error[unresolved-attribute] Submodule `Enterprise` may not be available as an attribute on module `github`
+ github/Enterprise.py:140:16: warning[possibly-missing-attribute] Submodule `Enterprise` may not be available as an attribute on module `github`
- github/Gist.py:148:26: error[unresolved-attribute] Submodule `Gist` may not be available as an attribute on module `github`
+ github/Gist.py:148:26: warning[possibly-missing-attribute] Submodule `Gist` may not be available as an attribute on module `github`
- github/GistHistoryState.py:207:69: error[unresolved-attribute] Submodule `GistFile` may not be available as an attribute on module `github`
+ github/GistHistoryState.py:207:69: warning[possibly-missing-attribute] Submodule `GistFile` may not be available as an attribute on module `github`
- github/GitRelease.py:316:16: error[unresolved-attribute] Submodule `GitRelease` may not be available as an attribute on module `github`
+ github/GitRelease.py:316:16: warning[possibly-missing-attribute] Submodule `GitRelease` may not be available as an attribute on module `github`
- github/GitRelease.py:384:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
+ github/GitRelease.py:384:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/IssueComment.py:194:13: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
+ github/IssueComment.py:194:13: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/IssueComment.py:216:16: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
+ github/IssueComment.py:216:16: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/MainClass.py:447:16: error[unresolved-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- github/MainClass.py:458:13: error[unresolved-attribute] Submodule `Organization` may not be available as an attribute on module `github`
+ github/MainClass.py:447:16: warning[possibly-missing-attribute] Submodule `Organization` may not be available as an attribute on module `github`
+ github/MainClass.py:458:13: warning[possibly-missing-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- github/MainClass.py:489:16: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/MainClass.py:489:16: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:509:13: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/MainClass.py:509:13: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:517:49: error[unresolved-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
+ github/MainClass.py:517:49: warning[possibly-missing-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
- github/MainClass.py:525:16: error[unresolved-attribute] Submodule `Project` may not be available as an attribute on module `github`
+ github/MainClass.py:525:16: warning[possibly-missing-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/MainClass.py:536:16: error[unresolved-attribute] Submodule `ProjectColumn` may not be available as an attribute on module `github`
- github/MainClass.py:606:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:607:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:608:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:609:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:610:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:611:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:612:32: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:613:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:614:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:615:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:616:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:617:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:618:25: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:619:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:620:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:623:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:626:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:628:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:632:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:634:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:637:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:641:32: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:643:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:647:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:649:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:651:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:653:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:655:25: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:657:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:660:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:536:16: warning[possibly-missing-attribute] Submodule `ProjectColumn` may not be available as an attribute on module `github`
+ github/MainClass.py:606:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:607:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:608:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:609:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:610:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:611:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:612:32: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:613:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:614:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:615:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:616:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:617:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:618:25: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:619:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:620:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:623:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:626:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:628:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:632:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:634:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:637:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:641:32: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:643:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:647:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:649:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:651:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:653:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:655:25: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:657:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ github/MainClass.py:660:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:663:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
+ github/MainClass.py:663:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/MainClass.py:704:13: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/MainClass.py:704:13: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:826:13: error[unresolved-attribute] Submodule `ContentFile` may not be available as an attribute on module `github`
+ github/MainClass.py:826:13: warning[possibly-missing-attribute] Submodule `ContentFile` may not be available as an attribute on module `github`
- github/MainClass.py:911:57: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/MainClass.py:911:57: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/NamedUser.py:469:51: error[unresolved-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
+ github/NamedUser.py:469:51: warning[possibly-missing-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
- github/NamedUser.py:486:13: error[unresolved-attribute] Submodule `Project` may not be available as an attribute on module `github`
+ github/NamedUser.py:486:13: warning[possibly-missing-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/NamedUser.py:585:38: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
+ github/NamedUser.py:585:38: warning[possibly-missing-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- github/NamedUser.py:598:16: error[unresolved-attribute] Submodule `Membership` may not be available as an attribute on module `github`
+ github/NamedUser.py:598:16: warning[possibly-missing-attribute] Submodule `Membership` may not be available as an attribute on module `github`
- github/NamedUser.py:652:54: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
+ github/NamedUser.py:652:54: warning[possibly-missing-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- github/Organization.py:756:16: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
+ github/Organization.py:756:16: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1156:16: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
+ github/Organization.py:1156:16: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1165:36: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
+ github/Organization.py:1165:36: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1171:16: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
+ github/Organization.py:1171:16: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1177:30: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
+ github/Organization.py:1177:30: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1229:41: error[unresolved-attribute] Submodule `Label` may not be available as an attribute on module `github`
+ github/Organization.py:1229:41: warning[possibly-missing-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/Organization.py:1240:30: error[unresolved-attribute] Submodule `Issue` may not be available as an attribute on module `github`
+ github/Organization.py:1240:30: warning[possibly-missing-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- github/Organization.py:1333:16: error[unresolved-attribute] Submodule `PublicKey` may not be available as an attribute on module `github`
+ github/Organization.py:1333:16: warning[possibly-missing-attribute] Submodule `PublicKey` may not be available as an attribute on module `github`
- github/Organization.py:1543:16: error[unresolved-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- github/Organization.py:1551:13: error[unresolved-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- github/Organization.py:1565:13: error[unresolved-attribute] Submodule `Installation` may not be available as an attribute on module `github`
+ github/Organization.py:1543:16: warning[possibly-missing-attribute] Submodule `Migration` may not be available as an attribute on module `github`
+ github/Organization.py:1551:13: warning[possibly-missing-attribute] Submodule `Migration` may not be available as an attribute on module `github`
+ github/Organization.py:1565:13: warning[possibly-missing-attribute] Submodule `Installation` may not be available as an attribute on module `github`
- github/PullRequest.py:454:16: error[unresolved-attribute] Submodule `Issue` may not be available as an attribute on module `github`
+ github/PullRequest.py:454:16: warning[possibly-missing-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- github/Repository.py:1210:38: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/Repository.py:1273:39: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/Repository.py:1274:29: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/Repository.py:1210:38: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/Repository.py:1273:39: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/Repository.py:1274:29: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/Repository.py:4757:17: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/Repository.py:4757:17: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/RepositoryDiscussionCategory.py:137:17: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/RepositoryDiscussionComment.py:111:13: error[unresolved-attribute] Submodule `RepositoryDiscussionComment` may not be available as an attribute on module `github`
- github/RepositoryDiscussionComment.py:122:13: error[unresolved-attribute] Submodule `RepositoryDiscussionComment` may not be available as an attribute on module `github`
- github/RepositoryDiscussionComment.py:135:17: error[unresolved-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
+ github/RepositoryDiscussionCategory.py:137:17: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/RepositoryDiscussionComment.py:111:13: warning[possibly-missing-attribute] Submodule `RepositoryDiscussionComment` may not be available as an attribute on module `github`
+ github/RepositoryDiscussionComment.py:122:13: warning[possibly-missing-attribute] Submodule `RepositoryDiscussionComment` may not be available as an attribute on module `github`
+ github/RepositoryDiscussionComment.py:135:17: warning[possibly-missing-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
- github/Team.py:128:33: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
+ github/Team.py:128:33: warning[possibly-missing-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/Team.py:322:16: error[unresolved-attribute] Submodule `Membership` may not be available as an attribute on module `github`
+ github/Team.py:322:16: warning[possibly-missing-attribute] Submodule `Membership` may not be available as an attribute on module `github`
- github/Team.py:429:13: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
+ github/Team.py:429:13: warning[possibly-missing-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/Team.py:566:53: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
+ github/Team.py:566:53: warning[possibly-missing-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/WorkflowRun.py:320:13: error[unresolved-attribute] Submodule `Artifact` may not be available as an attribute on module `github`
+ github/WorkflowRun.py:320:13: warning[possibly-missing-attribute] Submodule `Artifact` may not be available as an attribute on module `github`
- tests/AuthenticatedUser.py:796:81: error[unresolved-attribute] Submodule `Migration` may not be available as an attribute on module `github`
+ tests/AuthenticatedUser.py:796:81: warning[possibly-missing-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- tests/Exceptions.py:167:9: error[unresolved-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
+ tests/Exceptions.py:167:9: warning[possibly-missing-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
- tests/Exceptions.py:168:15: error[unresolved-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
+ tests/Exceptions.py:168:15: warning[possibly-missing-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
- tests/Framework.py:202:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:202:23: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:209:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:209:23: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:328:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:328:23: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:335:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:335:23: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:395:13: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:395:13: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:412:13: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:412:13: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:454:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:454:9: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:528:9: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ tests/Framework.py:528:9: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/Framework.py:529:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:529:9: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:530:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:530:9: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/GithubIntegration.py:148:45: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/GithubIntegration.py:148:45: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/GithubObject.py:39:7: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ tests/GithubObject.py:39:7: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/GithubObject.py:40:9: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
+ tests/GithubObject.py:40:9: warning[possibly-missing-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- tests/GithubObject.py:41:9: error[unresolved-attribute] Submodule `Organization` may not be available as an attribute on module `github`
+ tests/GithubObject.py:41:9: warning[possibly-missing-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- tests/Github_.py:292:49: error[unresolved-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
+ tests/Github_.py:292:49: warning[possibly-missing-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
- tests/Github_.py:295:50: error[unresolved-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
+ tests/Github_.py:295:50: warning[possibly-missing-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
- tests/Github_.py:431:30: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ tests/Github_.py:431:30: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/Installation.py:117:45: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Installation.py:117:45: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Issue.py:52:7: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
+ tests/Issue.py:52:7: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/Issue572.py:49:42: error[unresolved-attribute] Submodule `PullRequest` may not be available as an attribute on module `github`
+ tests/Issue572.py:49:42: warning[possibly-missing-attribute] Submodule `PullRequest` may not be available as an attribute on module `github`
- tests/Issue572.py:55:43: error[unresolved-attribute] Submodule `Issue` may not be available as an attribute on module `github`
+ tests/Issue572.py:55:43: warning[possibly-missing-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- tests/Logging_.py:85:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Logging_.py:85:9: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Logging_.py:88:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Logging_.py:88:9: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Organization.py:295:49: error[unresolved-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
+ tests/Organization.py:295:49: warning[possibly-missing-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
- tests/Organization.py:298:50: error[unresolved-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
+ tests/Organization.py:298:50: warning[possibly-missing-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
- tests/Organization.py:609:80: error[unresolved-attribute] Submodule `Migration` may not be available as an attribute on module `github`
+ tests/Organization.py:609:80: warning[possibly-missing-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- tests/Persistence.py:56:48: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ tests/Persistence.py:56:48: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- tests/PoolSize.py:39:43: e

... (truncated 3552 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-11-24 19:09_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-24 19:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-24 19:09_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-24 19:09_

---

_@Gankra approved on 2025-11-24 19:12_

Oh yes, big agree. 

---

_Merged by @AlexWaygood on 2025-11-24 19:15_

---

_Closed by @AlexWaygood on 2025-11-24 19:15_

---

_Branch deleted on 2025-11-24 19:15_

---
