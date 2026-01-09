---
number: 11652
title: "uv build creates dist/.gitignore; doesn't play nice with PYPI trusted publishing"
type: issue
state: closed
author: dimaqq
labels:
  - needs-mre
assignees: []
created_at: 2025-02-20T05:13:12Z
updated_at: 2025-06-05T04:48:22Z
url: https://github.com/astral-sh/uv/issues/11652
synced_at: 2026-01-07T13:12:18-06:00
---

# uv build creates dist/.gitignore; doesn't play nice with PYPI trusted publishing

---

_Issue opened by @dimaqq on 2025-02-20 05:13_

### Summary

A fresh project `mkdir hack; cd hack; uv init; uv build` creates a `.gitignore` file under `dist/`.

It is the `uv build` step that does it. Doesn't seem to depend on the build backend.

The issue with this file is that PYPI trusted publishing will try to upload everything under `dist/`, including the `.gitignore` file, which will be rejected, and the user is left in total confusion.

Typically users don't have access to the ephemeral GHA runners (hacks exist), and users can't run trusted publishing from the local checkout either.

I've only figured this out by adding `ls -halt dist/` to the workflow, as I suspected I've messed something up.

uv 0.6.1



### Platform

any (tested: macos, linux)

### Version

0.6.1

### Python version

3.13

---

_Label `bug` added by @dimaqq on 2025-02-20 05:13_

---

_Comment by @konstin on 2025-02-20 08:43_

What trusted publishing setup are you using? `uv publish` supports trusted publishing and ignores the `.gitignore` file (and all other unknown files).

---

_Comment by @charliermarsh on 2025-02-20 20:00_

@konstin -- It looks like they're using the GitHub Action? You can see here: https://github.com/dimaqq/otlp-test-data/commit/24d5b31411dd5d124d5b0cac2fd7441960cb5c4a

---

_Assigned to @konstin by @charliermarsh on 2025-02-20 20:00_

---

_Comment by @dimaqq on 2025-02-22 06:35_

Yes the pypa gh action.

---

_Comment by @konstin on 2025-02-26 10:01_

It's unfortunate that `pypa/gh-action-pypi-publish` does not handle `.gitginore` files automatically. You can either remove the gitignore file (as you are doing) or try `uv publish`, which also supports trusted publishing but also skip unknown files.

---

_Comment by @dimaqq on 2025-02-26 11:26_

I’ll try that!

---

_Comment by @ivan-kleshnin on 2025-03-05 15:31_

Yeah, why this extra file. Wouldn't `dist/` line in the root/project-level `.gitignore` work? 👀 

---

_Comment by @konstin on 2025-03-05 16:01_

Yes, the top-level `.gitignore` can also be used. We use a `.gitignore` with `*` for all our directories that we consider ephemeral (that includes e.g. `.venv` and all other virtual environments created with `uv venv`) so that the user doesn't need to handle the excludes manually, while also don't edit the user's top-level `.gitignore` unnecessarily.

---

_Comment by @djcopley on 2025-03-28 21:08_

I understand why it's there, but I'd like an option to disable the production of a .gitignore. I have a pipeline that produces artifacts for external consumption and I'd rather not need to add an additional script action to delete the file.

---

_Comment by @dimaqq on 2025-03-29 04:16_

Given that:
- uv already generates a top-level ignore file that includes dist 
- other build tools don’t create helper files in dist (that’s on purpose!)

My vote is that uv shouldn’t produce this file.

Current behaviour is hard to teach.

---

_Comment by @konstin on 2025-04-08 13:19_

