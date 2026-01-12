```yaml
number: 2325
title: "Show capturing groups in `--json` mode"
type: issue
state: closed
author: pmkap
labels:
  - enhancement
  - question
assignees: []
created_at: 2022-10-07T21:01:56Z
updated_at: 2024-05-18T10:29:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2325
synced_at: 2026-01-12T16:13:24Z
```

# Show capturing groups in `--json` mode

---

_@pmkap_

I couldn't find any way to show capturing groups in `--json` mode.

Motivation:
Sometimes I'm interested in a group. After parsing the json, I need to filter the group in an additional step. Possibly with the same regex as in the first step. In such cases it would be nice if this was directly supported.

Example how this feature could look like:

`echo 'Hello! !World! !foo!' | rg --json '!(\w+?)!' | jq`

```json
"submatches": [
  {
    "match": {
      "text": "!World!"
    },
    "groups" : [
      {
        "1": "World"
      }
    ]
  },
  {
    "match": {
      "text": "!foo!"
    },
    "groups" : [
      {
        "1": "foo"
      }
    ]
  }
]
```

---

_Comment by @BurntSushi on 2022-10-07 21:13_

Related #1872

Could you please provide an end-to-end use case where you'd want this? My suspicion is that it isn't necessary. The other issue here is that resolving capturing groups can be slow, so it would need to be behind a flag, i.e., `--json-captures`. 

---

_Label `enhancement` added by @BurntSushi on 2022-10-07 21:13_

---

_Label `question` added by @BurntSushi on 2022-10-07 21:13_

---

_Comment by @pmkap on 2022-10-07 22:46_

Thank you for your help!

Like you suspected, I actually did find a solution (without the `--json` flag).

My use case was the following:
I am using [Logseq](https://github.com/logseq/logseq) for taking notes. It has pages that can reference each other with either `[[ref]]` of `#ref`. The idea that I'm playing around with is to search a directory for all those references with `rg` and hook the results into vim's completion.

The solution I now have is ` rg -o -e '\[\[(?P<g1>.+?)\]\]' -e '#(?P<g2>[^\s#]+)' -r '$g1$g2'`

I stumbled upon the `--json` flag in the manpage and thought it was a good idea to use...

---

_Closed by @pmkap on 2022-10-07 22:50_

---

_Comment by @acheronfail on 2023-06-21 01:48_

I'd like to consider re-opening this issue, since I have a use case for it!
I have built a tool that wraps `ripgrep` - it's called `repgrep`: https://github.com/acheronfail/repgrep/.

If we could somehow provide capturing groups in the JSON output (don't mind if it's gated behind a flag or something) then it would enable using `repgrep` to replace capturing groups, so users could match on something like `foo (\w+)` and then, when replacing with `repgrep`, they could use something like `bar $1` to use the capturing group in the replacement text.

---

I suppose the only way I could work around this, is if I used `ripgrep` as a `lib` (which still seems to be in the experimental phase). That would require a significant refactor of my tool, though - and until `libripgrep` is stable, I don't think it's a good idea for `repgrep`.

---

_Comment by @BurntSushi on 2023-06-21 20:43_

@acheronfail Is it possible for you to just re-run the regex on the matched lines to get capture groups?


---

_Comment by @acheronfail on 2023-06-22 09:49_

@BurntSushi that is a possible workaround, yes.

In fact, it does seem like this is the strategy that VSCode uses:

* [runs ripgrep when it's using the `node` backend](https://github.com/microsoft/vscode/blob/main/src/vs/workbench/services/search/node/ripgrepFileSearch.ts)
* [when replacing, uses JavaScript regexes to perform replacements](https://github.com/microsoft/vscode/blob/main/src/vs/workbench/services/search/common/replace.ts#L103)

So, my program doesn't depend on any regular expression functionality right now, so including a regex crate and using that to match on the lines would definitely be a solution to the issue. I can't imagine it would be that hard, perhaps the only thing is detecting whether a regular expression with groups was passed... I imagine a regex crate would be able to tell me this in some way.

In terms of a performance trade off - I don't think it would be that bad. Since I only need to perform the regular expression matches on visible lines (`repgrep` is a terminal user interface) I can make it fast enough.

The main bottleneck in performance in `repgrep` though, [is reading the JSON output itself ](https://github.com/acheronfail/repgrep/tree/master/benches), but that's only really fixed by using `ripgrep` as a lib.

Long story short: @BurntSushi I think it makes sense for `repgrep` just to re-run a regular expression on each matched line as needed! Forget about my comment, but I really appreciate you taking the time to humour me! :heart: 

---

_Comment by @CabalCrow on 2024-05-18 10:29_

I would like to reopen the issue since I also have a use case for it.

I'm using nushell to parse strings into structured data, but I want to use rg for the actual regex. The parsing requires the capture groups numbers & names, as well as what they actually capture to structure the data (each capture group number/name represent a column & each row is a match). Having a ``--json-captures`` flag would help a lot with this, since I could just parse the json instead to create that structured data. Currently I'm trying to just manually obtain the captured group names (via a regex checking the regex given to rg) & numbering to then programically create ``-or '1: $1\n...name:$name\n..'`` output for rg to then parse. This is obviously not the best solution - it would be more sensible to directly get the capture group names & what their contain directly from rg.

Additionally ``--json-captures`` is going to be very useful for debugging purposes when using rg.

---
