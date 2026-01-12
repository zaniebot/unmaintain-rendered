```yaml
number: 2981
title: Update README.md with working instructions for RHEL/CentOS/Rocky Linux 9
type: pull_request
state: closed
author: wackget
labels:
  - rollup
assignees: []
base: master
head: patch-1
created_at: 2025-01-30T17:02:25Z
updated_at: 2025-09-20T01:08:40Z
url: https://github.com/BurntSushi/ripgrep/pull/2981
synced_at: 2026-01-12T18:23:14Z
```

# Update README.md with working instructions for RHEL/CentOS/Rocky Linux 9

---

_@wackget_

Resolves #2924 

Updated/working instructions for installing ripgrep via EPEL repos on RHEL/CentOS/Rocky 9

---

_@BurntSushi approved on 2025-08-17 18:21_

Thanks!

(It's kinda crazy what you have to do to install ripgrep from official package repos on these Linux distros. No wonder why some people just want to use random binaries off of GitHub.)

---

_Label `rollup` added by @BurntSushi on 2025-08-17 18:23_

---

_Comment by @BurntSushi on 2025-08-17 21:38_

Apparently superseded by #3124, which is much nicer.

---

_Closed by @BurntSushi on 2025-08-17 21:38_

---

_Label `rollup` removed by @BurntSushi on 2025-08-17 21:38_

---

_Comment by @wackget on 2025-08-18 00:47_

> Apparently superseded by #3124, which is much nicer.

They are the same solution. The only difference is my suggested changes include the necessary commands to install and enable to the EPEL repo. The instructions in #3124 skip that, which means any users who haven't already installed EPEL will get an error message.

I'd request you use my changes for clarity.

---

_Comment by @BurntSushi on 2025-08-18 00:51_

The PR message in #3124 seems to say that those steps aren't necessary.

I guess I'll have to test this myself. Sigh.

---

_Comment by @BurntSushi on 2025-08-19 20:06_

OK, well, for CentOS:

```
$ docker run -it centos:centos7.9.2009 bash
[root@4e490518fea9 /]# sudo dnf install -y ripgrep
bash: sudo: command not found
[root@4e490518fea9 /]# dnf install -y ripgrep
bash: dnf: command not found
[root@645ee29121d3 /]# yum install ripgrep
Loaded plugins: fastestmirror, ovl
Determining fastest mirrors
Could not retrieve mirrorlist http://mirrorlist.centos.org/?release=7&arch=x86_64&repo=os&infra=container error was
14: curl#6 - "Could not resolve host: mirrorlist.centos.org; Unknown error"


 One of the configured repositories failed (Unknown),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=<repoid> ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>
        or
            subscription-manager repos --disable=<repoid>

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true

Cannot find a valid baseurl for repo: base/7/x86_64
```

So that means neither PR works for CentOS. And the status quo doesn't seem to work either, although `yum` is at least available.

And for Rocky:

```
$ docker run -it rockylinux:9.3.20231119-minimal bash
bash-5.1# sudo dnf install -y ripgrep
bash: sudo: command not found
bash-5.1# dnf install -y ripgrep
bash: dnf: command not found
bash-5.1# yum
bash: yum: command not found
```

OK, so the status quo nor these PRs work here either.

I cannot tell whether these are problems with the docker images or with the Linux distro. I've worked with CentOS in production before (many many years ago), and all I remember is that it was a constant headache.

I'm just going to remove these Linux distros from the README. If someone can provide an authoritative source that I can trust on the recommended installation steps, I'll be happy to revisit it.

---

_Comment by @wackget on 2025-08-20 00:39_

The CentOS example won't work because it's really outdated (use CentOS 8 or newer, or ideally CentOS Stream).

The Rocky example isn't working because you're using the `-minimal` image which is really stripped down.

Try `docker run -it rockylinux:9.3 bash`.

Then you'll see the following results (**note: no sudo because you're already root**):

```
$ dnf install ripgrep
No match for argument: ripgrep
Error: Unable to find a match: ripgrep
```

Then paste the lines from my original PR (without sudo):

