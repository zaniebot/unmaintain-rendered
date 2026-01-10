```yaml
number: 14253
title: "Docs: add instructions for publishing to JFrog's Artifactory"
type: pull_request
state: merged
author: ondrej-profant-dtml
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2025-06-25T07:50:46Z
updated_at: 2025-06-30T13:58:56Z
url: https://github.com/astral-sh/uv/pull/14253
synced_at: 2026-01-10T06:53:01Z
```

# Docs: add instructions for publishing to JFrog's Artifactory

---

_Pull request opened by @ondrej-profant-dtml on 2025-06-25 07:50_

## Summary

Add instructions for publishing to JFrog's Artifactory into [documentation](https://docs.astral.sh/uv/guides/integration/alternative-indexes/).

Related issues:
https://github.com/astral-sh/uv/issues/9845
https://github.com/astral-sh/uv/issues/10193

## Test Plan

I ran the documentation locally and use npx prettier.


---

_Review requested from @jtfmumm by @jtfmumm on 2025-06-25 15:39_

---

_Comment by @jtfmumm on 2025-06-25 15:50_

Thanks for creating this. I verified the instructions work. 

Would you mind updating the documentation to more closely resemble the guides for the other registries (at least for the `### Publishing packages to <registry>` section)? 

---

_Comment by @ondrej-profant-dtml on 2025-06-26 07:56_

@jtfmumm done

---

_Comment by @jtfmumm on 2025-06-26 09:57_

Thank you for making those updates. Could you also replace the specific `pypi-general`-type values in the URLs with `<repo>`?

---

_Comment by @ondrej-profant-dtml on 2025-06-26 12:00_

@jtfmumm :+1:  I did not know that are specific. Fixed.

The tests were ok. These 3 failed are not connected with this PR changes.

---

_Comment by @jtfmumm on 2025-06-26 12:11_

I'll defer to @zanieb for final judgment on the content style.

---

_Assigned to @zanieb by @zanieb on 2025-06-26 16:06_

---

_Comment by @BartSchuurmans on 2025-06-29 10:49_

Not sure if it should be included here, but I use a `~/.netrc` file to provide credentials for Artifactory and prefer it over environment variables, since I can set it up once and then it just always works.

---

_Comment by @ondrej-profant-dtml on 2025-06-30 07:43_

> Not sure if it should be included here, but I use a `~/.netrc` file to provide credentials for Artifactory and prefer it over environment variables, since I can set it up once and then it just always works.

I see this PR as JFrog specific. 

I agree that we need page about authentication and best practice of storing credentials or tokens. But I don't feel like right person for this job.

---

_@zanieb reviewed on 2025-06-30 13:44_

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:429 on 2025-06-30 13:44_

Does this work with `UV_PUBLISH_TOKEN`? Do they just ignore the username?

---

_@zanieb reviewed on 2025-06-30 13:46_

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:414 on 2025-06-30 13:46_

```suggestion
!!! important

    If you use `--token "$JFROG_TOKEN"` or `UV_PUBLISH_TOKEN` with JFrog, you will receive a
    401 Unauthorized error as JFrog requires an empty username but uv passes `__token__` for as
    the username when `--token` is used.
```

---

_@zanieb reviewed on 2025-06-30 13:46_

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:429 on 2025-06-30 13:46_

Ah I see this is mentioned above. Maybe we can elevate that? https://github.com/astral-sh/uv/pull/14253/files#r2175124341

---

_@ondrej-profant-dtml reviewed on 2025-06-30 13:47_

---

_Review comment by @ondrej-profant-dtml on `docs/guides/integration/alternative-indexes.md`:429 on 2025-06-30 13:47_

This `uv publish -t "$JFROG_TOKEN"` does not work. Thats the reason why I see need for extending docs here. 

---

_Label `documentation` added by @zanieb on 2025-06-30 13:52_

---

_@zanieb approved on 2025-06-30 13:55_

---

_Comment by @zanieb on 2025-06-30 13:55_

Thanks!

---

_Merged by @zanieb on 2025-06-30 13:58_

---

_Closed by @zanieb on 2025-06-30 13:58_

---
