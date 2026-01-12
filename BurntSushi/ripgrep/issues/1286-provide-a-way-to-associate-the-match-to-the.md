```yaml
number: 1286
title: Provide a way to associate the match to the pattern
type: issue
state: closed
author: kewisch
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2019-05-28T10:17:09Z
updated_at: 2024-05-22T20:20:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1286
synced_at: 2026-01-12T16:13:23Z
```

# Provide a way to associate the match to the pattern

---

_@kewisch_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### Describe your feature request

When running `rg -f patternfile` with multiple patterns, I'm wanting to find out which pattern triggered the match so I can group them in my results. Assuming a pattern file like

```
foo+
bar
```

##### A new option `--with-pattern`

Calling `rg -f patternfile --no-heading --with-pattern --with-filename --line-number` would show output like

```
foo:src/cli.js:      // get all your foooooooo here
bar:src/quality.js   // There is a high bar for quality
```
This might need to be displayed somewhat differently with multiple patterns per line or with different options.

##### Including pattern in JSON output
This is what I am most interested in. When calling with `--json`, regardless of the pattern option, matches should include the pattern data:

```json
{
   "type" : "match",
   "data" : {
      "absolute_offset" : 0, "line_number" : 1,
      "path" : { "text" : "src/cli.js" },
      "submatches" : [
        {
            "match" : { "text" : "foooooooo" },
            "pattern" : "foo+",
            "start" : 16, "end" : 25
         }
      ],
      
      "lines" : { "text" : "// get all your foooooooo here\n" }
   }
}
```

##### Alternatives
If this doesn't fit in well, what about exposing the named capture groups in the JSON output? This way I could modify the pattern to `(?P<identifier>foo+)` and in whatever way you choose read this from the JSON data.

---

_Comment by @BurntSushi on 2019-05-28 12:05_

I think there are too many variables, too much complexity and not enough user demand for a feature like this. I'd recommend writing your own program to do this. You can even use ripgrep's libraries to do it.

---

_Closed by @BurntSushi on 2019-05-28 12:05_

---

_Comment by @BurntSushi on 2019-05-28 12:05_

(I'm pretty sure this is a duplicate, but I couldn't find the original issue.)

---

_Label `enhancement` added by @BurntSushi on 2019-05-28 12:05_

---

_Label `question` added by @BurntSushi on 2019-05-28 12:05_

---

_Label `wontfix` added by @BurntSushi on 2019-05-28 12:05_

---

_Label `question` removed by @BurntSushi on 2019-05-28 12:05_

---

_Comment by @kewisch on 2019-05-28 16:59_

Would you accept a patch to ripgrep to add the pattern to the JSON output, or are you specifically saying you would not want this feature in ripgrep?

---

_Comment by @BurntSushi on 2019-05-28 17:01_

I do not think this feature belongs in ripgrep at the current time.

Also, it is possible for multiple patterns to match the same text, so you'd need to include all of them. In general, this feature is more difficult to implement than you might imagine, particularly if performance is a concern.

---

_Comment by @kewisch on 2019-05-28 17:05_

Ok, thanks for the insight and for considering! I'll see what I can do without.

---

_Comment by @TheTechromancer on 2024-05-22 15:11_

@kewisch out of curiosity, did you ever find a solution to your problem of correlating regex patterns to their matches? I am looking at a similar problem.

---

_Comment by @kewisch on 2024-05-22 19:22_

It has been quite a while and for a past job. I don't believe we created something substantial based on ripgrep that solved this issue, looking back over the code we had for this I think we managed to work around by not relying on the pattern at all.

---

_Comment by @TheTechromancer on 2024-05-22 20:20_

Thanks. Right now it's looking like the answer to our problem is [yara](https://github.com/VirusTotal/yara).

---