I tried to reproduce this, but in my test everything passes with pypa/gh-action-pypi-publish@76f52bc884231f62b9a034ebfe128415bbaabdfc (#12742, https://github.com/astral-sh/uv/actions/runs/14334239283/job/40177272548?pr=12742)

---

_Label `bug` removed by @konstin on 2025-04-08 13:19_

---

_Label `needs-mre` added by @konstin on 2025-04-08 13:19_

---

_Unassigned @konstin by @konstin on 2025-04-08 13:19_

---

_Comment by @dimaqq on 2025-04-09 00:48_

it's a bit hard to see what's going on in logs... would you mind adding something like `find .` or equivalent step to check if there is in fact a `dist/.gitignore` file?

---

_Referenced in [astral-sh/uv#12742](../../astral-sh/uv/pulls/12742.md) on 2025-04-09 10:19_

---

_Comment by @dimaqq on 2025-06-05 04:47_

Confirming that at a high level it's no longer an issue.

Attestation is still includes the newly minted `.gitignore` file:

Edit: I guess not the attestation, rather the verbose log of the action. Clearly the attestation cannot include itself.

```
2025-06-05T04:43:58.2451206Z Transparency log entry created with index: 230061262
2025-06-05T04:43:58.3724525Z Showing hash values of files to be uploaded:
2025-06-05T04:43:58.3725604Z /github/workspace/dist/otlp_json-1.0.0-py3-none-any.whl
2025-06-05T04:43:58.3726179Z 
2025-06-05T04:43:58.3726718Z SHA256: 4997fb35aec43279d1d3a753fd74a6bd3ea6743d5345c824d89cd220a17aef37
2025-06-05T04:43:58.3727867Z MD5: 09f7b3bc412d0597452e5cfc2af81406
2025-06-05T04:43:58.3728821Z BLAKE2-256: 368f3a4f5087b7bd96acb4c6db47a66183c354e2ee5285a9c59a1aca19fefb27
2025-06-05T04:43:58.3729606Z 
2025-06-05T04:43:58.3729901Z /github/workspace/dist/otlp_json-1.0.0.tar.gz
2025-06-05T04:43:58.3730386Z 
2025-06-05T04:43:58.3730841Z SHA256: ab000a0d291a03357691e897a766dbaaa1d54ef3ed363b0c5cd8d436f44eea91
2025-06-05T04:43:58.3731748Z MD5: 8cf1539c8ac045355719f066fa733b31
2025-06-05T04:43:58.3732685Z BLAKE2-256: bbf0e2c3aea9687a37a9fd30ad30145ee292e02a93df9130bce0b5aed740a7b1
2025-06-05T04:43:58.3733304Z 
2025-06-05T04:43:58.3733744Z /github/workspace/dist/otlp_json-1.0.0-py3-none-any.whl.publish.attestation
2025-06-05T04:43:58.3734364Z 
2025-06-05T04:43:58.3734755Z SHA256: 6eeb33a6970b61e044c840eba6574dffea1ac024eb36410a03d09803ab72adfc
2025-06-05T04:43:58.3735489Z MD5: bef6db4819972c705e19656822f1fa9f
2025-06-05T04:43:58.3736205Z BLAKE2-256: 7e6f120085d5527755eafee378df429ce4adeb0e4713959dc259146f0ea3521d
2025-06-05T04:43:58.3736785Z 
2025-06-05T04:43:58.3737137Z /github/workspace/dist/otlp_json-1.0.0.tar.gz.publish.attestation
2025-06-05T04:43:58.3737759Z 
2025-06-05T04:43:58.3737989Z SHA256: d0100bbf6d311f373025864f3c3c5300146276ff2374919454a7766ac3bc9b4f
2025-06-05T04:43:58.3738450Z MD5: c9b3851b5ba4293294587b348feb436e
2025-06-05T04:43:58.3738906Z BLAKE2-256: 162075a27c4833455f6e886e2f3bafc320187c5ce69d3a5ffe117799ee006a02
2025-06-05T04:43:58.3739545Z 
2025-06-05T04:43:58.3739669Z /github/workspace/dist/.gitignore
2025-06-05T04:43:58.3739872Z 
2025-06-05T04:43:58.3740119Z SHA256: 684888c0ebb17f374298b65ee2807526c066094c701bcc7ebbe1c1095f494fc1
2025-06-05T04:43:58.3740571Z MD5: 3389dae361af79b04c9c8e7057f60cc6
2025-06-05T04:43:58.3741021Z BLAKE2-256: 395a122eb1402bf256d86e3fa44764cf9acc559017a00b2b9ee12498e73ef2b5
2025-06-05T04:43:58.3741395Z 
```

However, the action doesn't attempt to push it to PYPI, and thus doesn't trip on it.

---

_Closed by @dimaqq on 2025-06-05 04:47_

---
