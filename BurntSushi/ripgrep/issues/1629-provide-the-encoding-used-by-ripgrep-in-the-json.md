```yaml
number: 1629
title: "Provide the encoding used by ripgrep in the `--json` output"
type: issue
state: open
author: acheronfail
labels: []
assignees: []
created_at: 2020-06-25T21:55:30Z
updated_at: 2020-06-25T22:19:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1629
synced_at: 2026-01-12T16:13:23Z
```

# Provide the encoding used by ripgrep in the `--json` output

---

_@acheronfail_

#### Background context

I'm writing an [interactive replacer for ripgrep](https://github.com/acheronfail/repgrep). In a nutshell, my use case is somewhat similar to VSCode's:

* spawn `rg` with the `--json` flag
* parse the JSON output and build a UI from that
* allow the user to replace matched portions of files with some provided text

For `ascii` and `UTF-8` encoded files this works fine, since the `absolute_offset` included in the JSON output from ripgrep lines up with the bytes expected in the file (adjusting for the UTF-8 BOM header if it's present).

The tricky part is when ripgrep uses a different encoding - which as I understand it can happen in two ways:

1. the `-E` or `--encoding` flag is passed to ripgrep and that encoding is forced
2. ripgrep detects a BOM header and then uses that encoding

#### Describe your feature request

What I'd like to request is that the JSON output that ripgrep provides include an `encoding` field so that consumers of the JSON API can align with ripgrep. For the two cases above:

> the `-E` or `--encoding` flag is passed to ripgrep and that encoding is forced

As the caller of ripgrep, I can inspect its arguments and sniff the `-E` or `--encoding` flag myself, so while this isn't the nicest approach, it works well.

> ripgrep detects a BOM header and then uses that encoding

_I don't know that there's a reliable workaround for this_. I _could_ duplicate the logic that ripgrep itself uses internally, but that's subject to change and (in my opinion) is an internal implementation detail of ripgrep.

I think if the `begin` JSON object [documented here](https://docs.rs/grep-printer/0.1.5/grep_printer/struct.JSON.html#message-end) were to include an `encoding` field as a whatwg label (from what I can tell from the manpage ripgrep uses this internally) then my tool would not need to guess the encoding, but could rather uses the same encoding that ripgrep used.
I also think that the whatwg label is probably the best thing we have standard-wise for this kind of thing.

This would provide the following benefits:

* consumers of the `--json` API would be able to use the same encoding as ripgrep
* searching for the _correct_ match offset in a non UTF-8 encoded file would be easier, since we'd be using the same encoding
* consumers don't have to sniff the arguments sent to ripgrep as a part of guessing the encoding

---

_Comment by @acheronfail on 2020-06-25 22:19_

After a quick look at how VSCode handles this, I've noticed:

* Seems VSCode supports only `encoding=auto` or `encoding=utf8` [link](https://github.com/microsoft/vscode/blob/b385fa7ceb2b537e0ba2b58edaa162f4e299a947/src/vs/workbench/services/search/node/ripgrepTextSearchEngine.ts#L409-L411)

I haven't found exactly where VSCode performs the replacement of text (quite a large codebase). I'd be interested to know how they translate the matches provided by ripgrep in non UTF-8 encoded text to the actual replacements. When I have more time I'll try to investigate further.

---
