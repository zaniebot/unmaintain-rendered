```yaml
number: 3159
title: "doc: update the CentOS, RHEL and Rocky Linux installation instructions"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/centos-rocky-rhel-install-instructions
created_at: 2025-09-24T12:59:18Z
updated_at: 2025-09-26T16:18:14Z
url: https://github.com/BurntSushi/ripgrep/pull/3159
synced_at: 2026-01-12T18:23:15Z
```

# doc: update the CentOS, RHEL and Rocky Linux installation instructions

---

_@BurntSushi_

I've split the previously singular "CentOS/RHEL/Rocky" section into 3
sections. They each benefit from having their own steps.

I've also copied steps from [EPEL Getting Started] documentation,
including steps that don't seem to be required because it seems to be
best practice (although I do not understand it). Notably, this is not
required for CentOS Stream:

```
dnf config-manager --set-enabled crb
```

And this is not required for Red Hat:

```
subscription-manager repos --enable codeready-builder-for-rhel-10-$(arch)-rpms
```

And neither are available on Rocky Linux 10. Hence, all 3 have slightly
different instructions.

It has been suggested (see [here][suggest1] and [here][suggest2]) that
the installation instructions should just link to the [EPEL Getting
Started] documentation and just contain this step:

```
sudo dnf install ripgrep
```

However, this is not sufficient to actually install ripgrep from a
base installation of these Linux distributions. I tested this via the
`dokken/centos-stream-10:sha-d1e294f`, `rockylinux/rockylinux:10` and
`redhat/ubi10` Docker images on DockerHub.

