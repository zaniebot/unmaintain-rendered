```yaml
number: 10278
title: Add AWS Lambda integration guide
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/aws-lambda
created_at: 2025-01-02T18:07:06Z
updated_at: 2025-01-13T00:36:04Z
url: https://github.com/astral-sh/uv/pull/10278
synced_at: 2026-01-12T16:09:12Z
```

# Add AWS Lambda integration guide

---

_@charliermarsh_

## Summary

This includes instructions to:

- Deploy a standalone application.
- Deploy an application that depends on local libraries (a workspace).
- Deploy via Docker.
- Deploy via zip.

There's an accompanying Git repository here: https://github.com/astral-sh/uv-aws-lambda-example.

Closes https://github.com/astral-sh/uv/issues/8935.


---

_@charliermarsh reviewed on 2025-01-02 18:07_

---

_Review comment by @charliermarsh on `docs/guides/integration/aws-lambda.md`:1 on 2025-01-02 18:07_

There are two examples here: a standalone application, and a workspace (with an application and a library).

In the linked Git repo, I only included the former. Should I put the latter somewhere? Only include the latter? Not sure.


---

_Review comment by @charliermarsh on `docs/guides/integration/aws-lambda.md`:138 on 2025-01-02 18:08_

Not sure how much to cover here...?

---

_@charliermarsh reviewed on 2025-01-02 18:08_

---

_Label `documentation` added by @charliermarsh on 2025-01-02 18:18_

---

_Marked ready for review by @charliermarsh on 2025-01-02 18:19_

---

_Converted to draft by @charliermarsh on 2025-01-02 18:33_

---

_Marked ready for review by @charliermarsh on 2025-01-02 18:53_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-02 18:53_

---

_Review requested from @konstin by @charliermarsh on 2025-01-02 18:53_

---

_Review comment by @konstin on `docs/guides/integration/aws-lambda.md`:43 on 2025-01-03 08:43_

Should we add (lower) bounds?

Similarly for the docker build, i got a `error: unexpected argument '--no-emit-workspace' found` the first time because a had an older uv image in cache.

---

_Review comment by @konstin on `docs/guides/integration/aws-lambda.md`:93 on 2025-01-03 08:47_

```suggestion
# Enable bytecode compilation for faster startup.
```

---

_Review comment by @konstin on `docs/guides/integration/aws-lambda.md`:392 on 2025-01-03 08:56_

```suggestion
   --assume-role-policy-document '{"Version": "2012-10-17", "Statement": [{ "Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'
```

---

_@konstin approved on 2025-01-03 09:00_

I followed along with the local commands, though i did not try to deploy to aws.

---

_Review comment by @samypr100 on `docs/guides/integration/aws-lambda.md`:391 on 2025-01-04 00:44_

```suggestion
   --role-name my-lambda-role \
```

The prev `aws lambda create-function` command uses my-lambda-role, but we can also change the prev command to lambda-ex instead

---

_@samypr100 reviewed on 2025-01-04 00:44_

---

_@fitz-vivodyne reviewed on 2025-01-06 14:12_

---

_Review comment by @fitz-vivodyne on `docs/guides/integration/aws-lambda.md`:356 on 2025-01-06 14:12_

Might want to include a comment indicating the ARM/Graviton platform name here, and possibly mention the ARM docker images above (public.ecr.aws/lambda/python:3.13-arm64)

---

_@fitz-vivodyne reviewed on 2025-01-06 14:13_

---

_Review comment by @fitz-vivodyne on `docs/guides/integration/aws-lambda.md`:243 on 2025-01-06 14:13_

Comment out of sync with dep?

---

_@zanieb reviewed on 2025-01-07 18:07_

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:36 on 2025-01-07 18:07_

As a minor note, you could move this out and use a `!!! note` admonition instead so you can link to the library docs. Don't feel strongly though.

---

_@zanieb reviewed on 2025-01-07 18:08_

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:43 on 2025-01-07 18:08_

I would avoid it, just because of the maintenance burden. I don't feel strongly though — it's reasonable either way.

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:88 on 2025-01-07 18:09_

Here, we _could_ pin to the current uv version and add this to the files that are updated on release.

---

_@zanieb reviewed on 2025-01-07 18:09_

---

_@zanieb reviewed on 2025-01-07 18:10_

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:93 on 2025-01-07 18:10_

We may want to change this in the Docker guide too.

---

_@zanieb reviewed on 2025-01-07 18:10_

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:93 on 2025-01-07 18:10_

ref https://github.com/astral-sh/uv-docker-example/blob/a14ebc89e3a5e5b33131284968d8969ae054ed0d/Dockerfile#L7-L8

---

_@zanieb reviewed on 2025-01-07 18:14_

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:219 on 2025-01-07 18:14_

Should we include `uv add ./library` above?

---

_@zanieb reviewed on 2025-01-07 18:16_

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:395 on 2025-01-07 18:16_

```suggestion
Or, update an existing function with:
```

---

_@zanieb approved on 2025-01-07 18:16_

---

_@zanieb reviewed on 2025-01-07 18:17_

---

_Review comment by @zanieb on `docs/guides/integration/aws-lambda.md`:1 on 2025-01-07 18:17_

I think only including the former is fine in the example for now. We can create a separate `uv-aws-lambda-workspace-example` later if we need to?

---

_Comment by @charliermarsh on 2025-01-07 18:42_

Thanks all!

---

_Review comment by @charliermarsh on `docs/guides/integration/aws-lambda.md`:356 on 2025-01-07 18:42_

Good call. I added it in a tip.

---

_@charliermarsh reviewed on 2025-01-07 18:42_

---

_Merged by @charliermarsh on 2025-01-07 18:50_

---

_Closed by @charliermarsh on 2025-01-07 18:50_

---

_Branch deleted on 2025-01-07 18:50_

---
