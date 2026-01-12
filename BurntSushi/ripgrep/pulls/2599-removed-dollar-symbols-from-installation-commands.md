```yaml
number: 2599
title: removed dollar symbols from installation commands in order to make copying them useful
type: pull_request
state: closed
author: bwv847
labels: []
assignees: []
base: master
head: rm-dollar-symbols-from-codeblocks
created_at: 2023-08-26T13:22:15Z
updated_at: 2025-09-17T12:15:14Z
url: https://github.com/BurntSushi/ripgrep/pull/2599
synced_at: 2026-01-12T18:23:14Z
```

# removed dollar symbols from installation commands in order to make copying them useful

---

_@bwv847_

_No description provided._

---

_Comment by @BurntSushi on 2023-08-26 13:26_

I'm personally not a huge fan of how this looks personally. It feels like it all just runs together. Although I do admit that this makes copying commands much more useful because of GitHub's "copy" button. On the other hand, I'm not necessarily a huge fan of encouraging folks to blindly run commands from a GitHub README, especially when they contain `sudo` (as many of these do). So... the extra speed bump might actually be desirable.

---

_Closed by @BurntSushi on 2023-08-29 02:49_

---

_Comment by @hey-leon on 2025-09-17 12:13_

@BurntSushi I understand you already have a stance on this but if I could play devils advocate here. I think there are two camps of developer on your argument:

__Camp A__: Thoroughly reads the command before even copying the command (who would benefit from it working without modification).

__Camp B__: Will blindly copy and paste see the error because of the `$` and will proceed to remove the dollar sign, still without fully considering the command.

While I appreciate there is probably also developers that don't fit these two camps. I think in practice the speedbump probably would not save many developers 🤷‍♂️.

Either way thanks for taking the time to review these PRs!

---
