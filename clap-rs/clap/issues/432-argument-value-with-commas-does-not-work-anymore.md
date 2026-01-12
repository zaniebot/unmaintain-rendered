```yaml
number: 432
title: Argument value with commas does not work anymore with clap 2.x
type: issue
state: closed
author: andresv
labels: []
assignees: []
created_at: 2016-02-20T21:44:15Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/432
synced_at: 2026-01-12T16:14:09Z
```

# Argument value with commas does not work anymore with clap 2.x

---

_@andresv_

The following argument `--location lat=58.26566,lon=26.46615,alt=76.` worked with clap < 2.x.
It did not parse `lat`, `lon`, `alt` automatically, however I was able to read out whole set from `--location`. With clap 2.1.0 I just get `lat=58.26566`, so everything after comma is missing.

Example here: https://github.com/cubehub/doppler/blob/master/src/usage.rs#L199.


---

_Comment by @kbknapp on 2016-02-22 05:44_

That's because `clap` uses a comma to delimit values by default. This can be turned off however, or even changed to another delimiter.

To turn off delimiting (which sounds like what you're needing), see [`Arg::use_delimiter(false)`](http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#method.use_delimiter)

And then just FYI, to change the delimiter to another character, use [`Arg::value_delimiter(char)`](http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#method.value_delimiter)

I'll leave this open until you can confirm it's working for you and not some bug :wink:


---

_Comment by @andresv on 2016-02-22 20:30_

Allright, tried that and it works perfectly. So it was just RTFM issue.


---

_Closed by @andresv on 2016-02-22 20:30_

---
