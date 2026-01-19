```yaml
number: 17526
title: "Respect `Content-Disposition` for direct URL installations?"
type: issue
state: open
author: woodruffw
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2026-01-16T16:16:15Z
updated_at: 2026-01-19T11:53:57Z
url: https://github.com/astral-sh/uv/issues/17526
synced_at: 2026-01-19T12:32:42Z
```

# Respect `Content-Disposition` for direct URL installations?

---

_@woodruffw_

This is a breakout from #17426, since we need to obtain clarity/consensus on an approach here.

TL;DR: URLs don't necessarily have a clear filename suffix. When this happens, uv doesn't have a filename to use for the URL's content, which could be either a sdist or a wheel. Consequently, the URL is rejected.

One thing we could do (for a subset of scenarios) is to honor the `Content-Disposition` header, which _can_ include a `filename` field. 

pip appears to do this, and gives it precedence over any inferred filename:

https://github.com/pypa/pip/blob/545eda389c41478e2f99d23212254d757d8c2cef/src/pip/_internal/network/download.py#L105-L117

Some things we need clarity on (these may already have answers, I just haven't collected the information together):

1. How does this interact with other PEP 508 (and non-standard) distribution name/filename specification mechanisms for URLs? 
2. There are other `Content-Distribution` mechanisms for specifying filenames, e.g. `filename*`. We should see if pip handles these, if they do the precedence between them correctly, etc.


---

_Label `needs-decision` added by @woodruffw on 2026-01-16 16:16_

---

_Comment by @notatallshaw on 2026-01-16 17:53_

FYI, I'm following this discussion, if you think this puts pip out of standards compliance I will take a look. Although this is hairy enough I'm not likely going to change the behavior beyond logging a warning when content disposition does not match with the actual filename.

---

_Comment by @woodruffw on 2026-01-16 18:22_

> if you think this puts pip out of standards compliance I will take a look. Although this is hairy enough I'm not likely going to change the behavior beyond logging a warning when content disposition does not match with the actual filename.

I tried digging into this and found it pretty complicated/confusing 😅. Here's my attempt to tie it together:

From the living copy of PEP 440:

> Direct references
> Some automated tools may permit the use of a direct reference as an alternative to a normal version specifier. A direct reference consists of the specifier @ and an explicit URL.
>
> Whether or not direct references are appropriate depends on the specific use case for the version specifier. Automated tools SHOULD at least issue warnings and MAY reject them entirely when direct references are used inappropriately.
>
> Public index servers SHOULD NOT allow the use of direct references in uploaded distributions. Direct references are intended as a tool for software integrators rather than publishers.
>
> Depending on the use case, some appropriate targets for a direct URL reference may be an sdist or a wheel binary archive. The exact URLs and targets supported will be tool dependent.

Ref: https://packaging.python.org/en/latest/specifications/version-specifiers/#direct-references

From the living copy of PEP 508: 

> A minimal URL based lookup:
>
> `pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686`

Ref: https://packaging.python.org/en/latest/specifications/dependency-specifiers/#specification

And from PEP 625:

> The file name of a sdist was standardised in [PEP 625](https://peps.python.org/pep-0625/). The file name must be in the form {name}-{version}.tar.gz, where {name} is normalised according to the same rules as for binary distributions (see [Binary distribution format](https://packaging.python.org/en/latest/specifications/binary-distribution-format/#binary-distribution-format)), and {version} is the canonicalized form of the project version (see [Version specifiers](https://packaging.python.org/en/latest/specifications/version-specifiers/#version-specifiers)).

Ref: https://packaging.python.org/en/latest/specifications/source-distribution-format/#source-distribution-file-name

So, to summarize:

* Direct references are `specifier @ url`. Typical targets (i.e. the content behind the URL) are sdists and wheels, but PEP 440 explicitly carves out URLs and targets as tool-specific.
* PEP 508 gives an example of a URL-based lookup that *isn't* a source distribution, since it doesn't use the PEP 625 sdist filename format. This would fall under what both uv and pip would call "source archive" support, which is tool-specific behavior. In practice, that generally means (to my understanding) that both pip and uv try to use the target as a source tree or similar. 
* PEP 625 says that sdists are always of the form `{name}-{version}.tar.gz`, so I *think* a target URL that has `https://example/.../{name}-{version}.tar.gz` would be correctly treated as a source distribution (and validated as such) rather than a "source archive". Similarly, I think it's correct to treat `filename` in `Content-Disposition` as providing the filename for this validation purpose.

TL;DR: I'm pretty sure pip is compliant here, at least insofar as the specs are (1) not super prescriptive, and (2) leave a lot of this open to tool-specific behavior anyways. uv is *also* compliant w/r/t the specs, but *could* be more compatible with pip by doing the same `Content-Disposition` handling.

(I feel medium-low confidence on the analysis above; I'd appreciate someone pointing out places I've gotten it wrong!)

---

_Label `compatibility` added by @zanieb on 2026-01-16 18:24_

---

_Comment by @konstin on 2026-01-19 11:53_

uv depends on the filename in the URL in some locations such as `uv.lock` parsing, reading `Content-Disposition` requires bigger changes than allowing more URLs, since we need to ensure that the archive type doesn't change. I'm assuming that URLs that don't have a source distribution filename in their URL will return source archives rather than source distributions, or at least I haven't seen a production service that returns valid source distributions on opauqe URLs.

---
