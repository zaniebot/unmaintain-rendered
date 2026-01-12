```yaml
number: 118
title: rg --type-add not working
type: issue
state: closed
author: Kerruba
labels: []
assignees: []
created_at: 2016-09-27T22:04:13Z
updated_at: 2016-09-27T23:14:42Z
url: https://github.com/BurntSushi/ripgrep/issues/118
synced_at: 2026-01-12T18:23:11Z
```

# rg --type-add not working

---

_@Kerruba_

Even tried to copy your example (ie rg --type-add 'foo:*.{foo,foobar}'), not adding the type to type-list.

Mac - El Capitain 10.11.6


---

_Comment by @BurntSushi on 2016-09-27 22:05_

Could you please provide the entire command you're running, its output and the output you expected? Thanks.


---

_Comment by @Kerruba on 2016-09-27 22:09_

typing `rg --type-add 'vue:*.vue'` is returning `Invalid argument`

See picture attached
![image](https://cloud.githubusercontent.com/assets/2854782/18893745/02fce0a6-8507-11e6-9bd8-b0d121c26aef.png)


---

_Comment by @BurntSushi on 2016-09-27 22:25_

That looks like expected behavior to me. Check out the docs for `--type-add`:

```
    --type-add ARG ...
        Add a new glob for a particular file type. Only one glob can be added
        at a time. Multiple type-add flags can be provided. Unless type-clear
        is used, globs are added to any existing globs inside of ripgrep. Note
        that this must be passed to every invocation of rg.

        Example: `--type-add html:*.html`
```

So you need to do `rg --type-add 'vue:*.{vue}' -tvue PATTERN`.

Perhaps we need more concrete examples. I think that's #76.


---

_Closed by @BurntSushi on 2016-09-27 22:25_

---

_Comment by @Kerruba on 2016-09-27 22:30_

Sorry, but is not exactly what I understand from the ["tour"](http://blog.burntsushi.net/ripgrep/#whirlwind-tour)

![image](https://cloud.githubusercontent.com/assets/2854782/18894308/11451d2e-850a-11e6-9343-2311bc53bbbf.png)

Reading that I would suppose that typing `rg --type-add 'foo:*.{foo,foobar}'` would return a message like "New type added" and that `rg --type-list` would list that. 


---

_Reopened by @BurntSushi on 2016-09-27 22:34_

---

_Comment by @BurntSushi on 2016-09-27 22:34_

You're right. Thank you for catching that. The tour is sadly wrong. Fixing now.


---

_Closed by @BurntSushi on 2016-09-27 22:35_

---

_Comment by @BurntSushi on 2016-09-27 22:35_

All set. Thanks!


---

_Comment by @Kerruba on 2016-09-27 22:47_

I'm sorry, don't want to seem pedant, is what I described before as "expected behaviour" for `rg --type-add ...` actually the behaviour?

Because this is what I get following the new instructions:
![image](https://cloud.githubusercontent.com/assets/2854782/18894726/809343a2-850c-11e6-9b3d-2aca5af0737f.png) 
this is definitely not what I'm expecting from this setting


---

_Comment by @BurntSushi on 2016-09-27 22:58_

I put this note in the `README`:

> `rg` won't persist your type settings

Let me be very clear: `--type-add` must always be present. `rg` has no config file, it does not persist any of the options. If you want to configure it to always be present, use an alias. e.g., `alias rg="rg --type-add 'foo:*.{foo,foobar}'`.

(We may grow a config file or something at some point.)


---

_Comment by @Kerruba on 2016-09-27 23:09_

Ok so why should I write such a long line when I can do this?
`rg bar -g "*.{foo,foobar}"`


---

_Comment by @BurntSushi on 2016-09-27 23:14_

`--type-add` is probably most useful in an alias.


---