```
$ dnf install -y \
    https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm \
    https://dl.fedoraproject.org/pub/epel/epel-next-release-latest-9.noarch.rpm
dnf install -y ripgrep
# ...
Installed:
  ripgrep-14.1.1-1.el9.x86_64                                                                                                                                                                                                                                                        

Complete!
```

> (It's kinda crazy what you have to do to install ripgrep from official package repos on these Linux distros. No wonder why some people just want to use random binaries off of GitHub.)

Just to clarify: all you need to do is install the Extra Packages for Enterprise Linux (EPEL) repo. Then you can install ripgrep. It's literally just one extra step.

Please consider accepting my original PR.



---

_Comment by @BurntSushi on 2025-08-20 01:51_

You also need to do `dnf config-manager --set-enabled crb`. So that's two extra steps. And that two steps more than pretty much everything else in ripgrep's README. So yes, I consider that to be a degraded user experience.

I'll just take this PR and hopefully this is what people expect. Thanks.

---

_Reopened by @BurntSushi on 2025-08-20 01:51_

---

_Label `rollup` added by @BurntSushi on 2025-08-20 01:56_

---

_Comment by @wackget on 2025-08-20 02:30_

> You also need to do `dnf config-manager --set-enabled crb`. So that's two extra steps. And that two steps more than pretty much everything else in ripgrep's README. So yes, I consider that to be a degraded user experience.
> 
> I'll just take this PR and hopefully this is what people expect. Thanks.

The `crb` repo probably isn't necessary (it wasn't needed on the `rockylinux:9.3` image I tested) so that could be my unnecessary addition, or it could have been necessary for whatever version I was running when I wrote the PR.

Either way, thank you.

To get ripgrep included in the base RHEL repo I think it would need to be pulled by Red Hat from (upstream) Fedora. I'm guessing that's very unlikely, though, since it's already in the EPEL repo. Red Hat want to keep Enterprise Linux as trim as reasonably possible because it's explicitly meant to be a very stable server OS.

EPEL is very commonly used in RHEL so it's *almost* as good as having it in the base repo. Even common tools like emacs, htop, iftop, unrar, etc. are only found in EPEL. 

---

_Comment by @BurntSushi on 2025-08-20 02:32_

Thanks for the added context.

---

_Comment by @carlwgeorge on 2025-08-20 04:06_

> They are the same solution. The only difference is my suggested changes include the necessary commands to install and enable to the EPEL repo. The instructions in https://github.com/BurntSushi/ripgrep/pull/3124 skip that, which means any users who haven't already installed EPEL will get an error message.

They don't skip it, they link directly to the [official setup instructions for EPEL](https://docs.fedoraproject.org/en-US/epel/getting-started/).  Duplicating the instructions in this repo's README sets things up to get out of sync.

> The PR message in https://github.com/BurntSushi/ripgrep/pull/3124 seems to say that those steps aren't necessary.

I just said that the copr repo is no longer needed.  It made sense to include the instructions for a random copr that most people wouldn't be familiar with.  It doesn't make sense to duplicate the setup instructions for the most common third party repo for RHEL/CentOS systems.

> I'm just going to remove these Linux distros from the README. If someone can provide an authoritative source that I can trust on the recommended installation steps, I'll be happy to revisit it.

