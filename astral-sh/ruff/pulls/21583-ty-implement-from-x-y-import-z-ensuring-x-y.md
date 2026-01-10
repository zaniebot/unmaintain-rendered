```yaml
number: 21583
title: "[ty] Implement `from x.y import z` ensuring `x.y` resolves in the current file"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/submodule-attr-last
created_at: 2025-11-22T23:20:22Z
updated_at: 2025-12-13T20:08:19Z
url: https://github.com/astral-sh/ruff/pull/21583
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Implement `from x.y import z` ensuring `x.y` resolves in the current file

---

_Pull request opened by @AlexWaygood on 2025-11-22 23:20_

## Summary

Implement `from x.y import z` ensuring `x.y` resolves in the current file if a `y` attribute isn't otherwise available on `x`. This is implemented by adding these kinds of imports to `available_submodule_attributes` and lowering the priority of `available_submodule_attributes`.

Keeping the priority low ensures that things like `from .y import y` in an `__init__.py` reliable exposes the function/class `y` and not the submodule `y` (an extremely common idiom).

Fixes https://github.com/astral-sh/ty/issues/1654

## Test Plan

Snapshot tests.

---

_Label `ty` added by @AlexWaygood on 2025-11-22 23:20_

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 23:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-22 23:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/ruamel/yaml/main.py:77:25: warning[possibly-missing-attribute] Submodule `resolver` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:94:32: warning[possibly-missing-attribute] Submodule `representer` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:97:32: warning[possibly-missing-attribute] Submodule `constructor` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:100:32: warning[possibly-missing-attribute] Submodule `representer` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:103:32: warning[possibly-missing-attribute] Submodule `constructor` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:108:32: warning[possibly-missing-attribute] Submodule `representer` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:111:32: warning[possibly-missing-attribute] Submodule `constructor` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:117:32: warning[possibly-missing-attribute] Submodule `representer` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:122:32: warning[possibly-missing-attribute] Submodule `constructor` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:132:32: warning[possibly-missing-attribute] Submodule `representer` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:137:32: warning[possibly-missing-attribute] Submodule `constructor` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:660:13: warning[possibly-missing-attribute] Submodule `resolver` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:662:18: warning[possibly-missing-attribute] Submodule `resolver` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1472:46: warning[possibly-missing-attribute] Submodule `loader` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1481:46: warning[possibly-missing-attribute] Submodule `dumper` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1504:46: warning[possibly-missing-attribute] Submodule `loader` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1513:46: warning[possibly-missing-attribute] Submodule `dumper` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1563:33: warning[possibly-missing-attribute] Submodule `loader` may not be available as an attribute on module `spack.vendor.ruamel.yaml`
- Found 4265 diagnostics
+ Found 4247 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/tests/test_verify.py:169:25: warning[possibly-missing-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:237:25: warning[possibly-missing-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:238:25: warning[possibly-missing-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- Found 80 diagnostics
+ Found 77 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/door/_cls/doormeta.py:82:10: error[unresolved-attribute] Object of type `object` has no attribute `TypeHint`
- beartype/door/_cls/doormeta.py:163:19: error[unresolved-attribute] Object of type `object` has no attribute `TypeHint`
+ beartype/door/_cls/doormeta.py:163:45: error[invalid-assignment] Object of type `object` is not assignable to `TypeHint[Unknown]`
- beartype/door/_cls/doormeta.py:179:10: error[unresolved-attribute] Object of type `object` has no attribute `TypeHint`
- Found 491 diagnostics
+ Found 489 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_commands.py:46:59: warning[possibly-missing-attribute] Submodule `settings` may not be available as an attribute on module `scrapy`
- tests/test_feedexport.py:1347:29: warning[possibly-missing-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`
- tests/test_feedexport.py:1351:29: warning[possibly-missing-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`
- Found 1790 diagnostics
+ Found 1787 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:143:31: warning[possibly-missing-attribute] Submodule `main` may not be available as an attribute on module `_pytest`
- src/_pytest/python.py:1692:20: warning[possibly-missing-attribute] Submodule `_code` may not be available as an attribute on module `_pytest`
- testing/test_assertrewrite.py:2084:13: warning[possibly-missing-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
- testing/test_assertrewrite.py:2111:13: warning[possibly-missing-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2328:30: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2511:18: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2511:18: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2512:21: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- Found 443 diagnostics
+ Found 435 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/AuthenticatedUser.py:609:16: warning[possibly-missing-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:774:41: warning[possibly-missing-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:807:41: warning[possibly-missing-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:955:30: warning[possibly-missing-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:968:13: warning[possibly-missing-attribute] Submodule `Installation` may not be available as an attribute on module `github`
- github/Branch.py:100:43: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:101:38: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:103:44: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:105:48: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:500:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/Branch.py:501:13: warning[possibly-missing-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- github/Branch.py:511:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/Branch.py:512:13: warning[possibly-missing-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/BranchProtection.py:176:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
+ github/BranchProtection.py:176:86: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `str | _NotSetType`
- github/Commit.py:289:13: warning[possibly-missing-attribute] Submodule `PullRequest` may not be available as an attribute on module `github`
- github/CommitComment.py:191:13: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/CommitComment.py:213:16: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/GistHistoryState.py:207:69: warning[possibly-missing-attribute] Submodule `GistFile` may not be available as an attribute on module `github`
- github/GitRelease.py:384:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/IssueComment.py:194:13: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/IssueComment.py:216:16: warning[possibly-missing-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/MainClass.py:447:16: warning[possibly-missing-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- github/MainClass.py:458:13: warning[possibly-missing-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- github/MainClass.py:489:16: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:509:13: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:517:49: warning[possibly-missing-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
- github/MainClass.py:525:16: warning[possibly-missing-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/MainClass.py:536:16: warning[possibly-missing-attribute] Submodule `ProjectColumn` may not be available as an attribute on module `github`
- github/MainClass.py:606:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:607:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:608:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:609:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:610:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:611:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:612:32: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:613:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:614:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:615:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:616:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:617:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:618:25: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:619:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:620:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:623:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:626:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:628:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:632:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:634:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:637:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:641:32: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:643:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:647:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:649:27: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:651:28: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:653:26: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:655:25: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:657:24: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:660:29: warning[possibly-missing-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:663:16: warning[possibly-missing-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/MainClass.py:704:13: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:826:13: warning[possibly-missing-attribute] Submodule `ContentFile` may not be available as an attribute on module `github`
- github/MainClass.py:911:57: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:915:42: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `_NotSetType | (Repository & Unknown)`
+ github/MainClass.py:915:42: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `_NotSetType | Repository`
- github/NamedUser.py:469:51: warning[possibly-missing-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
- github/NamedUser.py:486:13: warning[possibly-missing-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/NamedUser.py:598:16: warning[possibly-missing-attribute] Submodule `Membership` may not be available as an attribute on module `github`
- github/Organization.py:756:16: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1156:16: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1165:36: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1171:16: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1177:30: warning[possibly-missing-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1229:41: warning[possibly-missing-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/Organization.py:1240:30: warning[possibly-missing-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- github/Organization.py:1333:16: warning[possibly-missing-attribute] Submodule `PublicKey` may not be available as an attribute on module `github`
- github/Organization.py:1543:16: warning[possibly-missing-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- github/Organization.py:1551:13: warning[possibly-missing-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- github/Organization.py:1565:13: warning[possibly-missing-attribute] Submodule `Installation` may not be available as an attribute on module `github`
- github/PullRequest.py:454:16: warning[possibly-missing-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- github/RepositoryDiscussionCategory.py:137:17: warning[possibly-missing-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/RepositoryDiscussionComment.py:135:17: warning[possibly-missing-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
- github/Team.py:322:16: warning[possibly-missing-attribute] Submodule `Membership` may not be available as an attribute on module `github`
- github/WorkflowRun.py:320:13: warning[possibly-missing-attribute] Submodule `Artifact` may not be available as an attribute on module `github`
- tests/Requester.py:49:9: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:52:9: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:61:21: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:79:41: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:98:16: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:115:21: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:404:29: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:406:30: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:410:29: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:412:30: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:416:29: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Requester.py:418:30: warning[possibly-missing-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- Found 354 diagnostics
+ Found 262 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/settings.py:732:72: warning[possibly-missing-attribute] Submodule `ttk` may not be available as an attribute on module `tkinter`
- Found 17 diagnostics
+ Found 16 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/lib/faas_utest.py:30:29: warning[possibly-missing-attribute] Submodule `mock` may not be available as an attribute on module `unittest`
- Found 436 diagnostics
+ Found 435 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/check_ssl_pinning.py:93:9: warning[possibly-missing-attribute] Submodule `certs` may not be available as an attribute on module `mitmproxy`
+ examples/contrib/check_ssl_pinning.py:93:9: error[invalid-assignment] Implicit shadowing of function `dummy_cert`
- mitmproxy/tools/web/app.py:580:30: warning[possibly-missing-attribute] Submodule `http` may not be available as an attribute on module `mitmproxy`
- mitmproxy/tools/web/app.py:585:29: error[unresolved-attribute] Unresolved attribute `port` on type `object`.
- mitmproxy/tools/web/app.py:587:29: error[unresolved-attribute] Object of type `object` has no attribute `headers`
+ mitmproxy/tools/web/app.py:580:55: error[invalid-assignment] Object of type `object` is not assignable to `Request`
+ mitmproxy/tools/web/app.py:596:33: warning[possibly-missing-attribute] Attribute `add` may be missing on object of type `Headers | None`
- mitmproxy/tools/web/app.py:589:33: error[unresolved-attribute] Object of type `object` has no attribute `headers`
- mitmproxy/tools/web/app.py:591:32: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:592:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:594:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`.
- mitmproxy/tools/web/app.py:594:52: warning[possibly-missing-attribute] Submodule `http` may not be available as an attribute on module `mitmproxy`
- mitmproxy/tools/web/app.py:596:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:598:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`.
- mitmproxy/tools/web/app.py:603:31: warning[possibly-missing-attribute] Submodule `http` may not be available as an attribute on module `mitmproxy`
- mitmproxy/tools/web/app.py:608:29: error[unresolved-attribute] Unresolved attribute `status_code` on type `object`.
- mitmproxy/tools/web/app.py:610:29: error[unresolved-attribute] Object of type `object` has no attribute `headers`
+ mitmproxy/tools/web/app.py:603:57: error[invalid-assignment] Object of type `object` is not assignable to `Response`
+ mitmproxy/tools/web/app.py:619:33: warning[possibly-missing-attribute] Attribute `add` may be missing on object of type `Headers | None`
- mitmproxy/tools/web/app.py:612:33: error[unresolved-attribute] Object of type `object` has no attribute `headers`
- mitmproxy/tools/web/app.py:614:32: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:615:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:617:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`.
- mitmproxy/tools/web/app.py:617:53: warning[possibly-missing-attribute] Submodule `http` may not be available as an attribute on module `mitmproxy`
- mitmproxy/tools/web/app.py:619:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:621:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`.
- test/mitmproxy/test_flow.py:135:25: warning[possibly-missing-attribute] Submodule `test` may not be available as an attribute on module `mitmproxy`
+ test/mitmproxy/test_flow.py:135:25: warning[possibly-missing-attribute] Submodule `tutils` may not be available as an attribute on module `mitmproxy.test`
- test/mitmproxy/test_flow.py:139:26: warning[possibly-missing-attribute] Submodule `test` may not be available as an attribute on module `mitmproxy`
+ test/mitmproxy/test_flow.py:139:26: warning[possibly-missing-attribute] Submodule `tutils` may not be available as an attribute on module `mitmproxy.test`
- Found 2135 diagnostics
+ Found 2119 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/contrib/emscripten/test_emscripten.py:913:23: warning[possibly-missing-attribute] Submodule `connection` may not be available as an attribute on module `urllib3`
- Found 272 diagnostics
+ Found 271 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- tests/_test_cursor.py:63:16: warning[possibly-missing-attribute] Submodule `rows` may not be available as an attribute on module `psycopg`
- tests/fix_faker.py:88:14: warning[possibly-missing-attribute] Submodule `adapt` may not be available as an attribute on module `psycopg`
- tests/test_connection.py:112:25: warning[possibly-missing-attribute] Submodule `conninfo` may not be available as an attribute on module `psycopg`
- tests/test_connection.py:643:35: warning[possibly-missing-attribute] Submodule `rows` may not be available as an attribute on module `psycopg`
- tests/test_connection.py:656:35: warning[possibly-missing-attribute] Submodule `rows` may not be available as an attribute on module `psycopg`
- tests/test_connection.py:668:47: warning[possibly-missing-attribute] Submodule `rows` may not be available as an attribute on module `psycopg`
- tests/test_connection_async.py:107:25: warning[possibly-missing-attribute] Submodule `conninfo` may not be available as an attribute on module `psycopg`
- tests/test_connection_async.py:641:40: warning[possibly-missing-attribute] Submodule `rows` may not be available as an attribute on module `psycopg`
- tests/test_connection_async.py:653:40: warning[possibly-missing-attribute] Submodule `rows` may not be available as an attribute on module `psycopg`
- tests/test_connection_async.py:665:52: warning[possibly-missing-attribute] Submodule `rows` may not be available as an attribute on module `psycopg`
- tests/test_cursor_common.py:204:29: warning[possibly-missing-attribute] Submodule `adapt` may not be available as an attribute on module `psycopg`
- tests/test_cursor_common_async.py:202:29: warning[possibly-missing-attribute] Submodule `adapt` may not be available as an attribute on module `psycopg`
- tests/types/test_json.py:28:23: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_json.py:41:23: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_json.py:63:22: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_json.py:85:23: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_json.py:133:23: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_json.py:148:23: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_json.py:163:23: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_json.py:178:23: warning[possibly-missing-attribute] Submodule `json` may not be available as an attribute on module `psycopg.types`
- tests/types/test_numeric.py:589:23: warning[possibly-missing-attribute] Submodule `numeric` may not be available as an attribute on module `psycopg.types`
- tests/types/test_numeric.py:600:23: warning[possibly-missing-attribute] Submodule `numeric` may not be available as an attribute on module `psycopg.types`
- tests/types/test_numeric.py:612:23: warning[possibly-missing-attribute] Submodule `numeric` may not be available as an attribute on module `psycopg.types`
- Found 682 diagnostics
+ Found 659 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/sourceline.py:17:30: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Expected `CommentedBase`, found `object`
+ schema_salad/sourceline.py:20:30: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Expected `CommentedBase`, found `object`
- schema_salad/sourceline.py:12:25: warning[possibly-missing-attribute] Submodule `comments` may not be available as an attribute on module `ruamel.yaml`
- schema_salad/sourceline.py:13:22: warning[possibly-missing-attribute] Submodule `comments` may not be available as an attribute on module `ruamel.yaml`
- schema_salad/sourceline.py:30:24: warning[possibly-missing-attribute] Submodule `comments` may not be available as an attribute on module `ruamel.yaml`
- schema_salad/sourceline.py:286:38: warning[possibly-missing-attribute] Submodule `comments` may not be available as an attribute on module `ruamel.yaml`
- schema_salad/tests/test_errors.py:302:24: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad`
- schema_salad/tests/test_errors.py:323:20: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad`
- schema_salad/tests/test_errors.py:350:20: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad`
- schema_salad/tests/test_errors.py:372:20: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad`
- schema_salad/tests/test_errors.py:394:20: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad`
- schema_salad/tests/test_examples.py:589:11: warning[possibly-missing-attribute] Submodule `ref_resolver` may not be available as an attribute on module `schema_salad`
- schema_salad/validate.py:93:22: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:95:22: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:97:23: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:97:46: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:99:22: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:101:23: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:101:48: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:177:36: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:210:36: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:240:37: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:240:62: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:261:73: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:264:21: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:264:47: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:264:70: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:270:21: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:271:21: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:272:21: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:273:21: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:312:36: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:451:37: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- schema_salad/validate.py:451:60: warning[possibly-missing-attribute] Submodule `schema` may not be available as an attribute on module `schema_salad.avro`
- Found 228 diagnostics
+ Found 198 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pyspark/container.py:141:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/api/pyspark/container.py:43:28: warning[possibly-missing-attribute] Member `api` may be missing on module `pandera`
- pandera/api/pyspark/container.py:581:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/api/pyspark/container.py:599:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/api/pyspark/container.py:600:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/dask/test_dask.py:111:27: warning[possibly-missing-attribute] Submodule `dask` may not be available as an attribute on module `pandera.typing`
- tests/dask/test_dask.py:114:28: warning[possibly-missing-attribute] Submodule `dask` may not be available as an attribute on module `pandera.typing`
+ tests/dask/test_dask.py:111:27: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `dask`
+ tests/dask/test_dask.py:114:28: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `dask`
- tests/fastapi/models.py:10:9: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/fastapi/models.py:10:9: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/fastapi/models.py:11:11: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/fastapi/models.py:11:11: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/fastapi/models.py:23:9: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/fastapi/models.py:23:9: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/fastapi/models.py:24:11: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/fastapi/models.py:24:11: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/fastapi/models.py:25:11: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/fastapi/models.py:25:11: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/fastapi/models.py:48:9: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing`
+ tests/fastapi/models.py:48:9: warning[possibly-missing-attribute] Attribute `DataFrame` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/io/test_pandas_io.py:1431:9: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:1443:9: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:1460:9: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:1502:18: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:1503:14: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:1526:9: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:1902:14: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:1983:14: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/io/test_pandas_io.py:2029:14: warning[possibly-missing-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- tests/modin/test_schemas_on_modin.py:317:20: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:318:22: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:319:20: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:389:12: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:392:12: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:418:13: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:419:10: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:425:13: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
- tests/modin/test_schemas_on_modin.py:426:10: warning[possibly-missing-attribute] Submodule `modin` may not be available as an attribute on module `pandera.typing`
+ tests/modin/test_schemas_on_modin.py:317:20: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:318:22: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:319:20: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:389:12: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:392:12: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:418:13: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:419:10: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:425:13: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
+ tests/modin/test_schemas_on_modin.py:426:10: error[unresolved-attribute] Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
- tests/mypy/pandas_modules/pandera_inheritance.py:8:8: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/mypy/pandas_modules/pandera_inheritance.py:8:8: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/mypy/pandas_modules/pandera_inheritance.py:9:8: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/mypy/pandas_modules/pandera_inheritance.py:9:8: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/mypy/pandas_modules/pandera_inheritance.py:10:8: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/mypy/pandas_modules/pandera_inheritance.py:10:8: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/mypy/pandas_modules/pandera_inheritance.py:14:8: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/mypy/pandas_modules/pandera_inheritance.py:14:8: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/mypy/pandas_modules/pandera_inheritance.py:15:8: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/mypy/pandas_modules/pandera_inheritance.py:15:8: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/mypy/pandas_modules/pandera_inheritance.py:16:8: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/mypy/pandas_modules/pandera_inheritance.py:16:8: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/mypy/pandas_modules/pandera_types.py:7:16: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/mypy/pandas_modules/pandera_types.py:7:16: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_extensions.py:258:15: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_extensions.py:258:15: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_extensions.py:259:15: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_extensions.py:259:15: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_extensions.py:293:18: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_extensions.py:293:18: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_extensions.py:303:14: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_extensions.py:303:14: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:16:14: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:16:14: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:17:14: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:17:14: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:73:16: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:73:16: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:212:16: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:212:16: warning[possibly-missing-attribute] Attribute `DataFrame` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:276:16: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:276:16: warning[possibly-missing-attribute] Attribute `DataFrame` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:276:50: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:276:50: warning[possibly-missing-attribute] Attribute `DataFrame` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:281:13: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:281:13: warning[possibly-missing-attribute] Attribute `DataFrame` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_from_to_format_conversions.py:282:10: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing`
+ tests/pandas/test_from_to_format_conversions.py:282:10: warning[possibly-missing-attribute] Attribute `DataFrame` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_model.py:225:23: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_model.py:225:23: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_model.py:1995:15: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_model.py:1995:15: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_model.py:1996:15: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_model.py:1996:15: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_model.py:2014:15: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_model.py:2014:15: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_model.py:2015:15: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing`
+ tests/pandas/test_model.py:2015:15: warning[possibly-missing-attribute] Attribute `Series` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
- tests/pandas/test_model.py:2033:15: warning[possibly-missing-attribute] Member `Series` ma

... (truncated 738 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct metadata = ~10MB
+     struct metadata = ~11MB


```

</details>




---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-22 23:52_

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 00:01_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-missing-attribute` | 218 | 381 | 132 |
| `unresolved-attribute` | 16 | 21 | 0 |
| `invalid-assignment` | 6 | 0 | 0 |
| `unused-ignore-comment` | 1 | 4 | 0 |
| `invalid-argument-type` | 4 | 0 | 0 |
| **Total** | **245** | **406** | **132** |

**[Full report with detailed diff](https://alex-submodule-attr-last.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-submodule-attr-last.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-11-23 00:28_

everything's great as long as you don't scroll down to the bit of the primer diff where the `prefect` diagnostics are listed 

---

_Comment by @Gankra on 2025-11-23 16:39_

Taking a look at the code, the errors on prefect seem completely legitimate? The file doesn't import it and the `__init__.py`'s along the chain don't import it. These symbols being available must be genuine spooky-action-at-a-distance that we should probably discourage.

---

_Comment by @AlexWaygood on 2025-11-23 16:41_

> Taking a look at the code, the errors on prefect seem completely legitimate? The file doesn't import it and the `__init__.py`'s along the chain don't import it. These symbols being available must be genuine spooky-action-at-a-distance that we should probably discourage.

Yeah I agree, I'm not sure why we aren't emitting errors on this on `main`? There are other repos on which this branch does indeed produce false positives though, which I'm looking at now

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1534 on 2025-11-23 16:45_

Should we add the `is_global` check to `ast::Stmt::Import` too? (It not being there bothers me...) 

---

_@Gankra reviewed on 2025-11-23 16:45_

---

_Comment by @Gankra on 2025-11-23 16:52_

Also did you ever try not-distinguishing-import-kind? i.e. even making `import ...` weak?

The synthesis of these two issues would be interested in it (but maybe we just use the kind machinery to represent that case anyway):

* https://github.com/astral-sh/ty/issues/1490
* https://github.com/astral-sh/ty/issues/1526

---

_Comment by @AlexWaygood on 2025-11-23 17:02_

> Also did you ever try not-distinguishing-import-kind? i.e. even making `import ...` weak?

I _did_, but it directly contradicts some [longstanding tests](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/tracking.md#attribute-overrides-submodule) that were added in https://github.com/astral-sh/ruff/pull/14946 sooo... I got scared :P

I _do_ think it makes sense that if a user has `import a.b` at the top of their module, they almost certainly expect the attribute `a.b` to resolve to the `b` submodule of the package `a`, whereas they won't necessarily expect the same thing for the `a.b` access if all they have in their module is `from a.b import c`... but it's also the case that in tie-breaking situations, we just don't know for sure which way the tie will _actually_ be broken, and I don't know if it would be clear to users why we're distinguishing between the two kinds of imports.

The longstanding tests were also added before we added any kind of ecosystem check, so I do think it would be interesting to see the ecosystem effect from not-distinguishing-import-kind as well. I'd like to try it separately, after I've debugged some of the stranger diagnostics on this branch a bit more.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1534 on 2025-11-23 17:03_

yeah you're right, these should definitely be consistent

---

_@AlexWaygood reviewed on 2025-11-23 17:03_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-23 17:17_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-23 17:17_

---

_@AlexWaygood reviewed on 2025-11-23 19:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:13330 on 2025-11-23 19:19_

it's extremely hard to write a regression test for this (though I might try harder later), but empirically this gets rid of a false-positive diagnostic on [this line](https://github.com/DataDog/dd-trace-py/blob/7e78fe5f172442f2379ea725fe4e5ea534f00395/ddtrace/contrib/internal/trace_utils.py#L56) that was ultimately due to us thinking the `importing_file` of `ddtrace.internal` was `ddtrace/__init__.py` rather than `ddtrace/contrib/internal/trace_utils.py`

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-23 19:21_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-23 19:21_

---

_Comment by @AlexWaygood on 2025-11-23 19:43_

I can't see any "obviously wrong" diagnostics in the ecosystem anymore among the added or changed diagnostics. Nearly all the _new_ diagnostics are on prefect. As noted above, I'm not sure why these aren't emitted on `main`; they seem like true positives, as do all new diagnostics on other repos. This does get rid of diagnostics across the ecosystem overall now; it's just a question of whether this behaviour is explainable to users.

Still TODO is
- ~~to just try out the bigger hammer of always giving `available_submodule_attributes` lowest priority, which would certainly be a simpler rule (and therefore easier to explain to users)~~.
  - possibly a wash? https://github.com/astral-sh/ruff/pull/21653
- ~~https://github.com/astral-sh/ruff/pull/21583#discussion_r2554196826 should probably be fixed in a standalone PR~~
  - I tried this in #21599 and didn't like it
- ~~try harder to write a standalone regression test for https://github.com/astral-sh/ruff/pull/21583#discussion_r2554271218~~
  - THE DEVIL ITSELF LIVES HERE??
- ~~figure out why none of those prefect diagnostics are reported on `main`~~
  - no longer hitting a `__getattr__` fallback

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-23 22:54_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-23 22:54_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-24 18:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 18:57_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-25 10:58_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-25 10:58_

---

_Comment by @Gankra on 2025-11-25 13:52_

They say that even to this day if you put your ear against a `from ... import` statement you can hear the screams of engineers in tie-break hell.

---

_Comment by @Gankra on 2025-11-26 20:14_

Taking over this PR for Alex.

---

_Assigned to @Gankra by @Gankra on 2025-11-26 20:14_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-11-26 20:17_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-26 20:18_

---

_Renamed from "alex/submodule attr last" to "[ty] Implement `from x.y import z` ensuring `x.y` resolves in the current file" by @Gankra on 2025-11-26 20:21_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-11-26 20:30_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-26 20:30_

---

_Comment by @Gankra on 2025-11-26 20:43_

Oh dear, rebasing + rerunning has even worse results than last time. Gotta look at what's goin' on...

---

_Comment by @Gankra on 2025-11-26 23:17_

`prefect` mystery solved: their `__init__.py` intentionally doesn't import anything and contains a `__getattr__` that lazy-imports its submodules. We previously found this implementation out of desperation and concluded all submodule attributes resolved and were `Any`. Now that we resolve a lot of imports, we "dodge" the `__getattr__` and have way less `Any` making everything resolve everywhere.

This is an acceptable regression.

---

_Comment by @Gankra on 2025-11-27 00:32_

Ok well I can see why you had trouble reproducing the issue... THE ISSUE ONLY APPEARS IF THE CODE CONTAINS `from typing import TYPE_CHECKING`

YOU DON'T EVEN NEED TO USE IT.

WHAT.

---

_Comment by @Gankra on 2025-11-27 00:56_

More precise statement after messing around with it: `from typing import <anything that resolves>` has this issue. Importing from other modules like `typing_extensions` or `warnings` has no effect. 

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-11-27 01:09_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-27 01:09_

---

_Comment by @Gankra on 2025-11-27 01:21_

With asymmetric weak priority:

<img width="519" height="349" alt="Screenshot 2025-11-26 at 8 07 45 PM" src="https://github.com/user-attachments/assets/bec118ca-425c-49d4-bbfb-a18dccc28198" />


With fully weak priority:


<img width="541" height="409" alt="Screenshot 2025-11-26 at 8 18 25 PM" src="https://github.com/user-attachments/assets/a0ff70e0-6e0f-4b50-852a-7a51912c8895" />


---

_Comment by @Gankra on 2025-11-27 01:51_

Note: split the uniformly-weak-submodule-attributes out to https://github.com/astral-sh/ruff/pull/21653


---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-11-27 02:36_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-27 02:36_

---

_Comment by @Gankra on 2025-11-27 02:45_

scipy has the same magic lazy imports with `__getattr__` thing that prefect does, although it comes up a lot less.

*  [ Module `scipy.special` has no member `multigammaln` ](https://github.com/scipy/scipy/blob/1c36a69785d3d27ae279f3efbc4d66d7dd8acc7b/scipy/stats/_multivariate.py#L1821) even though [it's exported in `__all__` but maybe we get confused about the complex definition of it?](https://github.com/scipy/scipy/blob/1c36a69785d3d27ae279f3efbc4d66d7dd8acc7b/scipy/special/__init__.py#L829)
*  [Module `scipy.linalg.blas` has no member `dnrm2`](https://github.com/scipy/scipy/blob/1c36a69785d3d27ae279f3efbc4d66d7dd8acc7b/scipy/linalg/tests/test_blas.py#L1025)

---

_Comment by @Gankra on 2025-11-27 02:52_

```diff
- [warning] possibly-missing-attribute - Submodule `modin` may not be available as an attribute on module `pandera.typing`
+ [error] unresolved-attribute - Object of type `<module 'pandera.typing'> | <module 'pandera.typing'>` has no attribute `modin`
```

This is an unfortunate but kinda minor degradation (we could potentially add some diagnostic cleaning for it).

---

_Marked ready for review by @Gankra on 2025-11-27 14:39_

---

_Review requested from @carljm by @Gankra on 2025-11-27 14:39_

---

_Review requested from @sharkdp by @Gankra on 2025-11-27 14:39_

---

_Review requested from @dcreager by @Gankra on 2025-11-27 14:39_

---

_Comment by @Gankra on 2025-11-27 14:40_

I think this is ready for review/discussion.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:13330 on 2025-11-27 14:42_

update: @Gankra succeeded in writing a regression test 😃 it's the "A tale of two modules" regression test

---

_@AlexWaygood reviewed on 2025-11-27 14:42_

---

_@Gankra approved on 2025-11-27 14:51_

I guess I actually didn't end up changing the code itself, so, the code changes honestly look like what I'd expect.

---

_Comment by @AlexWaygood on 2025-11-27 15:00_

Thank you so much for picking this up, and in particular for experimenting with #21653 separately!

I'm surprised at how small the diff is on #21653 -- it's basically _all_ pwndbg, which is doing very strange and unusual things with its submodule imports (["submodule imports Georg" should be excluded as an outlier](https://www.huffingtonpost.co.uk/entry/spiders-georg-meme_n_5412861), etc.). So I wonder if we should just adopt the rule proposed by #21653, where submodule attributes always have lower priority than symbols defined in the `__init__.py` module namespace? The attraction of that is that it's a much simpler rule to explain to users; it's not great to have to explain a subtle distinction where we treat a submodule attribute introduced by `import a.b` as having a higher priority than a submodule attribute introduced by `from a.b import c`.

As per https://github.com/astral-sh/ruff/pull/14946#discussion_r1884626442, that would mean that we'd be moving towards mypy's behaviour here and away from pyright's. But empirically, the ecosystem results appear to indicate that mypy's heuristic works better much more of the time than pyright's does.

I'm curious for what others think; don't make any changes here until other folks have had a chance to chime in 😅 The disadvantage of adopting the rule in #21653 is that you can get counterintuitive situations where things like this can happen:

```py
import a.b

reveal_type(a.b)  # revealed: Literal[1]
```

But the ecosystem report suggests that situations like this are rare. And honestly, at some point you just have to say that libraries need to stop shadowing their submodules with variables in `__init__.py` if they want type checkers to understand this stuff.

---

_Comment by @AlexWaygood on 2025-12-02 21:44_

Moving back into draft because @Gankra and I want to try a few more things out (possibly after the beta)

---

_Converted to draft by @AlexWaygood on 2025-12-02 21:44_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-12-13 19:59_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-12-13 19:59_

---
