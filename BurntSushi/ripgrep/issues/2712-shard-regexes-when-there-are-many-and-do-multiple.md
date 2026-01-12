```yaml
number: 2712
title: shard regexes when there are many and do multiple passes over the haystack
type: issue
state: closed
author: kochbj
labels:
  - question
assignees: []
created_at: 2024-01-11T23:34:52Z
updated_at: 2024-01-14T19:00:52Z
url: https://github.com/BurntSushi/ripgrep/issues/2712
synced_at: 2026-01-12T16:13:24Z
```

# shard regexes when there are many and do multiple passes over the haystack

---

_@kochbj_

#### Describe your feature request

Huge ripgrep fan. Thanks for making such a great program. I don't know how hard this would be, but I often have files with 1000s of patterns. It's inconvenient that I have to use `split` to create separate files first and then run rg again in a loop. Could rg just split the patterns file itself and keep the shards in memory? No worries if this is an impractical idea! 


---

_Comment by @BurntSushi on 2024-01-12 00:01_

I don't understand the request. Whether you shard them manually or not, they all get compiled into a single regex pattern by joining them with `|`. I think it would be better to avoid the XY problem and describe the real thing you're trying to do. This means including the command you're running, the inputs you're giving to it, the actual output and the expected/desired output. As Jerry Maguire says, _[help me help you](https://www.youtube.com/watch?v=l1B1_jQnlFk)_.

Regex engines (with the exception of things like Hyperscan and other more niche engines) generally don't scale well to that many patterns. You're basically up shit's creek without a paddle. ripgrep's default regex engine is based on finite automata and so will generally do much better than a backtracker like PCRE2, but it has its limits. One special case here is if all your patterns are literals (which becomes true if you pass `-F`). In that case, ripgrep can do a little better by diverting to Aho-Corasick.

---

_Comment by @kochbj on 2024-01-12 03:23_

Thanks for the quick reply, and the advice about ```-F```; thats helpful.  I'm asking (or really just suggesting) a convenience wrapper for when the regex engine maxes out. If you do ```-if``` with a pattern file with 2M lines, it throws this error:

```compiled regex exceeds size limit of 104857600```

This could be avoided if rg just ran multiple loops through the data at max size chunks in the pattern file. I know this isn't normal grep behavior, but would be useful to me as an extra flag for -f. Here's a pattern file; it's fixed strings, but you can reproduce the behavior with ```-i```.

[rg_all_conferences_papers.inArxiv.citations.citingids.txt.zip](https://github.com/BurntSushi/ripgrep/files/13911174/rg_all_conferences_papers.inArxiv.citations.citingids.txt.zip)


---

_Comment by @BurntSushi on 2024-01-14 14:50_

> This could be avoided if rg just ran multiple loops through the data at max size chunks in the pattern file.

Ah yeah this isn't going to happen, sorry. To you it might look like one extra little flag, but to me it looks like completely reshuffling the internals. And it looks like a flag that begets more flags. This sort of thing is definitely something you'll want to build as a layer on top of ripgrep. Building something bespoke is much easier than targeting the general case.

> If you do `-if` with a pattern file with 2M lines, it throws this error:

So for your data, while it contains 2 million patterns, it's actually less than half of that because of duplicates.

But right. As soon as you throw `-i/--ignore-case` in there, the regex engine has to get involved in order to implement case insensitive searching. Did you know that you can configure different size limits? The defaults are just there so that you don't accidentally consume a whole bunch of memory. But you can do, e.g., `--regex-size-limit 10G --dfa-size-limit 10G` and searching for your patterns will work, even with `-i/--ignore-case` enabled. It won't be the fastest thing in the world, but it works.

---

_Comment by @kochbj on 2024-01-14 17:44_

Haha, I figured that's what you'd say. Fair enough. I did not notice the regex-size-limit flag. That's super useful! Anyways will close the issue. Thanks for the quick responses.

P.S. I meant to grab the uniq one, but it made the point. :)

---

_Closed by @kochbj on 2024-01-14 17:44_

---

_Label `question` added by @BurntSushi on 2024-01-14 19:00_

---

_Renamed from "Infinite file length for -f " to "shard regexes when there are many and do multiple passes over the haystack" by @BurntSushi on 2024-01-14 19:00_

---