Hi, I'm the authoritative source.  I'm an elected member of the [EPEL Steering Committee](https://docs.fedoraproject.org/en-US/epel/epel-policy-steering-committee/).  I also lead the Red Hat EPEL team (part of the [CLE team](https://docs.fedoraproject.org/en-US/cle/)).

> You also need to do dnf config-manager --set-enabled crb.

This is why I linked to the official EPEL instructions.  The setup setup steps vary between distros and versions.

> The crb repo probably isn't necessary

Many packages in EPEL have dependencies in CRB.  When it's not set up at the start, EPEL maintainers later get bogus bug reports for missing dependencies.

Here is verification that the combination of the official EPEL setup and #3124 work on CentOS 10.

<details>

```
carl red ~ 
❯ podman run -it --rm centos:10
[root@4d2660035a1b /]# dnf config-manager --set-enabled crb
[root@4d2660035a1b /]# dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-10.noarch.rpm
CentOS Stream 10 - BaseOS                                                                                                      6.2 MB/s | 6.7 MB     00:01    
CentOS Stream 10 - AppStream                                                                                                   4.3 MB/s | 3.4 MB     00:00    
CentOS Stream 10 - CRB                                                                                                         1.1 MB/s | 749 kB     00:00    
CentOS Stream 10 - Extras packages                                                                                              17 kB/s | 6.9 kB     00:00    
epel-release-latest-10.noarch.rpm                                                                                               88 kB/s |  18 kB     00:00    
Dependencies resolved.
===============================================================================================================================================================
 Package                                   Architecture                    Version                                 Repository                             Size
===============================================================================================================================================================
Installing:
 epel-release                              noarch                          10-6.el10_0                             @commandline                           18 k
Installing weak dependencies:
 dnf-plugins-core                          noarch                          4.7.0-9.el10                            baseos                                 44 k

Transaction Summary
===============================================================================================================================================================
Install  2 Packages

Total size: 62 k
Total download size: 44 k
Installed size: 47 k
Is this ok [y/N]: y
Downloading Packages:
dnf-plugins-core-4.7.0-9.el10.noarch.rpm                                                                                       124 kB/s |  44 kB     00:00    
---------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                           76 kB/s |  44 kB     00:00     
CentOS Stream 10 - BaseOS                                                                                                      1.6 MB/s | 1.6 kB     00:00    
Importing GPG key 0x8483C65D:
 Userid     : "CentOS (CentOS Official Signing Key) <security@centos.org>"
 Fingerprint: 99DB 70FA E1D7 CE22 7FB6 4882 05B5 55B3 8483 C65D
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial-SHA256
Is this ok [y/N]: y
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                       1/1 
  Installing       : dnf-plugins-core-4.7.0-9.el10.noarch                                                                                                  1/2 
  Installing       : epel-release-10-6.el10_0.noarch                                                                                                       2/2 
  Running scriptlet: epel-release-10-6.el10_0.noarch                                                                                                       2/2 
Many EPEL packages require the CodeReady Builder (CRB) repository.
It is recommended that you run /usr/bin/crb enable to enable the CRB repository.


Installed:
  dnf-plugins-core-4.7.0-9.el10.noarch                                             epel-release-10-6.el10_0.noarch                                            

Complete!
[root@4d2660035a1b /]# dnf install ripgrep
Extra Packages for Enterprise Linux 10 - x86_64                                                                                3.0 MB/s | 5.3 MB     00:01    
Last metadata expiration check: 0:00:01 ago on Wed Aug 20 00:00:21 2025.
Dependencies resolved.
===============================================================================================================================================================
 Package                              Architecture                        Version                                      Repository                         Size
===============================================================================================================================================================
Installing:
 ripgrep                              x86_64                              14.1.1-1.el10_0                              epel                              1.5 M

Transaction Summary
===============================================================================================================================================================
Install  1 Package

Total download size: 1.5 M
Installed size: 4.6 M
Is this ok [y/N]: y
Downloading Packages:
ripgrep-14.1.1-1.el10_0.x86_64.rpm                                                                                             2.1 MB/s | 1.5 MB     00:00    
---------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                          1.4 MB/s | 1.5 MB     00:01     
Extra Packages for Enterprise Linux 10 - x86_64                                                                                1.6 MB/s | 1.6 kB     00:00    
Importing GPG key 0xE37ED158:
 Userid     : "Fedora (epel10) <epel@fedoraproject.org>"
 Fingerprint: 7D8D 15CB FC4E 6268 8591 FB26 33D9 8517 E37E D158
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-10
Is this ok [y/N]: y
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                       1/1 
  Installing       : ripgrep-14.1.1-1.el10_0.x86_64                                                                                                        1/1 
  Running scriptlet: ripgrep-14.1.1-1.el10_0.x86_64                                                                                                        1/1 

Installed:
  ripgrep-14.1.1-1.el10_0.x86_64                                                                                                                               

Complete!
```

</details>

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
