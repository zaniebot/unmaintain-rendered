```yaml
number: 2972
title: centos copr url unvalid
type: issue
state: closed
author: TouchDeeper
labels:
  - doc
assignees: []
created_at: 2025-01-14T06:07:08Z
updated_at: 2025-09-23T01:31:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2972
synced_at: 2026-01-12T16:13:25Z
```

# centos copr url unvalid

---

_@TouchDeeper_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

none

### How did you install ripgrep?

yum

### What operating system are you using ripgrep on?

centos7

### Describe your bug.

centos copr url unvalid

### What are the steps to reproduce the behavior?

```
$ sudo yum install -y yum-utils
$ sudo yum-config-manager --add-repo=https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo
$ sudo yum install ripgrep
```

### What is the actual behavior?

url page not found

### What is the expected behavior?

url valid

---

_Comment by @BurntSushi on 2025-01-14 11:46_

I don't use CentOS. Patches are welcome.

---

_Label `doc` added by @BurntSushi on 2025-01-14 11:47_

---

_Comment by @OnlineCop on 2025-02-24 18:03_

The 'carlwgeorge' maintainer on [copr.fedorainfracloud.org](https://copr.fedorainfracloud.org/coprs/carlwgeorge/) does not appear to be providing RipGrep anymore.

As CentOS 7 is officially deprecated, the instructions within README.md should probably be removed.

Alternate "generic" instructions might be useful in its place:

> 1. Locate your appropriate architecture on the [Releases](https://github.com/BurntSushi/ripgrep/releases/tag/14.1.1) page.
> 2. Download the archive locally with `curl` or `wget`.
> 3. Extract the `rg` executable from the archive with `tar`.
> Example:
>     ```bash
>     $ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep-14.1.1-i686-unknown-linux-gnu.tar.gz
>     $ tar -C /usr/bin --strip-components=1 -xf ripgrep-14.1.1-i686-unknown-linux-gnu.tar.gz '*/rg'
>     ```
> - This would extract the binary to `/usr/bin/rg`. You could also extract the licenses, documentation, and auto-completion scripts as needed.

---

_Comment by @BurntSushi on 2025-09-23 01:31_

This should be fixed on `master`:

https://github.com/BurntSushi/ripgrep/blob/1b07c6616a5b06bd6c00f2a7d3418b9096f601f9/README.md#L305-L314

---

_Closed by @BurntSushi on 2025-09-23 01:31_

---
