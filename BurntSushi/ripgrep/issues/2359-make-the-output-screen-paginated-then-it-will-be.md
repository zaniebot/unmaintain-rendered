```yaml
number: 2359
title: make the output screen paginated then it will be easier to look into the search-result output.
type: issue
state: closed
author: pavanyogi
labels:
  - wontfix
assignees: []
created_at: 2022-11-24T12:55:19Z
updated_at: 2022-11-24T14:38:34Z
url: https://github.com/BurntSushi/ripgrep/issues/2359
synced_at: 2026-01-12T16:13:24Z
```

# make the output screen paginated then it will be easier to look into the search-result output.

---

_@pavanyogi_

#### make the output screen paginated instead of bumping the whole output at once

Please make the output of ripgrep as paginated instead of bumping the whole output at once. Sometimes the output spans more than a thousand lines and it makes it difficult to analyze the resulting output. If we can make the output paginated and as the user press the space key then more output should be visible.

there is one such utility script named bat https://github.com/sharkdp/bat . The output of the bat is paginated. A similar thing we can implement in the ripgrep.

although we can pipe the output to less command, but the color behavior of the search term is lost. 

The bat command output is attached here.
![Screenshot from 2022-11-24 18-07-18](https://user-images.githubusercontent.com/13834016/203789361-900dc2e8-76f3-4d85-857e-6e32df3d68ca.png)
![Screenshot from 2022-11-24 18-06-54](https://user-images.githubusercontent.com/13834016/203789379-6911aaaa-c2dc-4ebf-8346-eb4940863c7b.png)



---

_Comment by @BurntSushi on 2022-11-24 14:18_

This has been requested before. I'm not currently interested in adding this to ripgrep because you can use a pager yourself. And you can keep colors too: https://github.com/BurntSushi/dotfiles/blob/e366e9096d09aad5bf90247655af78fdc4110155/bin/rgp

---

_Closed by @BurntSushi on 2022-11-24 14:18_

---

_Label `wontfix` added by @BurntSushi on 2022-11-24 14:18_

---

_Comment by @Pavanyogi121 on 2022-11-24 14:38_

Thanks man.
The custom pager was an exact solution. Maybe we can add information about this custom pager script in the [readme file](https://github.com/BurntSushi/ripgrep/#readme).

---
