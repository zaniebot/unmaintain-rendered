---
number: 2400
title: Hang/OOM on generic function wrapping a method with many overloads
type: issue
state: open
author: jelle-openai
labels:
  - performance
  - fatal
  - overloads
assignees: []
created_at: 2026-01-08T17:33:46Z
updated_at: 2026-01-09T11:18:38Z
url: https://github.com/astral-sh/ty/issues/2400
synced_at: 2026-01-10T01:48:23Z
---

# Hang/OOM on generic function wrapping a method with many overloads

---

_Issue opened by @jelle-openai on 2026-01-08 17:33_

### Summary

Given this file:

```python
#!/usr/bin/env python3
# /// script
# requires-python = ">=3.12"
# dependencies = ["githubkit"]
# ///
from typing import Callable, ParamSpec, TypeVar

from githubkit.webhooks import parse_obj


P = ParamSpec("P")
R = TypeVar("R", covariant=True)


def wrap_non_retryable(fn: Callable[P, R]) -> Callable[P, R]:
    def _wrapped(*args: P.args, **kwargs: P.kwargs) -> R:
        raise Exception

    return _wrapped


safe_parse_obj = wrap_non_retryable(parse_obj)

```

`uvx ty check tybugoom.py` takes a huge amount of memory and never finishes (well, maybe I wasn't patient enough).

I haven't tried to minimize away the third-party library, but parse_obj is at https://github.com/yanyongyu/githubkit/blob/daec39c41e7cc0e13924647bb061950ba4fa1b68/githubkit/webhooks/__init__.py#L12, it's a method from a class that's bound to a local variable.

### Version

ty 0.0.10 (d18902cdc 2026-01-07)

---

_Label `fatal` added by @MichaReiser on 2026-01-08 17:36_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-08 17:36_

---

_Comment by @jelle-openai on 2026-01-08 17:50_

OK managed to get rid of the third-party library. `parse_obj` is an overloaded function with like 50 definitions, I'm guessing something is O(bad) in the number of overloads?

`uvx ty check` on this still reproes the hang:
```python
from typing import Callable, ParamSpec, TypeVar, overload, Union, Literal, Any, Mapping


class WebhookEvent: pass
class WorkflowRunEvent(WebhookEvent): pass
class BranchProtectionConfigurationEvent(WebhookEvent): pass
class BranchProtectionRuleEvent(WebhookEvent): pass
class CheckRunEvent(WebhookEvent): pass
class CheckSuiteEvent(WebhookEvent): pass
class CodeScanningAlertEvent(WebhookEvent): pass
class CommitCommentEvent(WebhookEvent): pass
class CreateEvent(WebhookEvent): pass
class CustomPropertyEvent(WebhookEvent): pass
class CustomPropertyValuesEvent(WebhookEvent): pass
class DeleteEvent(WebhookEvent): pass
class DependabotAlertEvent(WebhookEvent): pass
class DeployKeyEvent(WebhookEvent): pass
class DeploymentEvent(WebhookEvent): pass
class DeploymentProtectionRuleEvent(WebhookEvent): pass
class DeploymentReviewEvent(WebhookEvent): pass
class DeploymentStatusEvent(WebhookEvent): pass
class DiscussionEvent(WebhookEvent): pass
class DiscussionCommentEvent(WebhookEvent): pass
class ForkEvent(WebhookEvent): pass
class GithubAppAuthorizationEvent(WebhookEvent): pass
class GollumEvent(WebhookEvent): pass
class InstallationEvent(WebhookEvent): pass
class InstallationRepositoriesEvent(WebhookEvent): pass
class InstallationTargetEvent(WebhookEvent): pass
class IssueCommentEvent(WebhookEvent): pass
class IssueDependenciesEvent(WebhookEvent): pass
class IssuesEvent(WebhookEvent): pass
class LabelEvent(WebhookEvent): pass
class MarketplacePurchaseEvent(WebhookEvent): pass
class MemberEvent(WebhookEvent): pass
class MembershipEvent(WebhookEvent): pass
class MergeGroupEvent(WebhookEvent): pass
class MetaEvent(WebhookEvent): pass
class MilestoneEvent(WebhookEvent): pass
class OrgBlockEvent(WebhookEvent): pass
class OrganizationEvent(WebhookEvent): pass
class PackageEvent(WebhookEvent): pass
class PageBuildEvent(WebhookEvent): pass
class PersonalAccessTokenRequestEvent(WebhookEvent): pass
class PingEvent(WebhookEvent): pass
class ProjectCardEvent(WebhookEvent): pass
class ProjectEvent(WebhookEvent): pass
class ProjectColumnEvent(WebhookEvent): pass
class ProjectsV2Event(WebhookEvent): pass
class ProjectsV2ItemEvent(WebhookEvent): pass
class ProjectsV2StatusUpdateEvent(WebhookEvent): pass
class PublicEvent(WebhookEvent): pass
class PullRequestEvent(WebhookEvent): pass
class PullRequestReviewCommentEvent(WebhookEvent): pass
class PullRequestReviewEvent(WebhookEvent): pass
class PullRequestReviewThreadEvent(WebhookEvent): pass
class PushEvent(WebhookEvent): pass
class RegistryPackageEvent(WebhookEvent): pass
class ReleaseEvent(WebhookEvent): pass
class RepositoryAdvisoryEvent(WebhookEvent): pass
class RepositoryEvent(WebhookEvent): pass
class RepositoryDispatchEvent(WebhookEvent): pass
class RepositoryImportEvent(WebhookEvent): pass

class RepositoryRulesetEvent(WebhookEvent): pass
class RepositoryVulnerabilityAlertEvent(WebhookEvent): pass
class SecretScanningAlertEvent(WebhookEvent): pass
class SecretScanningAlertLocationEvent(WebhookEvent): pass
class SecretScanningScanEvent(WebhookEvent): pass
class SecurityAdvisoryEvent(WebhookEvent): pass
class SecurityAndAnalysisEvent(WebhookEvent): pass

class SponsorshipEvent(WebhookEvent): pass
class StarEvent(WebhookEvent): pass
class StatusEvent(WebhookEvent): pass
class SubIssuesEvent(WebhookEvent): pass
class TeamAddEvent(WebhookEvent): pass
class TeamEvent(WebhookEvent): pass
class WatchEvent(WebhookEvent): pass
class WorkflowDispatchEvent(WebhookEvent): pass
class WorkflowJobEvent(WebhookEvent): pass

type EventNameType = str

class WN:
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["branch_protection_configuration"], payload: Mapping[str, Any]
    ) -> "BranchProtectionConfigurationEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["branch_protection_rule"], payload: Mapping[str, Any]
    ) -> "BranchProtectionRuleEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["check_run"], payload: Mapping[str, Any]
    ) -> "CheckRunEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["check_suite"], payload: Mapping[str, Any]
    ) -> "CheckSuiteEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["code_scanning_alert"], payload: Mapping[str, Any]
    ) -> "CodeScanningAlertEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["commit_comment"], payload: Mapping[str, Any]
    ) -> "CommitCommentEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["create"], payload: Mapping[str, Any]
    ) -> "CreateEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["custom_property"], payload: Mapping[str, Any]
    ) -> "CustomPropertyEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["custom_property_values"], payload: Mapping[str, Any]
    ) -> "CustomPropertyValuesEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["delete"], payload: Mapping[str, Any]
    ) -> "DeleteEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["dependabot_alert"], payload: Mapping[str, Any]
    ) -> "DependabotAlertEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["deploy_key"], payload: Mapping[str, Any]
    ) -> "DeployKeyEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["deployment"], payload: Mapping[str, Any]
    ) -> "DeploymentEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["deployment_protection_rule"], payload: Mapping[str, Any]
    ) -> "DeploymentProtectionRuleEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["deployment_review"], payload: Mapping[str, Any]
    ) -> "DeploymentReviewEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["deployment_status"], payload: Mapping[str, Any]
    ) -> "DeploymentStatusEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["discussion"], payload: Mapping[str, Any]
    ) -> "DiscussionEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["discussion_comment"], payload: Mapping[str, Any]
    ) -> "DiscussionCommentEvent": ...
    @overload
    @staticmethod
    def parse_obj(name: Literal["fork"], payload: Mapping[str, Any]) -> "ForkEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["github_app_authorization"], payload: Mapping[str, Any]
    ) -> "GithubAppAuthorizationEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["gollum"], payload: Mapping[str, Any]
    ) -> "GollumEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["installation"], payload: Mapping[str, Any]
    ) -> "InstallationEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["installation_repositories"], payload: Mapping[str, Any]
    ) -> "InstallationRepositoriesEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["installation_target"], payload: Mapping[str, Any]
    ) -> "InstallationTargetEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["issue_comment"], payload: Mapping[str, Any]
    ) -> "IssueCommentEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["issue_dependencies"], payload: Mapping[str, Any]
    ) -> "IssueDependenciesEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["issues"], payload: Mapping[str, Any]
    ) -> "IssuesEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["label"], payload: Mapping[str, Any]
    ) -> "LabelEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["marketplace_purchase"], payload: Mapping[str, Any]
    ) -> "MarketplacePurchaseEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["member"], payload: Mapping[str, Any]
    ) -> "MemberEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["membership"], payload: Mapping[str, Any]
    ) -> "MembershipEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["merge_group"], payload: Mapping[str, Any]
    ) -> "MergeGroupEvent": ...
    @overload
    @staticmethod
    def parse_obj(name: Literal["meta"], payload: Mapping[str, Any]) -> "MetaEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["milestone"], payload: Mapping[str, Any]
    ) -> "MilestoneEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["org_block"], payload: Mapping[str, Any]
    ) -> "OrgBlockEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["organization"], payload: Mapping[str, Any]
    ) -> "OrganizationEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["package"], payload: Mapping[str, Any]
    ) -> "PackageEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["page_build"], payload: Mapping[str, Any]
    ) -> "PageBuildEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["personal_access_token_request"], payload: Mapping[str, Any]
    ) -> "PersonalAccessTokenRequestEvent": ...
    @overload
    @staticmethod
    def parse_obj(name: Literal["ping"], payload: Mapping[str, Any]) -> "PingEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["project_card"], payload: Mapping[str, Any]
    ) -> "ProjectCardEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["project"], payload: Mapping[str, Any]
    ) -> "ProjectEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["project_column"], payload: Mapping[str, Any]
    ) -> "ProjectColumnEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["projects_v2"], payload: Mapping[str, Any]
    ) -> "ProjectsV2Event": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["projects_v2_item"], payload: Mapping[str, Any]
    ) -> "ProjectsV2ItemEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["projects_v2_status_update"], payload: Mapping[str, Any]
    ) -> "ProjectsV2StatusUpdateEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["public"], payload: Mapping[str, Any]
    ) -> "PublicEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["pull_request"], payload: Mapping[str, Any]
    ) -> "PullRequestEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["pull_request_review_comment"], payload: Mapping[str, Any]
    ) -> "PullRequestReviewCommentEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["pull_request_review"], payload: Mapping[str, Any]
    ) -> "PullRequestReviewEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["pull_request_review_thread"], payload: Mapping[str, Any]
    ) -> "PullRequestReviewThreadEvent": ...
    @overload
    @staticmethod
    def parse_obj(name: Literal["push"], payload: Mapping[str, Any]) -> "PushEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["registry_package"], payload: Mapping[str, Any]
    ) -> "RegistryPackageEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["release"], payload: Mapping[str, Any]
    ) -> "ReleaseEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["repository_advisory"], payload: Mapping[str, Any]
    ) -> "RepositoryAdvisoryEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["repository"], payload: Mapping[str, Any]
    ) -> "RepositoryEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["repository_dispatch"], payload: Mapping[str, Any]
    ) -> "RepositoryDispatchEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["repository_import"], payload: Mapping[str, Any]
    ) -> "RepositoryImportEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["repository_ruleset"], payload: Mapping[str, Any]
    ) -> "RepositoryRulesetEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["repository_vulnerability_alert"], payload: Mapping[str, Any]
    ) -> "RepositoryVulnerabilityAlertEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["secret_scanning_alert"], payload: Mapping[str, Any]
    ) -> "SecretScanningAlertEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["secret_scanning_alert_location"], payload: Mapping[str, Any]
    ) -> "SecretScanningAlertLocationEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["secret_scanning_scan"], payload: Mapping[str, Any]
    ) -> "SecretScanningScanEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["security_advisory"], payload: Mapping[str, Any]
    ) -> "SecurityAdvisoryEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["security_and_analysis"], payload: Mapping[str, Any]
    ) -> "SecurityAndAnalysisEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["sponsorship"], payload: Mapping[str, Any]
    ) -> "SponsorshipEvent": ...
    @overload
    @staticmethod
    def parse_obj(name: Literal["star"], payload: Mapping[str, Any]) -> "StarEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["status"], payload: Mapping[str, Any]
    ) -> "StatusEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["sub_issues"], payload: Mapping[str, Any]
    ) -> "SubIssuesEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["team_add"], payload: Mapping[str, Any]
    ) -> "TeamAddEvent": ...
    @overload
    @staticmethod
    def parse_obj(name: Literal["team"], payload: Mapping[str, Any]) -> "TeamEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["watch"], payload: Mapping[str, Any]
    ) -> "WatchEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["workflow_dispatch"], payload: Mapping[str, Any]
    ) -> "WorkflowDispatchEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["workflow_job"], payload: Mapping[str, Any]
    ) -> "WorkflowJobEvent": ...
    @overload
    @staticmethod
    def parse_obj(
        name: Literal["workflow_run"], payload: Mapping[str, Any]
    ) -> "WorkflowRunEvent": ...

    @overload
    @staticmethod
    def parse_obj(
        name: EventNameType, payload: Mapping[str, Any]
    ) -> "WebhookEvent": ...

    @overload
    @staticmethod
    def parse_obj(name: str, payload: Mapping[str, Any]) -> "WebhookEvent": ...

    @staticmethod
    def parse_obj(
        name: Union[EventNameType, str], payload: Mapping[str, Any]
    ) -> "WebhookEvent":
        raise Exception

parse_obj = WN.parse_obj


P = ParamSpec("P")
R = TypeVar("R", covariant=True)


def wrap_non_retryable(fn: Callable[P, R]) -> Callable[P, R]:
    def _wrapped(*args: P.args, **kwargs: P.kwargs) -> R:
        raise Exception

    return _wrapped


safe_parse_obj = wrap_non_retryable(parse_obj)

```

But if there's only 10 overloads it finishes quickly.

---

_Comment by @MichaReiser on 2026-01-08 18:28_

Thank you so much for narrowing the issue down to a one file repro.

This sounds related to https://github.com/astral-sh/ty/issues/1337

---

_Label `performance` added by @MichaReiser on 2026-01-08 18:29_

---

_Label `overloads` added by @MichaReiser on 2026-01-08 18:29_

---

_Renamed from "Hang/OOM on generic function wrapping a method" to "Hang/OOM on generic function wrapping a method with many overloads" by @carljm on 2026-01-08 23:56_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2026-01-09 11:18_

---
