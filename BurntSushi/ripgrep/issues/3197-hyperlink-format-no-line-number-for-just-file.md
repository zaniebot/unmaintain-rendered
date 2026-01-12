```yaml
number: 3197
title: "`--hyperlink-format-no-line-number` for just file path without line number (from `--heading`)"
type: issue
state: closed
author: wind0204
labels:
  - wontfix
assignees: []
created_at: 2025-10-21T08:04:17Z
updated_at: 2025-10-23T05:33:49Z
url: https://github.com/BurntSushi/ripgrep/issues/3197
synced_at: 2026-01-12T16:13:25Z
```

# `--hyperlink-format-no-line-number` for just file path without line number (from `--heading`)

---

_@wind0204_

Hello!

I would love to have `ripgrep` write a hyperlink in a different format when there is no line number information for the line, instead of writing 1 in the place of the line number or column number using the same `--hyperlink-format` format

I could imagine issuing a command sequence like this:
```
$ rg  --line-number --heading --hyperlink-format='myeditor:////{path}:{line}:{column}' --hyperlink-format-no-line-number='myeditor:////{path}
filepath
(hyperlink: myeditor://filepath)
linenr:columnnr:file_content
(hyperlink: myeditor://filepath:linenr:columnnr)
```
Opening the hyperlink for the heading would make `vim` open the file and go to the last cursor position that it has recorded in the `.viminfo` file. This behavior would be very nice to see!

The status quo:
```
$ rg  --line-number --heading --hyperlink-format='myeditor:////{path}:{line}:{column}'
filepath
(hyperlink: myeditor://filepath:1:1)
linenr:columnnr:file_content
(hyperlink: myeditor://filepath:linenr:columnnr)
```
In this case, opening the hyperlink for the heading would make `vim` open the file and overwrite the last cursor position in the `.viminfo` file that might have been useful to me.


Thank you so much!

---

_Comment by @BurntSushi on 2025-10-21 11:05_

I understand the request and I get why you want it, but I'm not adding more flags for this. ripgrep's output is not meant to be arbitrarily customizable. Sorry.

---

_Closed by @BurntSushi on 2025-10-21 11:05_

---

_Label `wontfix` added by @BurntSushi on 2025-10-21 11:05_

---

_Comment by @ltrzesniewski on 2025-10-21 11:41_

Just some thoughts:

We could trim everything after `{path}` in the format string, but I'm afraid this wouldn't be reliable enough. Or add a second pattern to the same format string after a separator, but I don't really like the idea either.

That being said, it would be easy to add a second pattern to the [*built-in* aliases](https://github.com/BurntSushi/ripgrep/blob/master/crates/printer/src/hyperlink/aliases.rs), but vim doesn't have any because AFAIK there's no standard way to open vim through a hyperlink.


---

_Comment by @wind0204 on 2025-10-21 11:49_

> We could trim everything after {path} in the format string, but I'm afraid this wouldn't be reliable enough. Or add a second pattern to the same format string after a separator, but I don't really like the idea either.

Yeah, I guess trimming after `{path}` would be totally fine for most users, including me.

In a rare case some people might need something like this in, my wild imagination:
`--hyperlink-format='{default_format=myeditor:////{path}:{line}:{column}@{host}},{heading_format=myeditor:////{path}@{host}}'`




---

_Comment by @wind0204 on 2025-10-21 12:08_

> That being said, it would be easy to add a second pattern to the [built-in aliases](https://github.com/BurntSushi/ripgrep/blob/master/crates/printer/src/hyperlink/aliases.rs), but vim doesn't have any because AFAIK there's no standard way to open vim through a hyperlink.

I just saw the code in `crates/printer/src/hyperlink/aliases.rs` to find out that macvim has a way to handle a special URL format `mvim://open?url=file://{path}&line={line}&column={column}`

And I can guess that macvim can remember the last cursor location like the traditional vim; And the feature I requested can benefit the users of macvim too, or at least a few future users..

Here's how I make my own solution:

~/.vim/vimrc:
```vim
    " When editing a file, always jump to the last known cursor position.
    " Don't do it when the position is invalid or
    " when inside an event handler (happens when dropping a file on gvim).
    autocmd BufWinEnter *
    \ if line("'\"") > 1 | 
    \   if line("'\"") > line("$") |
    \     exe "normal! ggj" |
    \   else |
    \     exe "normal! G" |
    \     if line("'\"") == line("$") && col("'\"") > col("$") |
    \         call setpos("'\"", [0,line("'\""),1,0]) |
    \         exe "normal! g'\"" |
    \     else |
    \       exe "normal! g`\"" |
    \     endif |
    \   endif |
    \ endif
```

~/bin/summon_editor.sh:
```sh
#!/bin/sh

file_to_open="$(echo $1 | sed -nre 's/^.+:\/\/([^:]+).*/\1/p')"
line_nr="$(echo $1 | sed -nre 's/^.+:\/\/([^:]+)(:([^:]+))?(:([^:]+))?/\3/p')"
column_nr="$(echo $1 | sed -nre 's/^.+:\/\/([^:]+)(:([^:]+))?(:([^:]+))?/\5/p')"

if [ -z "$column_nr" ]; then
  if [ -z "$line_nr" ]; then
    $EDITOR $file_to_open
  else
    $EDITOR "+${line_nr}" $file_to_open
  fi
else
  $EDITOR "+${line_nr}" -c "norm!$(expr $column_nr - 1)l" $file_to_open
fi
```

~/.local/share/applications/myeditor.desktop:
```desktop
[Desktop Entry]
Name=Summon my editor
Comment=Opens a file at a specific line
Exec=summon_editor.sh %u
Terminal=true
Type=Application
Categories=Utility;TextEditor;
StartupNotify=false
MimeType=text/english;text/plain;text/x-makefile;text/x-c++hdr;text/x-c++src;text/x-chdr;text/x-csrc;text/x-java;text/x-moc;text/x-pascal;text/x-tcl;text/x-tex;application/x-shellscript;text/x-c;text/x-c++;x-scheme-handler/myeditor;
```

~/.config/mimeapps.list:
```mimeapps.list
x-scheme-handler/myeditor=myeditor.desktop
```

ripgrep:
```sh
rg --hyperlink-format='myeditor:////{path}:{line}:{column}'
```

---
