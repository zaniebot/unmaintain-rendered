```yaml
number: 2216
title: Add MPR installation instructions
type: pull_request
state: closed
author: hwittenborn
labels: []
assignees: []
base: master
head: master
created_at: 2022-05-19T05:01:12Z
updated_at: 2022-05-19T20:38:54Z
url: https://github.com/BurntSushi/ripgrep/pull/2216
synced_at: 2026-01-12T18:23:14Z
```

# Add MPR installation instructions

---

_@hwittenborn_

This adds installation instructions for the [makedeb Package Repository](https://mpr.makedeb.org), an AUR-like platform for Debian and Ubuntu based systems.

The MPR is a personal project of mine, but I thought it'd be helpful to include it here so people know its an option.

---

_Comment by @BurntSushi on 2022-05-19 12:06_

Thanks for submitting this.

I'll note that your project page doesn't appear to load for me (note that I am behind a VPN):

```
$ curl 'https://mpr.makedeb.org'
curl: (35) OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to mpr.makedeb.org:443
```

Separately from that, I personally haven't heard of the `makedeb` project before, which makes me pause before including it in the installation instructions. I'd prefer it reach a bit more of a critical mass before recommending it here. Apologies for being vague, but it is difficult not to be in such things.

---

_Closed by @BurntSushi on 2022-05-19 12:06_

---

_Comment by @hwittenborn on 2022-05-19 20:38_

> I'll note that your project page doesn't appear to load for me (note that I am behind a VPN)

I'm assuming it's something with your VPN, it loads just fine on my end. I'd try without it if you're still curious.

>  I personally haven't heard of the makedeb project before, which makes me pause before including it in the installation instructions

That's fair enough. The MPR has about 300 packages at the moment, but makedeb itself has been growing in popularity. It's definitely more of a niche thing, as the project was only started about a year ago.

Thanks regardless though! I thought I'd just throw this out and see what happened.

---
