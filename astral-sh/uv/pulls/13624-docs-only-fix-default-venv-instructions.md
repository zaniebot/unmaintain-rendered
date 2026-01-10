```yaml
number: 13624
title: "Docs only: Fix default venv instructions"
type: pull_request
state: closed
author: stevenae
labels: []
assignees: []
base: main
head: patch-2
created_at: 2025-05-23T18:53:36Z
updated_at: 2025-05-27T15:17:19Z
url: https://github.com/astral-sh/uv/pull/13624
synced_at: 2026-01-10T11:10:42Z
```

# Docs only: Fix default venv instructions

---

_Pull request opened by @stevenae on 2025-05-23 18:53_

## Summary

Existing documentation is incorrect and leads to overwriting of existing `.venv` with blank `.venv`, see https://github.com/astral-sh/uv/issues/1472

## Test Plan

Markdown preview in GitHub

---

_Comment by @charliermarsh on 2025-05-23 18:55_

These docs seem correct to me. It’s illustrating that the uv pip install automatically installs into the .venv created in the preceding command, right?

---

_Comment by @stevenae on 2025-05-23 18:59_

Thank you for the quick reponse!

Earlier in the docs, it says `uv venv` will create `.venv`.  

This diff deletes text saying `When using the default virtual environment name, uv will automatically find and use the virtual
 environment during subsequent invocations.`

To me, the deleted text implies that if you have a `.venv` already, `uv venv` will use that virtual environment. Instead, it overwrites it.

---

_Comment by @stevenae on 2025-05-23 19:01_

Edited original summary to clarify that issue is overwriting `.venv` with blank, not deleting.

---

_Comment by @stevenae on 2025-05-23 22:44_

An alternative would be adding documentation about the current behavior of replacing the extant venv with a blank one. Happy to add that instead. 

---

_Comment by @zanieb on 2025-05-27 14:43_

Sorry I'm not sure I get this change, it's removing an explanation of how `uv pip install` works, which includes environment creation for completeness. If you want to add docs about replacing the existing virtual environment, that seems fine — but this isn't the right place for it.

---

_Comment by @stevenae on 2025-05-27 14:48_

Okay! What would be the right place for that?

On Tue, May 27, 2025, 10:43 AM Zanie Blue ***@***.***> wrote:

> *zanieb* left a comment (astral-sh/uv#13624)
> <https://github.com/astral-sh/uv/pull/13624#issuecomment-2912788843>
>
> Sorry I'm not sure I get this change, it's removing an explanation of how uv
> pip install works, which includes environment creation for completeness.
> If you want to add docs about replacing the existing virtual environment,
> that seems fine — but this isn't the right place for it.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/pull/13624#issuecomment-2912788843>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAFZOHLKU2AEOXZNWBNY7ML3AR22LAVCNFSM6AAAAAB5ZNQFLSVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDSMJSG44DQOBUGM>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @zanieb on 2025-05-27 14:51_

In https://docs.astral.sh/uv/pip/environments/#creating-a-virtual-environment

It could be a `!!! note` admonition, or just another short paragraph.

---

_Comment by @stevenae on 2025-05-27 15:17_

Put an edit up here: https://github.com/astral-sh/uv/pull/13684

Thank you!

---

_Closed by @stevenae on 2025-05-27 15:17_

---
