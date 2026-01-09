---
number: 10202
title: Client did not publish a checksum value when using uv publish
type: issue
state: closed
author: OliverKleinBST
labels:
  - external
assignees: []
created_at: 2024-12-27T17:17:05Z
updated_at: 2025-09-04T15:47:35Z
url: https://github.com/astral-sh/uv/issues/10202
synced_at: 2026-01-07T13:12:18-06:00
---

# Client did not publish a checksum value when using uv publish

---

_Issue opened by @OliverKleinBST on 2024-12-27 17:17_

When using uv publish to publish a python package to pypi on artifactory, I get a warning that the client did not publish a checksum value.

uv version: 0.5.13

---

_Label `preview` added by @charliermarsh on 2024-12-27 17:47_

---

_Assigned to @konstin by @charliermarsh on 2024-12-27 17:47_

---

_Comment by @charliermarsh on 2024-12-27 17:47_

\cc @konstin when you're back

---

_Comment by @konstin on 2025-01-07 11:10_

Can you share the command that you ran (with urls redacted) and the error you got?

---

_Unassigned @konstin by @konstin on 2025-01-07 11:10_

---

_Comment by @OliverKleinBST on 2025-01-07 11:54_

I was going to replace 

```
TWINE_REPOSITORY_URL: https://artifactory.xyz.com/artifactory/api/pypi/xyz-pypi-local
twine upload dist/* --username ${{ secrets.JFROG_USER }} --password ${{ secrets.JFROG_TOKEN }}
```

with
`uv publish --publish-url $TWINE_REPOSITORY_URL --username ${{ secrets.JFROG_USER }} --password ${{ secrets.JFROG_TOKEN }}
`

which publishes the package onto private PyPI on artifactory.
Both commands work fine with respect to that the packages arrive.

However, when I go to the UI of artifactory where my package is located, I see in the package metadata the md5 and some SHA of the package, and artifactory tells me for the packages I published with uv that "client did not publish a checksum value" and I manually need to checkmark the package to make it trusted whereas this is not the case for twine.

