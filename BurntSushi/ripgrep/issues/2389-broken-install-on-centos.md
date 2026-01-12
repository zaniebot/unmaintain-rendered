```yaml
number: 2389
title: Broken install on Centos
type: issue
state: closed
author: JudsenHembree
labels:
  - invalid
assignees: []
created_at: 2023-01-04T05:43:35Z
updated_at: 2023-01-04T11:33:41Z
url: https://github.com/BurntSushi/ripgrep/issues/2389
synced_at: 2026-01-12T16:13:24Z
```

# Broken install on Centos

---

_@JudsenHembree_

#### What version of ripgrep are you using?

No version can't install

#### How did you install ripgrep?

``` 
sudo yum-config-manager --add-repo=https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo
```
```sudo yum install ripgrep```

#### What operating system are you using ripgrep on?

PRETTY_NAME="CentOS Linux 7 (Core)"

#### Describe your bug.

Yum throws some error reporting a lapsed certification. 

```
Could not fetch/save url https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo to file /etc/yum.repos.d/carlwgeorge-ripgrep-epel-7.repo: [Errno 14] curl#60 - "Peer's Certificate has expired."
```

#### What are the steps to reproduce the behavior?
Just run the commands listed in the docs for centos.

#### What is the actual behavior?

Fails to install.

```
sudo yum-config-manager --add-repo=https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo
Loaded plugins: fastestmirror, langpacks
adding repo from: https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo
grabbing file https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo to /etc/yum.repos.d/carlwgeorge-ripgrep-epel-7.repo
Could not fetch/save url https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo to file /etc/yum.repos.d/carlwgeorge-ripgrep-epel-7.repo: [Errno 14] curl#60 - "Peer's Certificate has expired."

```

#### What is the expected behavior?

What do you think ripgrep should have done?

Install


---

_Comment by @BurntSushi on 2023-01-04 11:33_

You've reported this problem in the wrong place. You need to report problems with packages to the people that maintain those packages. This is the project itself. I don't maintain its distro packages.

---

_Closed by @BurntSushi on 2023-01-04 11:33_

---

_Label `invalid` added by @BurntSushi on 2023-01-04 11:33_

---
