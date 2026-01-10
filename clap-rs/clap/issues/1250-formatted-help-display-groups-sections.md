---
number: 1250
title: Formatted help display groups / sections
type: issue
state: closed
author: polarathene
labels: []
assignees: []
created_at: 2018-04-18T09:18:31Z
updated_at: 2018-08-02T03:30:22Z
url: https://github.com/clap-rs/clap/issues/1250
synced_at: 2026-01-10T01:26:47Z
---

# Formatted help display groups / sections

---

_Issue opened by @polarathene on 2018-04-18 09:18_

### Rust Version

1.25

### Affected Version of clap

2.31.2

---

I am porting a C project which has a help layout with options and flags grouped into sections.

Similar to https://github.com/kbknapp/clap-rs/issues/805 , but without heading sections. I thought that using groups would be useful for this in some way along with `AppSettings::DeriveDisplayOrder` and `AppSettings::UnifiedHelpMessage` to get a layout like so:

```
Looking Glass Client
Usage: looking-glass-client [OPTION]...
Example: looking-glass-client -h

  -h        Print out this help

  -f PATH   Specify the path to the shared memory file [current: /dev/shm/looking-glass]

  -s        Disable spice client
  -c HOST   Specify the spice host [current: 127.0.0.1]
  -p PORT   Specify the spice port [current: 5900]
  -j        Disable cursor position scaling
  -M        Don't hide the host cursor

  -k        Enable FPS display
  -g NAME   Force the use of a specific renderer
  -o OPTION Specify a renderer option (ie: opengl:vsync=0)
            Alternatively specify "list" to list all renderers and their options

  -a        Auto resize the window to the guest
  -n        Don't allow the window to be manually resized
  -r        Don't maintain the aspect ratio
  -d        Borderless mode
  -F        Borderless fullscreen mode
  -x XPOS   Initial window X position [current: 200]
  -y YPOS   Initial window Y position [current: 0]
  -w WIDTH  Initial window width [current: 1024]
  -b HEIGHT Initial window height [current: 768]
  -Q        Ignore requests to quit (ie: Alt+F4)

  -l        License information
```

I think this could be fairly simple to support with the v3 feature partially implemented with PR: https://github.com/kbknapp/clap-rs/pull/1211 . Using `.help_heading("")` seems to basically do it, although it still keeps the empty string in a new line resulting in two line breaks, it also repeats all options/flags in that heading(in testing I used the same help_heading `""` value, so it printed it twice but with all 4 options in each heading instance).

While it could use something like `.group()` it's probably not appropriate(nor with multiple groups involved). `.args(&[...])` could be a good way to group for this purpose(and might appropriate for the current `.help_heading()` too to avoid the duplication issue?). I'm not too familiar with what `.args()` is used for vs `.arg()`, so maybe there might be an issue with that?

---

_Comment by @kbknapp on 2018-06-05 01:27_

Does `DeriveDisplayOrder` and `UnifiedHelpMessage` not give the correct results? 

I could *maybe* be considered to call the `help_heading("")` with a blank line or spaces a bug...but it would be a very low priority bug :stuck_out_tongue_winking_eye: 

I'm going to close this issue for the time being, but if there is something we need to fix we can re-open.

---

_Closed by @kbknapp on 2018-06-05 01:27_

---

_Referenced in [stickeritis/sticker#147](../../stickeritis/sticker/issues/147.md) on 2019-11-05 22:59_

---