see also [https://jfrog.com/help/r/what-are-client-checksum-server-checksum-and-checksum-policy-in-local-repositories/what-are-client-checksum-server-checksum-and-checksum-policy-in-local-repositories](https://jfrog.com/help/r/what-are-client-checksum-server-checksum-and-checksum-policy-in-local-repositories/what-are-client-checksum-server-checksum-and-checksum-policy-in-local-repositories) and [https://jfrog.com/help/r/why-am-i-getting-client-did-not-publish-a-checksum-value-for-npm-packages/why-am-i-getting-client-did-not-publish-a-checksum-value...-for-npm-packages](https://jfrog.com/help/r/why-am-i-getting-client-did-not-publish-a-checksum-value-for-npm-packages/why-am-i-getting-client-did-not-publish-a-checksum-value...-for-npm-packages)

The same problem I had some time back when I was using REST API, which I found could be solved by:
[https://stackoverflow.com/questions/46703183/fix-checksum-in-artifactory-when-uploading-file-through-rest-api](https://stackoverflow.com/questions/46703183/fix-checksum-in-artifactory-when-uploading-file-through-rest-api)

Twine may solve the problem with that HashManager code:
[https://github.com/pypa/twine/blob/2386ca5300cd7bde59432834d362c07de61e9a53/twine/package.py#L377](https://github.com/pypa/twine/blob/2386ca5300cd7bde59432834d362c07de61e9a53/twine/package.py#L377)




---

_Assigned to @konstin by @konstin on 2025-01-07 11:56_

---

_Comment by @konstin on 2025-01-08 12:25_

Do you know which header and which hash algorithm artifactory reads? We are sending `sha256_digest` as part of the form-data as twine does. The help article mentions `X-Checksum`, though I don't see twine setting that.

---

_Comment by @OliverKleinBST on 2025-01-08 12:33_

I just checked - this is what it says, and indeed the 256 one is in but it seems to expect all checksums not only one:

**Checksums**

```
SHA-256:	b5d69b2543ecf56d4d74e236fa9482eb43aa174f9b176d12a0d6cbfe9abdd211 (Uploaded: Identical)
SHA-1: abede45dd45673b9523b58bb1e7b20b5a8c3390c (Uploaded: None)
MD5: 3ec27736db428b55897f8344ca7ce718 (Uploaded: None)

Client did not publish a checksum value. If you trust the uploaded artifact you can accept the actual checksum by clicking the 'Fix Checksum' button.
```

---

_Comment by @konstin on 2025-01-08 12:36_

I'm hesitant to add MD5 and SHA-1 hashes, both have been broken (https://en.wikipedia.org/wiki/MD5#Security, https://shattered.io/) and cannot be relied on for security. Is there a reason artifactory doesn't use the SHA-256 alone?

---

_Label `upstream` added by @konstin on 2025-01-08 12:36_

---

_Comment by @OliverKleinBST on 2025-01-08 12:56_

> I'm hesitant to add MD5 and SHA-1 hashes, both have been broken (https://en.wikipedia.org/wiki/MD5#Security, https://shattered.io/) and cannot be relied on for security. Is there a reason artifactory doesn't use the SHA-256 alone?

Good question, I found some FAQ in this case for npm but same problem
https://jfrog.com/help/r/why-am-i-getting-client-did-not-publish-a-checksum-value-for-npm-packages/why-am-i-getting-client-did-not-publish-a-checksum-value...-for-npm-packages

which says

```
If binaries are being deployed using a CI server, the CI server needs to be configured to pass the checksums using the following headers in PUT request:

X-Checksum-Sha1: sha1Value, X-Checksum-Sha256: sha256Value, X-Checksum: checksum value
```

---

_Unassigned @konstin by @konstin on 2025-01-08 13:32_

---

_Comment by @geoffrey-g-delhomme on 2025-02-25 12:30_

Any update on this issue? Indeed, using Artifactory it raises a warning: Client did not publish a checksum value. If you trust the uploaded artifact you can accept the actual checksum by clicking the 'Fix Checksum' button.
Thank you

---

_Comment by @konstin on 2025-02-25 12:58_

@geoffrey-g-delhomme As far as I can tell this needs to be fixed on the artifactory side, uv is sending `sha256_digest` as part of the form_data.

---

_Comment by @OliverKleinBST on 2025-02-25 13:22_

I would kind of agree that in the end it is some kind of issue on artifactory side - which is known there since ages.
I don't expect this to be solved anytime soon if ever in artifactory, while other publish tools like twine seem to send all 3 hashes.

Hence, even though maybe redundant, it would be great if uv offers that functionality, maybe optional, as otherwise it blocks our enterprise from adopting uv publlish.

---

_Comment by @zanieb on 2025-02-25 15:13_

I'm vaguely okay with the idea of opt-in to insecure hashes? Like `--insecure-checksums`?

---

_Comment by @geoffrey-g-delhomme on 2025-02-26 12:58_

Thank you @konstin for reminding the root cause. However, I am in the exact situation depicted by @OliverKleinBST. @zanieb 's suggestion would be enough waiting for a fix on Artifactory side (if ever). Moreover, adding a specifc paragraph in the documentation in order to publish on Artifactory (highlighting the issue on artifactory's side) would also help to ground `uv` in already set up development environments, as Artifactory is a widely used solution in the industry (at least mine).

---

_Comment by @janaroj on 2025-04-04 17:21_

having the same issue

---

_Comment by @OliverKleinBST on 2025-07-24 09:07_

Will there be any progress on that topic, like with the idea of optional parameter?

---

_Comment by @kohlrabi on 2025-07-25 07:21_

@OliverKleinBST I just noticed this as well, does it prohibit you from using the published package or is it just an annoyance in the artifactory browser?

---

_Referenced in [pypa/twine#1258](../../pypa/twine/issues/1258.md) on 2025-08-08 19:59_

---

_Comment by @woodruffw on 2025-08-11 14:52_

I'm looking into this now, and I'm pretty convinced that Artifactory's behavior here is bugged:

* `uv publish` provides a SHA-256 digest, which is sufficient for a cryptographic integrity check on the index side against the uploaded data.
* There's no intelligible security benefit to requiring both a SHA-256 *and* a MD5 digest.
* The `md5_digest` form field is considered optional by PyPI itself, and has been for a long time. Uploadable indices should *not* reject uploads just because it isn't present.

Given all of these, I think the right course of action here is actually to have `twine` stop sending an `md5_digest`, at which point `twine` and `uv publish` would be more consistent about what they send to uploadable indices. I think that's preferable over having uv add MD5 hashing here, since doing so papers over a real bug on JFrog's side that is (IMO) worth fixing on their end.

As part of the above, I think it'd be great if users who have support contracts with JFrog would ping them on this -- I think they'd benefit from hearing from their users on poor UX/DX here.

---

_Comment by @woodruffw on 2025-08-12 16:32_

The next release of `twine` will also stop sending `md5_digest` to indices, meaning that uv and twine will behave consistently (and compatibly) in this regard.



---

_Label `preview` removed by @konstin on 2025-08-18 09:19_

---

_Comment by @OliverKleinBST on 2025-08-21 11:43_

I did some checks of downloading / installing packages with pip and uv pip, with and without --require-hashes and on CLI I cannot see any problems or (verbose level) warnings when I point to a package that only has the SHA256 but no SHA1 and MD5. So, eventually this is only a GUI issue in JFROG as otherwise they should send a 404:
_If a checksum is not provided, and you do not address the problem by using "Fix Checksum," then when a client attempts to send a get request to access that checksum file, Artifactory will return a 404 (not found) error_

---

_Comment by @woodruffw on 2025-09-04 15:47_

Updating: the latest release of twine (6.2.0) has also dropped `md5_digest` support, so I believe there's no longer a useful feature parity target for `uv publish` to match here.

Given that, I think this is safe to close.

---

_Closed by @woodruffw on 2025-09-04 15:47_

---
