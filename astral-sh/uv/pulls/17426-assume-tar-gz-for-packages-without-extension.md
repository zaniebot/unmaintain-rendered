```yaml
number: 17426
title: Assume tar.gz for packages without extension
type: pull_request
state: open
author: ppalucha
labels: []
assignees: []
draft: true
base: main
head: pp/assume-tgz
created_at: 2026-01-12T21:08:12Z
updated_at: 2026-01-16T15:30:08Z
url: https://github.com/astral-sh/uv/pull/17426
synced_at: 2026-01-16T15:58:35Z
```

# Assume tar.gz for packages without extension

---

_@ppalucha_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This fixes issue with installing packages that contain URLs without explicit file extensions 

The example are GitHub releases that can be references with URLs like https://api.github.com/repos/pyro-ppl/pyro/tarball/dev (real example at https://github.com/pyro-ppl/pyro-api/blob/master/setup.py#L36).

If extension is not specified, we assume it is a tar.gz file, as it's a common pattern for GitHub.

Fixes: 17425
Co-authored-by: Copilot Agent

## Test Plan

<!-- How was it tested? -->

I added test with installing a package without file extension..


---

_Comment by @charliermarsh on 2026-01-12 22:44_

Appreciate the contribution! Unfortunately I don't think it's correct to assume `.tar.gz` for extension-less URLs. I think the right fix here is that we need to respect `Content-Disposition` headers in these cases.

---

_Comment by @konstin on 2026-01-13 07:58_

Not having the file name in the URL obscures what the underlying resource is: It can be a source archive, it can be wheel, and it could change in between downloads.

> If extension is not specified, we assume it is a tar.gz file, as it's a common pattern for GitHub.

Note that source tarballs from GitHub are not proper source distributions. They happen to work in pip and uv, but at least in uv that's more accidental than a proper feature and should not relied upon.

---

_Comment by @ppalucha on 2026-01-13 08:08_

> Not having the file name in the URL obscures what the underlying resource is: It can be a source archive, it can be wheel, and it could change in between downloads.
> 
> > If extension is not specified, we assume it is a tar.gz file, as it's a common pattern for GitHub.
> 
> Note that source tarballs from GitHub are not proper source distributions. They happen to work in pip and uv, but at least in uv that's more accidental than a proper feature and should not relied upon.

What if I update the code to use headers for deciding the content type?

---

_Comment by @konstin on 2026-01-13 08:08_

Both of those problems remain when using headers.

---

_Comment by @ppalucha on 2026-01-13 08:19_

> Both of those problems remain when using headers.

If you get content-disposition header specifying a filename with extension, is it not enough?

---

_Comment by @woodruffw on 2026-01-13 15:51_

> If you get content-disposition header specifying a filename with extension, is it not enough?

Unfortunately I think it's not enough -- at the moment a disposition of `tar+gz` is a decent proxy for a "source archive," but as @konstin notes we can't clearly distinguish that from a well-formed sdist (and in the future, `tar+gz` could potentially signal a wheel or other distribution format as well).

I think as a workaround, you could use one of GitHub's other archive endpoints, i.e. one that provides a suffix. For example, this should work:

```
https://github.com/pyro-ppl/pyro/archive/refs/heads/dev.zip
```

(I believe that that endpoint is also recommended in general, since I *think* it's not subject to the same global IP restrictions as the `api.github.com` ones.)

This is potentially something that uv could provide a hint/suggestion on 🙂 

---

_Converted to draft by @konstin on 2026-01-15 17:06_

---

_Comment by @ppalucha on 2026-01-16 09:25_

> > If you get content-disposition header specifying a filename with extension, is it not enough?
> 
> Unfortunately I think it's not enough -- at the moment a disposition of `tar+gz` is a decent proxy for a "source archive," but as @konstin notes we can't clearly distinguish that from a well-formed sdist (and in the future, `tar+gz` could potentially signal a wheel or other distribution format as well).

But we would have exactly the same problem for URLs with file extension - you don't really know what is behind tar.gz file. So using content disposition would just make it behave the same as URL with file extension. Also, content-disposition is not a mime-type/content-type, it's a filename with extension. So it's not `tar+gz` but rather `file.tar.gz` or `package-1.0.sdist`.

And, for the particular package I'm showing as an example, this is not a problem at all. The no-extensions URLs are used for testing dependencies, that we don't even plan to install. Just having them in metadata prevents uv from installing package that is otherwise perfectly installable/usable. 

---

_Comment by @woodruffw on 2026-01-16 15:25_

> But we would have exactly the same problem for URLs with file extension - you don't really know what is behind tar.gz file.

Yeah, but this is an issue at a different layer: PEP 625 says that an sdist has the `.tar.gz` extension, so that's an initial check. If the retrieved content then isn't actually a valid tarball, we'd fail subsequently. 

> Also, content-disposition is not a mime-type/content-type, it's a filename with extension. So it's not `tar+gz` but rather `file.tar.gz` or `package-1.0.sdist`.

Sorry, yeah, this was my misunderstanding. I think it'd be reasonable for uv to handle URLs that don't have an extension but that *do* have a valid `.tar.gz` filename in their `Content-Disposition` header's filename field. One nuance there is that it's unclear what uv should do in this case:

```http
GET /foo/bar/abc-1.2.3.tar.gz

200 OK
...
Content-Disposition: attachment; filename="def-4.5.6.tar.gz"
...
```

i.e., whether we should prefer the filename inferred from the URL or from the `Content-Disposition` header. I suspect the HTTP specs say the latter, but we should see what pip and other tools do (if anything) in this case.

---

_Comment by @woodruffw on 2026-01-16 15:30_

Looks like pip handles `Content-Disposition` and gives it precedence, at least: https://github.com/pypa/pip/blob/545eda389c41478e2f99d23212254d757d8c2cef/src/pip/_internal/network/download.py#L105-L117

---
