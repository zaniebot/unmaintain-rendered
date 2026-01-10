```yaml
number: 4857
title: "Docs: Add guide for authenticating to Azure Artifacts"
type: pull_request
state: merged
author: benjamin-hodgson
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-07-07T15:18:18Z
updated_at: 2024-07-15T16:07:03Z
url: https://github.com/astral-sh/uv/pull/4857
synced_at: 2026-01-10T13:42:52Z
```

# Docs: Add guide for authenticating to Azure Artifacts

---

_Pull request opened by @benjamin-hodgson on 2024-07-07 15:18_

## Summary

As discussed in #3542 - there has been some confusion about how to get `uv` to work with ADO Artifacts so I'm adding a quick guide.

## Test Plan

Smoke-tested the examples on my machine.


---

_Comment by @benjamin-hodgson on 2024-07-07 15:29_

I’ve written the examples using `UV_EXTRA_INDEX_URL` but of course that’s discouraged because of dependency confusion attacks, lemme know if you think I should change it.

---

_Renamed from "Add guide for authenticating to Azure Artifacts" to "Docs: Add guide for authenticating to Azure Artifacts" by @benjamin-hodgson on 2024-07-07 15:30_

---

_Assigned to @zanieb by @zanieb on 2024-07-07 16:53_

---

_Label `documentation` added by @zanieb on 2024-07-07 16:53_

---

_Review comment by @Rogdham on `docs/guides/using-azure-artifacts.md`:14 on 2024-07-08 09:17_

I believe you should also be able to manually add the PAT into `keyring` (without the `artifacts-keyring` plugin) and authenticate transparently with Keyring?

Minus the issues we had in #4056 / #4583

---

_Review comment by @Rogdham on `docs/guides/using-azure-artifacts.md`:15 on 2024-07-08 09:18_

I believe what the `artifacts-keyring` plugin is doing under the hood is creating a PAT and returning it.

Could you confirm that? Do you think it's worth documenting?

---

_@Rogdham reviewed on 2024-07-08 09:18_

---

_@benjamin-hodgson reviewed on 2024-07-09 02:48_

---

_Review comment by @benjamin-hodgson on `docs/guides/using-azure-artifacts.md`:15 on 2024-07-09 02:48_

The [credential provider tool](https://github.com/microsoft/artifacts-credprovider) has a few different modes and I don’t know whether _all_ of them wind up creating a PAT behind the scenes. The interactive scenario does seem to.

I wanted to keep this doc relatively short to help people get up and running, but I could link to the repo for the cred-provider so there’s enough of a paper trail for people to learn more, what do you think?

---

_@benjamin-hodgson reviewed on 2024-07-09 02:51_

---

_Review comment by @benjamin-hodgson on `docs/guides/using-azure-artifacts.md`:14 on 2024-07-09 02:51_

Haven’t tested that workflow myself and I don’t know how common it would be (seems to me that you’d either use the plugin, if you have the wherewithal to preinstall stuff, or not use Keyring at all?). I can certainly give it a go tomorrow and add a note if you think it’d be worthwhile. Let me know

---

_@Rogdham reviewed on 2024-07-14 10:27_

---

_Review comment by @Rogdham on `docs/guides/using-azure-artifacts.md`:15 on 2024-07-14 10:27_

I can confirm that if the following environment variable is set as follows, a PAT is not created behind the scenes:

```
NUGET_CREDENTIALPROVIDER_VSTS_TOKENTYPE=SelfDescribing
```

Here is the relevant help from the credential provider tool (`dotnet exec CredentialProvider.Microsoft.dll -h`):
```
Token Type
    NUGET_CREDENTIALPROVIDER_VSTS_TOKENTYPE
        Specify 'Compact' to generate a Personal Access Token, which may
        have a long validity period as it can easily be revoked from the UI,
        and sends a notification mail on creation.
        Specify 'SelfDescribing' to generate a shorter-lived JWT token,
        which does not appear in any UI or notifications
        and is more difficult to revoke.
        By default PATs are generated rather than JWTs,
        unless authentication can be performed non-interactively.
```

---

_Review comment by @benjamin-hodgson on `docs/guides/using-azure-artifacts.md`:15 on 2024-07-14 13:20_

Got it. I would prefer to keep this doc as just a high-level getting started guide, I don’t think we should include advanced configuration like that here. I’ve added a link to the cred provider’s GitHub repo in 50f8af659c74ceb8dbbee001746e4efe160b9b66 so people can learn more if they want to.

---

_@benjamin-hodgson reviewed on 2024-07-14 13:20_

---

_@Rogdham reviewed on 2024-07-15 08:59_

---

_Review comment by @Rogdham on `docs/guides/using-azure-artifacts.md`:15 on 2024-07-15 08:59_

Looks good to me :+1: 

---

_@zanieb approved on 2024-07-15 16:06_

Thank you!

---

_Merged by @zanieb on 2024-07-15 16:06_

---

_Closed by @zanieb on 2024-07-15 16:06_

---

_Label `preview` added by @zanieb on 2024-07-15 16:07_

---
