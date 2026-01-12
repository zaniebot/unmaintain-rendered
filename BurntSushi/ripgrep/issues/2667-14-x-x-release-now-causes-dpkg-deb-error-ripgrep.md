```yaml
number: 2667
title: "14.x.x release now causes \"dpkg-deb: error: 'ripgrep_14.0.3_amd64.deb' is not a Debian format archive\""
type: issue
state: closed
author: Integralist
labels:
  - invalid
assignees: []
created_at: 2023-11-30T10:09:51Z
updated_at: 2023-11-30T13:06:43Z
url: https://github.com/BurntSushi/ripgrep/issues/2667
synced_at: 2026-01-12T16:13:24Z
```

# 14.x.x release now causes "dpkg-deb: error: 'ripgrep_14.0.3_amd64.deb' is not a Debian format archive"

---

_@Integralist_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

I'm trying to install latest Ripgrep.

### How did you install ripgrep?

```yaml
name: Testing Ripgrep Install
on: push
concurrency: # cancel in progress runs on push
  group: "ripgrep-test"
  cancel-in-progress: true
defaults:
  run:
    shell: bash
jobs:
  install-ripgrep:
    name: Install Ripgrep
    runs-on: ubuntu-latest
    steps:
      - name: "Install Ripgrep"
        run: |
          RIPGREP_VERSION=$(curl -s "https://api.github.com/repos/BurntSushi/ripgrep/releases/latest" | grep -Po '"tag_name": "\K[0-9.]+')
          RIPGREP_VERSION=13.0.0 # NOTE: override to validate why this stopped working 2 days ago
          curl -LO "https://github.com/BurntSushi/ripgrep/releases/download/${RIPGREP_VERSION}/ripgrep_${RIPGREP_VERSION}_amd64.deb"
          sudo dpkg -i "ripgrep_${RIPGREP_VERSION}_amd64.deb"
          rg -h
```

Example of 14.0.0 failing:
https://github.com/Integralist/actions-testing/actions/runs/7044888292/job/19173509725

Example of 13.0.0 suceeding:
https://github.com/Integralist/actions-testing/actions/runs/7044894703/job/19173530627

### What operating system are you using ripgrep on?

GitHub Actions ubuntu-latest runner.

### Describe your bug.

Ripgrep 14.x.x releases don't appear to be valid for Debian anymore.

### What are the steps to reproduce the behavior?

See the above GitHub Actions install code.

### What is the actual behavior?

The command fails as per the following error...

```
dpkg-deb: error: 'ripgrep_14.0.0_amd64.deb' is not a Debian format archive
dpkg: error processing archive ripgrep_[14](https://github.com/Integralist/actions-testing/actions/runs/7044888292/job/19173509725#step:2:15).0.0_amd64.deb (--install):
 dpkg-deb --control subprocess returned error exit status 2
Errors were encountered while processing:
 ripgrep_14.0.0_amd64.deb
Error: Process completed with exit code 1.
```

### What is the expected behavior?

Ripgrep should be installable on Ubuntu via GitHub Actions.

---

_Comment by @BurntSushi on 2023-11-30 12:22_

_Please_, in the future, provide a more minimal reproduction. I don't have the time to debug GitHub Action stuff.

In this case, I believe the 14.0.0 release tweaked the filename of the release asset to include a `-1` in the name. So it's `ripgrep_14.0.0-1_amd64.deb` instead of `ripgrep_14.0.0_amd64.deb`. It looks like this was a result of [this change in `cargo-deb`](https://github.com/kornelski/cargo-deb/pull/105). Neither the PR nor the commit messages say _why_ the change was made. Ah, but the [README mentions this in a note at the top](https://github.com/kornelski/cargo-deb#debian-packages-from-cargo-projects), and apparently the `-1` is desirable for compliance with Debian's packaging standard. OK, I'll take their word for it.

So for you, change

```
curl -LO "https://github.com/BurntSushi/ripgrep/releases/download/${RIPGREP_VERSION}/ripgrep_${RIPGREP_VERSION}_amd64.deb"
```

to

```
curl --fail -LO "https://github.com/BurntSushi/ripgrep/releases/download/${RIPGREP_VERSION}/ripgrep_${RIPGREP_VERSION}-1_amd64.deb"
```

Notice that I also added the `--fail` flag there. That would have also told you that you were getting a 404.

---

_Closed by @BurntSushi on 2023-11-30 12:22_

---

_Label `invalid` added by @BurntSushi on 2023-11-30 12:23_

---

_Comment by @Integralist on 2023-11-30 13:06_

Thanks. I'll be sure to use `--fail` in future (wasn't aware curl had that flag).

---