While this does mean ripgrep's installation instructions can become out
of sync from upstream, this is *always* a risk regardless of platform.
The instructions are provided on a best effort basis and generally
should work on the latest release of said platform. If the instructions
result in unhelpful errors (like `dnf install ripgrep` does if you
don't enable EPEL), then that isn't being maximally helpful to users.
I'd rather attempt to give the entire set of instructions and risk
being out of sync.

Also, since the installation instructions include URLs with version
numbers in them, I made the section names include version numbers as
well.

Note: I found using the `dokken/centos-stream-10:sha-d1e294f` Docker
image to be somewhat odd, as I could not find any official CentOS
Docker images. [This][DockerHub-CentOS] is still the first hit on
Google, but all of its tags have been deleted and the image is
deprecated. I was profoundly confused by this given that the [EPEL
Getting Started] documentation *specifically* cites CentOS 10. In fact,
it is citing CentOS *Stream* 10, which is something wholly distinct
from CentOS. What an absolute **clusterfuck**. If I had just read this
paragraph on Wikipedia from the beginning, I would have saved myself a
lot of confusion:

> In December 2020, Red Hat unilaterally terminated CentOS development in favor
> of CentOS Stream 9, a distribution positioned upstream of RHEL. In March
> 2021, CloudLinux (makers of CloudLinux OS) released a RHEL derivative called
> AlmaLinux. Later in May 2021, one of the CentOS founders (Gregory Kurtzer)
> created the competing Rocky Linux project as a successor to the original
> mission of CentOS.

Ref #2981, Ref #2924

[EPEL Getting Started]: https://docs.fedoraproject.org/en-US/epel/getting-started/
[suggest1]: https://github.com/BurntSushi/ripgrep/pull/2981#issuecomment-3204114293
[suggest2]: https://github.com/BurntSushi/ripgrep/issues/2924#issuecomment-3326357254
[DockerHub-CentOS]: https://hub.docker.com/_/centos


---

_Merged by @BurntSushi on 2025-09-24 14:02_

---

_Closed by @BurntSushi on 2025-09-24 14:02_

---

_Branch deleted on 2025-09-24 14:02_

---

_Comment by @carlwgeorge on 2025-09-25 08:07_

> I've also copied steps from [EPEL Getting Started](https://docs.fedoraproject.org/en-US/epel/getting-started/) documentation,

I really wish you wouldn't, as you admittedly don't understand it but are signing up to maintain a parallel set of instructions.

> including steps that don't seem to be required because it seems to be best practice (although I do not understand it).

Enabling CRB is not required to install ripgrep from EPEL, as it doesn't have dependencies in CRB.  It is required for many other EPEL packages that do have dependencies in CRB.  When users forget to enable it, they often file erroneous bug reports that waste EPEL maintainers' time.  This PR will lead to that same outcome for Rocky users since you left out the CRB step there.

> And neither are available on Rocky Linux 10.

That doesn't make sense, as Rocky 10 also [names this repo `crb`](https://git.rockylinux.org/staging/src/rocky-release/-/blob/r10/SOURCES/rocky.repo?ref_type=heads#L67).

> However, this is not sufficient to actually install ripgrep from a base installation of these Linux distributions.

It is when you have EPEL enabled, which most systems do, and systems that don't can click the EPEL link to follow the official setup instructions.

> I tested this via the dokken/centos-stream-10:sha-d1e294f, rockylinux/rockylinux:10 and redhat/ubi10 Docker images on DockerHub.

I don't know what dokken/centos-stream-10 is.  It certainly isn't the [official container image from the CentOS Project](https://quay.io/centos/centos:stream10).

[UBI](https://catalog.redhat.com/en/software/base-images) is not the same as RHEL.  It is a subset of RHEL content that doesn't require a subscription (so subscription-manager is the wrong tool there) and is redistributable.  And in the context of this discussion, it uses different repo names (e.g. codeready-builder-for-**ubi**-10-x86_64-rpms instead of codeready-builder-for-**rhel**-10-x86_64-rpms).

> While this does mean ripgrep's installation instructions can become out of sync from upstream, this is always a risk regardless of platform.

It's a greater risk for EPEL due to the long lifecycle of RHEL.  #3124 was valid for all current versions of EPEL (8, 9, 10) and would likely continue being valid in the future.  This PR only covers EPEL 10, and lacking instructions for different versions will likely lead to people trying to follow the same instructions on a different base (e.g. enabling EPEL 10 on RHEL 9) and breaking their systems.  This wasn't a big deal when ripgrep was being installed from a copr repo that only contained ripgrep, which is why #1428 appeared to work.  But EPEL has far more packages, and enabling a non-corresponding major version will inevitably break systems.

> If the instructions result in unhelpful errors (like dnf install ripgrep does if you don't enable EPEL), then that isn't being maximally helpful to users.

Users of CentOS, RHEL, and related distros know what EPEL is.  I promise you, mentioning that ripgrep is available is EPEL will almost always be sufficient, and if by chance someone doesn't know about it they can follow the link to get the official instructions.

> Note: I found using the dokken/centos-stream-10:sha-d1e294f Docker image to be somewhat odd, as I could not find any official CentOS Docker images.

* https://centos.org
* download button -> https://centos.org/download/
* containers column, images button -> https://quay.io/centos/centos:stream10

> I was profoundly confused by this given that the EPEL Getting Started documentation specifically cites CentOS 10.

Most EPEL 10 packages are built against CentOS 10, a.k.a. CentOS Stream 10.

> In fact, it is citing CentOS Stream 10, which is something wholly distinct from CentOS.

Not really.  What you're referring to as "CentOS" is actually CentOS Linux.  This was the legacy variant of the distro from the CentOS Project.  In 2019 a new variant named CentOS Stream was created, still within the same CentOS Project.  It's the essentially the same distro but with content delivered on a different schedule, which fixed lots of problems with sustainability and contributions.  For a time I was a maintainer of both variants, and I would literally tag the same builds into both variants at different times.  People regularly use the shorthand CentOS to mean "the distro from the CentOS Project", which used to be CentOS Linux, and now is CentOS Stream.

> If I had just read this paragraph on Wikipedia from the beginning, I would have saved myself a
lot of confusion:

The Wikipedia page is full of errors, but so far attempts at correcting information or even rephrasing things to be more neutral have been repeatedly reverted by a handful of hostile editors.  Don't take my word for it, check out the [edit history](https://en.wikipedia.org/w/index.php?title=CentOS&action=history) and [talk page](https://en.wikipedia.org/wiki/Talk:CentOS).  I strongly recommend disregarding that page in it's entirety.

---

I don't expect you to continue editing these instructions, as clearly you want to do things your way despite my repeated attempts to guide you to a better solution.  I just wanted to reply here with clarifying information for the things you misunderstood, and note my concerns for posterity and to refer back to in the future.

---

_Comment by @BurntSushi on 2025-09-25 12:42_

> > And neither are available on Rocky Linux 10.
> 
> That doesn't make sense, as Rocky 10 also [names this repo `crb`](https://git.rockylinux.org/staging/src/rocky-release/-/blob/r10/SOURCES/rocky.repo?ref_type=heads#L67).

I was unable to make either work:

```
$ docker run -it rockylinux/rockylinux:10 bash
bash-5.2# dnf config-manager --set-enabled crb
No such command: config-manager. Please use /usr/bin/dnf --help
It could be a DNF plugin command, try: "dnf install 'dnf-command(config-manager)'"
bash-5.2# subscription-manager repos --enable codeready-builder-for-rhel-10-$(arch)-rpms
bash: subscription-manager: command not found
bash-5.2#
```

> I don't know what dokken/centos-stream-10 is. It certainly isn't the [official container image from the CentOS Project](https://quay.io/centos/centos:stream10).

That link is broken. It redirects me to <https://quay.io/500>:

<img width="1083" height="744" alt="quay-500" src="https://github.com/user-attachments/assets/4de219ee-b0d9-43de-8ee4-1e08076781d0" />

> [UBI](https://catalog.redhat.com/en/software/base-images) is not the same as RHEL. It is a subset of RHEL content that doesn't require a subscription (so subscription-manager is the wrong tool there) and is redistributable. And in the context of this discussion, it uses different repo names (e.g. codeready-builder-for-**ubi**-10-x86_64-rpms instead of codeready-builder-for-**rhel**-10-x86_64-rpms).

I didn't say that UBI was the same as RHEL. I said that I used it to test the RHEL commands to install ripgrep. I used it because I couldn't find anything that was obviously better (to me).

I find the entire RHEL/CentOS/Rocky dynamic to be extremely confusing. And especially so given how hard it was for me to just try out a few Docker images. I never have this problem with other Linux distros. It seems like there is plenty of room for RHEL to improve their communication here.

-----

Personally, I find your communication style to be condescending (particularly your last paragraph) and overall pretty frustrating here. I understand that I'm doing something that you don't agree with, but I've given what I believe are compelling reasons for doing so. From my perspective, you've dismissed my reasons without really engaging with them.

I'd like to ask you to please step back and reconsider how you're approaching engagement here. If you don't want to do that, that's fine, but I'll ask you to stop engaging at all in that case.

---

_Comment by @carlwgeorge on 2025-09-25 21:32_

> I was unable to make either work:

I'm not involved in Rocky, and it's not my fault that `dnf config-manager` doesn't work out of the box in their image while it does in CentOS, UBI, and RHEL.  Go complain to them about their "bug-for-bug compatibility".

> That link is broken. It redirects me to https://quay.io/500

```
carl red ~ 
❯ curl -IL https://quay.io/centos/centos:stream10
HTTP/2 308 
date: Thu, 25 Sep 2025 21:14:15 GMT
content-type: text/html; charset=utf-8
content-length: 265
location: https://quay.io/centos/centos:stream10/
server: nginx/1.22.1
x-frame-options: DENY
strict-transport-security: max-age=63072000; preload

HTTP/2 302 
date: Thu, 25 Sep 2025 21:14:15 GMT
content-type: text/html; charset=utf-8
content-length: 289
server: nginx/1.22.1
location: /repository/centos/centos?tab=tags&tag=stream10
cache-control: no-cache, no-store, must-revalidate
vary: Cookie
x-frame-options: DENY
strict-transport-security: max-age=63072000; preload

HTTP/2 200 
date: Thu, 25 Sep 2025 21:14:15 GMT
content-type: text/html; charset=utf-8
content-length: 78147
server: nginx/1.22.1
x-frame-options: DENY
cache-control: no-cache, no-store, must-revalidate
vary: Cookie
set-cookie: _csrf_token=eyJfY3NyZl90b2tlbiI6IkJzMDF2dS0zWGRTQmVTZm1VMnFlNFp5N0k5SWx5LUZIYlRmTndtYVNxSjh1aWRTZjdTNnJPaFIzcGotc0xMTXoifQ.aNWwpw.CcYry0WliR1zNs9-FFPgixpZgkg; Secure; HttpOnly; Path=/; SameSite=Lax
x-frame-options: DENY
strict-transport-security: max-age=63072000; preload
```

> Personally, I find your communication style to be condescending (particularly your last paragraph) and overall pretty frustrating here.

> What an absolute clusterfuck.

Pot, meet kettle.

> If you don't want to do that, that's fine, but I'll ask you to stop engaging at all in that case.

Roger that.  I regret ever making that copr repo, contributing the original installation instructions here, or trying to refresh them.  I'll refrain from attempting to work with you in the future.

---

_Comment by @BurntSushi on 2025-09-25 21:49_

Great. Then enjoy the block.

---

_Comment by @jonathanspw on 2025-09-26 13:24_

Why is there no mention of AlmaLinux at all?

---

_Comment by @BurntSushi on 2025-09-26 16:18_

I've never heard of it.

---
