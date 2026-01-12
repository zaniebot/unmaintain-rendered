```yaml
number: 833
title: require use of --option=value syntax
type: issue
state: closed
author: adeschamps
labels: []
assignees: []
created_at: 2017-01-30T07:17:37Z
updated_at: 2018-08-02T03:30:00Z
url: https://github.com/clap-rs/clap/issues/833
synced_at: 2026-01-12T16:14:09Z
```

# require use of --option=value syntax

---

_@adeschamps_

I'm looking at the `follow` option of the Unix `tail` command, and I'm not sure how to implement it. From the help output:

```
-f, --follow[={name|descriptor}]
                output appended data as the file grows;
                  an absent option argument means 'descriptor'
```

So the following are equivalent:

```
$ tail -f some-file
$ tail --follow some-file
$ tail --follow=descriptor some-file
```

Essentially, I think I need to only accept the `=` syntax, because in `$ tail --follow some-file`, `some-file` should be interpreted as a positional argument (in this case, the name of the file to print) and not as the value for the `follow` argument (which should default to `descriptor`). Is this possible?

---

_Comment by @kbknapp on 2017-01-30 15:30_

There currently isn't a way to *force* use of the `--option=value` syntax. Although, I could add this as an opt-in feature.

You're right that `--follow some-file` would currently be interpretted as `some-file` being the value to `--follow`. There is a way to allow options to have no values (`Arg::minimum_values(0)`), you'd have to use either `$ tail some-file --follow` or `$ tail --follow -- some-file` have it parse correctly in current `clap`.

I'll add this to the new feature request issues and should be able to knock it out this week.

---

_Label `C: settings` added by @kbknapp on 2017-01-30 15:30_

---

_Label `D: easy` added by @kbknapp on 2017-01-30 15:30_

---

_Label `T: new feature` added by @kbknapp on 2017-01-30 15:30_

---

_Label `T: new setting` added by @kbknapp on 2017-01-30 15:30_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-30 15:30_

---

_Label `C: options` added by @kbknapp on 2017-01-30 15:32_

---

_Renamed from "Optional option where value is not required." to "require use of --option=value syntax" by @kbknapp on 2017-01-30 15:32_

---

_Added to milestone `2.21.0` by @kbknapp on 2017-01-30 15:32_

---

_Closed by @homu on 2017-02-21 04:50_

---

_Comment by @adeschamps on 2017-03-19 15:11_

Just wanted to say a very late thank you!

---
