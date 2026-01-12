```yaml
number: 1591
title: "[FR] Extend --replace"
type: issue
state: closed
author: NightMachinery
labels:
  - doc
assignees: []
created_at: 2020-05-21T23:02:33Z
updated_at: 2020-05-22T02:23:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1591
synced_at: 2026-01-12T16:13:23Z
```

# [FR] Extend --replace

---

_@NightMachinery_

I want to both print a highlighted version of the text a la `rg --passthru ...` and further process matches. Currently this needs two calls of rg. If `--replace` supported something like `$1:$LINE`, I could just run a `cut -d: -f1` and `cut -d: -f2-1` to get the data, with just one rg call. This should perform better if there are lots of non-matching lines? ... Anyways I think it's a good feature to have.

Perhaps a better option is to add `--exec` that execs a command? Sth in the spirit of `--exec 'cp $1 ~/tmp'`.

---

_Renamed from "[FR] Extend --replace to allow using the entire line as a variable" to "[FR] Extend --replace" by @NightMachinery on 2020-05-21 23:02_

---

_Comment by @okdana on 2020-05-21 23:33_

I don't know about the `--exec` thing (doubt he'll go for that), but for the `$LINE` thing, you're just asking for this, aren't you?

```
% rg -r '$1:$0' '(foo) .+' <<< 'foo bar'
foo:foo bar
```

---

_Comment by @BurntSushi on 2020-05-21 23:48_

I'm not sure I understand the problem here. @NightMachinary Could you please provide a more detailed example with inputs and outputs?

(But yeah, I am unlikely to add an `--exec` flag or enrich the `--replace` language.)

---

_Label `question` added by @BurntSushi on 2020-05-21 23:48_

---

_Comment by @NightMachinery on 2020-05-22 02:02_

@okdana That `$0` should be added to the manpage. 
@BurntSushi The `--replace` functionality I wanted is this `$0` which is already there. :)
The `--exec` flag would be something just like `--replace`, but instead of printing the output you call `$SHELL -c $OUTPUT_HERE`. It's actually pretty easy to implement since you have the `--replace` part down already.

---

_Label `question` removed by @BurntSushi on 2020-05-22 02:18_

---

_Label `doc` added by @BurntSushi on 2020-05-22 02:18_

---

_Closed by @BurntSushi on 2020-05-22 02:23_

---
