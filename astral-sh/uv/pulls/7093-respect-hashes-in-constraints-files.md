```yaml
number: 7093
title: Respect hashes in constraints files
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - security
assignees: []
merged: true
base: main
head: charlie/constraint-hash
created_at: 2024-09-05T17:57:24Z
updated_at: 2024-09-05T19:49:55Z
url: https://github.com/astral-sh/uv/pull/7093
synced_at: 2026-01-12T16:07:41Z
```

# Respect hashes in constraints files

---

_@charliermarsh_

## Summary

Like pip, if hashes are present on both the requirement and the constraint, we prefer the requirement.

Closes #7089.


---

_Label `enhancement` added by @charliermarsh on 2024-09-05 17:57_

---

_Label `security` added by @charliermarsh on 2024-09-05 17:57_

---

_Comment by @charliermarsh on 2024-09-05 17:59_

@alex -- Do you have an opinion on what's correct, if both a requirement and a constraint for a given package-version include `--hash`?

---

_Comment by @alex on 2024-09-05 18:03_

I guess there's three cases:

1) They have the same hashes, that's fine, that's ok.
2) One of them has a subset of the others' hashes. If constraint has a subset of requirements' hashes, I think that should act as a "filter" -- effectively it's constraining the set of valid hashes. If requirements' hashes are a subset of constraints hashes, error?
3) If they just disagree: Error.

But that seems kind of complicated: So really: same == fine, different == error is probably sufficient.

---

_Merged by @charliermarsh on 2024-09-05 18:30_

---

_Closed by @charliermarsh on 2024-09-05 18:30_

---

_Branch deleted on 2024-09-05 18:30_

---

_Comment by @charliermarsh on 2024-09-05 18:40_

For now, I went with what I observe from pip's behavior, but I may change prior to merging. I see some pip issues that suggest they want to be using the intersection, but I don't see that behavior locally.

---

_Comment by @alex on 2024-09-05 18:42_

Sounds good. Thanks

On Thu, Sep 5, 2024 at 2:41 PM Charlie Marsh ***@***.***>
wrote:

> For now, I went with what I observe from pip's behavior, but I may change
> prior to merging. I see some pip issues that suggest they want to be using
> the intersection, but I don't see that behavior locally.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/pull/7093#issuecomment-2332405605>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBEQ37FR3BDFFJAMROLZVCQUFAVCNFSM6AAAAABNXAJTGGVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGMZSGQYDKNRQGU>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


-- 
All that is necessary for evil to succeed is for good people to do nothing.


---

_Comment by @notatallshaw on 2024-09-05 19:25_

> For now, I went with what I observe from pip's behavior, but I may change prior to merging. I see some pip issues that suggest they want to be using the intersection, but I don't see that behavior locally.

The latest discussion is about dropping support for hashes in constraints: https://github.com/pypa/pip/issues/12942#issuecomment-2332158690

In practise hashes in constraints files don't really work in pip as they imply require hashes, but the requires hashes errors  out unless the hash is in the requirements file. 

So I think uv has quite a bit of leeway here to choose what it thinks is best.

---

_Comment by @charliermarsh on 2024-09-05 19:29_

What's the motivation for dropping support there?

---

_Comment by @notatallshaw on 2024-09-05 19:39_

> What's the motivation for dropping support there?

As I understand it (and I've not spent time reviewing the code, I'm just basing this on what I've read in comments) constraints are implemented as a collection filter where hashes are not checked, and are not part of resolution verification where hashes can be checked.

Currently pip only allows hashes to be specified if you pin the requirement, and adding a hash implies requiring hashes, this leads to the behaviour of:


1. Create `constraints.txt` with the contents:
```
setuptools==74.1.1 --hash=sha256:fc91b5f89e392ef5b77fe143b17e32f65d3024744fba66dc3afe07201684d766
```
2. Run `pip install setuptools==74.1.1 -c constraints.txt`, and get error:

```
ERROR: Hashes are required in --require-hashes mode, but they are missing from some requirements. Here is a list of those requirements along with the hashes their downloaded archives actually had. Add lines like these to your requirements files to prevent tampering. (If you did not enable --require-hashes manually, note that it turns on automatically when any package has a hash.)
	setuptools==74.1.1 --hash=sha256:fc91b5f89e392ef5b77fe143b17e32f65d3024744fba66dc3afe07201684d766
```

I would love for pip to support this use case (and hence also uv), but I'm not going to be the one to drive large architectural changes on the pip side. 


---

_Comment by @charliermarsh on 2024-09-05 19:42_

Ahh ok I see. Yeah we can support it, it's structured slightly differently here. Not sure what's best though: union, intersection, ignroing one or the other, etc.

---

_Comment by @notatallshaw on 2024-09-05 19:48_

IMO a constraint should "constrain" the solution. Which I think would be an intersection?

A requirements of:

```
foo==1.0.0 --hash=sha256:{hashA} --hash=sha256:{hashB}
bar==1.0.0 
baz
```

With a constraints file of:

```
foo==1.0.0 --hash=sha256:{hashB} --hash=sha256:{hashC}
bar==1.0.0 --hash=sha256:{hashD}
baz==1.0.0 --hash=sha256:{hashE}
```

For those packages the solution (assuming one exists) should only be able to have:

```
foo==1.0.0 --hash=sha256:{hashB}
bar==1.0.0 --hash=sha256:{hashD}
baz==1.0.0 --hash=sha256:{hashE}
```

Does that make sense?

---
