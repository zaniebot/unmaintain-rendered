```yaml
number: 215
title: Searching for strings that start with a dash is very difficult
type: issue
state: closed
author: alejandro5042
labels:
  - doc
assignees: []
created_at: 2016-11-02T22:56:10Z
updated_at: 2016-11-23T03:44:18Z
url: https://github.com/BurntSushi/ripgrep/issues/215
synced_at: 2026-01-12T18:23:11Z
```

# Searching for strings that start with a dash is very difficult

---

_@alejandro5042_

Create a file `test.txt` with the contents `-1`. The only way you can search this is by a `--regexp`/`-e` search:
```
0.03 - C:\Dev\Temp\rg-bug-negativenumbers> rg -e "-1"
test.txt
1:-1
```

Calling it without `-e` doesn't work:
```
0.03 - C:\Dev\Temp\rg-bug-negativenumbers> rg -1
Unknown flag: '-1'
```

A `--fixed-strings`/`-F` search doesn't work either. I expected this to work.

```
0.04 - C:\Dev\Temp\rg-bug-negativenumbers> rg -F "-1"
Unknown flag: '-1'
```

Other strings that are affected by this: command-line options.

(@pakdev actually found this issue.)

---

_Comment by @BurntSushi on 2016-11-02 22:59_

This is a classic issue with all command line tools. There are two accepted work-arounds. The first one, which you found already, is to explicitly use `-e`. The second one is to use `--` to indicate that all subsequent parameters are positional parameters. So in this case, that would be `rg -- -1`.

AFAIK, these are widely spread work-arounds. I'm not sure there's much more that can be done, and the same issue afflicts `grep`.


---

_Closed by @BurntSushi on 2016-11-02 22:59_

---

_Comment by @alejandro5042 on 2016-11-02 23:02_

I understand, however, I did expect `-F` to accept the '-1' parameter.

Perhaps, we can enforce that the pattern follows `-F` such that you can find these strings easily, mirroring how `-e` works:

```
      -e, --regexp PATTERN ...   Use PATTERN to search. This option can be
                                 provided multiple times, where all patterns
                                 given are searched.
>>    -F, --fixed-strings ***PATTERN***  Treat the pattern as a literal string instead of <<
                                         a regular expression.
```


---

_Comment by @BurntSushi on 2016-11-02 23:12_

I don't think `-F` should be changed. In its current form, it's precisely identical to `grep`'s `-F` flag. Moreover, `-F` modifies the meaning of _all_ patterns given, not just one. So if you gave `-F pat -e pat`, what would the behavior be? We could certainly pick one, but it seems like a kludge to me.

I feel like this is a well understood failure mode for CLI tools and ripgrep provides the work arounds that one would expect already.

Perhaps there could be an example showing a work around in the docs?


---

_Reopened by @BurntSushi on 2016-11-02 23:12_

---

_Label `doc` added by @BurntSushi on 2016-11-02 23:12_

---

_Comment by @kaushalmodi on 2016-11-03 16:02_

Dupe of https://github.com/BurntSushi/ripgrep/issues/102


---

_Closed by @BurntSushi on 2016-11-06 17:10_

---

_Comment by @alejandro5042 on 2016-11-07 15:23_

Thanks for the doc change!


---

_Comment by @alejandro5042 on 2016-11-10 05:33_

FYI - What I did to workaround these issues in my tool was encode the entire fixed string into Unicode code points (`\x{FFFF}`) and use regex instead. No problem now!


---

_Comment by @BurntSushi on 2016-11-10 11:35_

@alejandro5042 Do you mean something like this? `rg '\x2Dfoo`? Why do that instead of `rg -e -foo`?


---

_Comment by @BurntSushi on 2016-11-18 01:37_

Note that in the next release (`0.3`) this is unfortunately going to change because I switched argv parsers in #233. I [have a bug report open to fix the issue upstream](https://github.com/kbknapp/clap-rs/issues/742). The TL;DR is that `rg -e -foo` won't work, so you'll need to do either `rg -- -foo` or `rg -e [-]foo`.


---

_Comment by @alejandro5042 on 2016-11-23 02:59_

I escape the plain-text because AFAIK that's the only way to search for exactly that text.

Background: I built a wrapper script around `rg.exe` to pass the appropriate options and provide a simpler CLI interface for our source code; workaround some PowerShell issues (like this); and an option that returns the results as PowerShell objects for other scripts to consume. I also have the ability to export the results as a CSV file and append metadata such as the label/owner of the file (for pull requests).

I have a TODO to upgrade to rg 0.3.x and I'll run my tests to verify everything still works with this new argv parser. Thanks for your great utility!

---

_Comment by @BurntSushi on 2016-11-23 03:44_

Great! The good news is that this will get fixed again in 0.3.2, which i can hopefully get out tomorrow or the day after.

---
