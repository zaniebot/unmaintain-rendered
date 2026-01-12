```yaml
number: 1304
title: "Why does ripgrep traverse into directories that don't match the provided glob?"
type: issue
state: closed
author: oliversalzburg
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2019-06-17T20:39:36Z
updated_at: 2019-06-17T22:00:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1304
synced_at: 2026-01-12T16:13:23Z
```

# Why does ripgrep traverse into directories that don't match the provided glob?

---

_@oliversalzburg_

#### What version of ripgrep are you using?

```shell
ripgrep 11.0.1 (rev 973de50c9e)
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

Downloaded the binary from the GitHub Releases page.

#### What operating system are you using ripgrep on?

Windows 10 x64

#### Describe your question, feature request, or bug.

Through [an issue](https://github.com/microsoft/vscode/issues/75314) I was experiencing with an [extension for VS Code](https://github.com/hbenl/vscode-mocha-test-adapter/issues/56), I looked at the behavior of ripgrep, given the underlying command that was causing the excessive load.

The relevant parts of the command are:

```shell
rg --files --follow --no-ignore -g /test/**/*.js
```

This will quickly produce the desired file names from the `test` subfolder in the current working directory and then it will print out huge chains of folder names (resulting from symlinks) in a folder named `node_modules` in the current working directory.

Now my question is, why does `ripgrep` even traverse down into the `node_modules` folder, when it doesn't match the requested glob anyway? Is that by-design or is that unintended behavior?

---

_Comment by @BurntSushi on 2019-06-17 21:23_

> Now my question is, why does ripgrep even traverse down into the node_modules folder, when it doesn't match the requested glob anyway? Is that by-design or is that unintended behavior?

Neither. It's simply a missed performance optimization. While it's obvious to us humans that looking in `node_modules` is a waste of time, ripgrep doesn't know that because it doesn't do the sophisticated analysis on globs required to implement the optimization you want here. In particular, ripgrep _must_ descend into directories when given a glob like this unless the glob specifically excludes that directory, otherwise ripgrep might miss files. For example, the path `/test` does _not_ match the glob` /test/**/*.js`. So in order to implement this optimization, ripgrep would need to reason about glob prefixes and the like.

Moreover, the command you've presented is fairly idiosyncratic. I don't understand why you're using in a glob for this instead of just using your shell's glob expansion to search the `test` directory directly. e.g., `rg --files --follow --no-ignore /test/**/*.js`. If your shell isn't capable of that, then try `rg --files --follow --no-ignore /test -g '*.js'`.

Invariably, this is a duplicate of #546, so I'm going to close this one as well for the same reason.

---

_Closed by @BurntSushi on 2019-06-17 21:23_

---

_Label `duplicate` added by @BurntSushi on 2019-06-17 21:23_

---

_Label `enhancement` added by @BurntSushi on 2019-06-17 21:23_

---

_Comment by @oliversalzburg on 2019-06-17 22:00_

Awesome. Thanks a lot for the detailed explanation.

The presented command is not issued in a shell and I have no direct control
over it either, as it is third-party code invoking ripgrep. I was merely
trying to understand why things are happening as they are.

You explained it well. Thanks again.

On Mon, Jun 17, 2019, 23:23 Andrew Gallant <notifications@github.com> wrote:

> Now my question is, why does ripgrep even traverse down into the
> node_modules folder, when it doesn't match the requested glob anyway? Is
> that by-design or is that unintended behavior?
>
> Neither. It's simply a missed performance optimization. While it's obvious
> to us humans that looking in node_modules is a waste of time, ripgrep
> doesn't know that because it doesn't do the sophisticated analysis on globs
> required to implement the optimization you want here. In particular,
> ripgrep *must* descend into directories when given a glob like this
> unless the glob specifically excludes that directory, otherwise ripgrep
> might miss files. For example, the path /test does *not* match the glob
> /test/**/*.js. So in order to implement this optimization, ripgrep would
> need to reason about glob prefixes and the like.
>
> Moreover, the command you've presented is fairly idiosyncratic. I don't
> understand why you're using in a glob for this instead of just using your
> shell's glob expansion to search the test directory directly. e.g., rg
> --files --follow --no-ignore /test/**/*.js. If your shell isn't capable
> of that, then try rg --files --follow --no-ignore /test -g '*.js'.
>
> Invariably, this is a duplicate of #546
> <https://github.com/BurntSushi/ripgrep/issues/546>, so I'm going to close
> this one as well for the same reason.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1304?email_source=notifications&email_token=AAMVARKRP5D4ZZMNGBJVKM3P276GHA5CNFSM4HY2CLO2YY3PNVWWK3TUL52HS4DFVREXG43VMVBW63LNMVXHJKTDN5WW2ZLOORPWSZGODX4PVFI#issuecomment-502856341>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAMVARL5K4AE4OOFSV42EVDP276GHANCNFSM4HY2CLOQ>
> .
>


---
