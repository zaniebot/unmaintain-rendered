```yaml
number: 9040
title: Risk of non-official Ruff packages in package managers.
type: issue
state: closed
author: FishAlchemist
labels:
  - question
assignees: []
created_at: 2023-12-07T03:35:32Z
updated_at: 2023-12-11T04:15:02Z
url: https://github.com/astral-sh/ruff/issues/9040
synced_at: 2026-01-10T11:09:51Z
```

# Risk of non-official Ruff packages in package managers.

---

_Issue opened by @FishAlchemist on 2023-12-07 03:35_

About installing Ruff from the package management tool.
https://github.com/astral-sh/ruff/blob/main/docs/installation.md
These packages may be malicious or contain vulnerabilities that could be exploited by attackers.

There is no way to ensure that Ruff uploaded to the package management tool is native or problem-free, so is it necessary to mark unofficial ways as community supported?

---

_Label `question` added by @MichaReiser on 2023-12-07 04:21_

---

_Comment by @MichaReiser on 2023-12-07 04:22_

Hmm, that's a fair point. An added benefit of marking them as community-supported is that users know where to open an issue. 

---

_Comment by @eli-schwartz on 2023-12-11 02:06_

> These packages may be malicious or contain vulnerabilities that could be exploited by attackers.

Can you explain more about this?

---

_Comment by @FishAlchemist on 2023-12-11 03:23_

> > These packages may be malicious or contain vulnerabilities that could be exploited by attackers.
> 
> Can you explain more about this?

Ruff is an open source project, so modifications are not difficult. 
Unless the user obtains the hash from the Ruff project for verification, the security of Ruff cannot be guaranteed.
If additional modifications are required, the verification will fail.
Therefore, I suggest marking unofficial download methods as community support to prevent users from misunderstanding that it is official and trusting it directly.

---

_Comment by @eli-schwartz on 2023-12-11 03:47_

> Ruff is an open source project, so modifications are not difficult.

I'm not sure I understand what open source has to do with it. A supremely humongous number of proprietary projects have been modified, both maliciously and non-maliciously. I'd go so far as to say that it's the primary market for malicious behavior...

> Unless the user obtains the hash from the Ruff project for verification, the security of Ruff cannot be guaranteed.
If additional modifications are required, the verification will fail.

Do you obtain the hash of ruff when downloading it with pip install? I suspect that virtually no one anywhere does that.

Conversely, the "community support" distribution channels rely on verifiable hashes, which hashes are actually tracked publicly in a git repository. The hashes are signed by widely known public OSS personas.

Not only that, ruff actually gets built from source. Some of these distribution channels use dedicated builders maintained by big companies with a reputation at stake. Some of these distribution channels automated the process of *you* building ruff from source, which is like the epitome of trust. Some of these distribution channels repackage the official ruff precompiled binaries but with added tracking and verification of the hashes.

Most of these distribution channels are the ones you already trust to provide you with the Linux kernel, the userland, a GCC, a clang, a python installation, a graphical user environment and hundreds of other software projects you use on a daily basis (in some cases, thousands rather than hundreds).

Security, verifiability, and accountability are big topics here and these community distribution channels take that quite seriously. They are busy building tools and mechanisms for anyone to verify honesty, e.g. by founding and operating https://reproducible-builds.org/


Saying that they "might be malicious" feels quite bizarre.

---

_Comment by @eli-schwartz on 2023-12-11 03:58_

Independent of the topic of malice, community distributors *do* usually prefer their users come to them directly with any issues they even suspect might be specific to that distribution channel, or when in doubt come to them directly *anyway*. It's not about malice, though, simply that different rust compiler versions, different libcs, backported security fixes etc can always cause differences in behavior, and those community distribution channels are staffed by people who have experience at debugging the differences and forwarding bug reports upstream as necessary.

And of course community distribution channels can't necessarily guarantee that ruff will be promptly updated the day a new version is published. Some do, some don't. If you need the latest version then getting it directly is your surest bet.

...

Overall I see no reason not to emphasize the difference in responsibility between the community and the official binaries... But making this about "possibility of malicious binaries" feels like a very wrong move. Frankly, if you're after security you'll be much safer using the community, as it goes through several more layers of independent verification and multiple eyes, *in addition to* upstream. And again, rebuilding from source on your *own* machine.

---

_Closed by @FishAlchemist on 2023-12-11 04:15_

---
