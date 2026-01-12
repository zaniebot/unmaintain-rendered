```yaml
number: 2186
title: Support for special characters (such as new line characters) for --replace
type: issue
state: closed
author: kenorb
labels: []
assignees: []
created_at: 2022-04-18T15:51:53Z
updated_at: 2022-04-19T14:21:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2186
synced_at: 2026-01-12T16:13:24Z
```

# Support for special characters (such as new line characters) for --replace

---

_@kenorb_

#### Describe your feature request

I've got the following command to grep for 2 words:

    $ echo foo bar | rg -o '(\w+)\s(\w+)'
    foo bar

However I would like to extend it to print the second line with the opposite order.

For example:

    $ echo foo bar | rg -o '(\w+)\s(\w+)' -r '$1 $2\n$2 $1'
    foo bar\nbar foo

However new line character (`\n`) isn't processed.

Same with: `-r "\$1 \$2\n\$2 \$1"`.

I've tried with `^M`, but it prints the 2nd line only

    $ echo foo bar | rg -o '(\w+)\s(\w+)' -r '$1 $2^M$2 $1'
    bar foo

Here is the workaround with expected results:

    $ echo foo bar | rg -o '(\w+)\s(\w+)' -r "\$1 \$2$(printf '\n\r')\$2 \$1"
    foo bar
    bar foo

however it's still not working as expected (it's adding ^M at the beginning of the line, and `\n` alone doesn't work, so adding `tr -d "\r"` on top of it works).

The feature is to add a support for special characters for replacement text.

Version:

    $ rg --version
    ripgrep 12.1.1


---

_Comment by @kenorb on 2022-04-19 11:06_

I think I've found the acceptable workaround:

```
% echo foo bar | rg -o '(\w+)\s(\w+)' -r "$(printf '$1 $2\n$2 $1')"
foo bar
bar foo
```

or by mixing it with another command:

```
% echo foo bar | rg -o '(\w+)\s(\w+)' -r "$(crunch 5 5 + + 12 -t '$% $%' 2>/dev/null)"
foo foo
foo bar
bar foo
bar bar
```

This should be enough for me.


---

_Closed by @kenorb on 2022-04-19 11:06_

---
