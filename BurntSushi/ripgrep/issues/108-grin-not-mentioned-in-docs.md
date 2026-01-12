```yaml
number: 108
title: "\"grin\" not mentioned in docs"
type: issue
state: closed
author: hmeine
labels:
  - question
assignees: []
created_at: 2016-09-26T19:45:02Z
updated_at: 2016-09-27T08:01:28Z
url: https://github.com/BurntSushi/ripgrep/issues/108
synced_at: 2026-01-12T18:23:11Z
```

# "grin" not mentioned in docs

---

_@hmeine_

I am a long-term [grin](http://blog.enthought.com/python/grin-11/)-user (link should go to https://pypi.python.org/pypi/grin/1.2.1 , but pypi seems to be down ATM).  Not saying it is better in any way, but maybe you want to mention it in your documentation.

Same goes for http://rak.rubyforge.org ?

Anyhow, thanks for the nice background documentation on why some tools are faster than others. :-)


---

_Comment by @BurntSushi on 2016-09-26 20:53_

I'll take a look at both. I think I had come across them in my travels, but hadn't taken a serious look. I'm kind of assuming that since they're written in Python/Ruby that they won't be competitive.

I probably also don't want to get into the business of listing every search tool in existence either. But I'll at least check them out. Thanks for the heads up.


---

_Label `question` added by @BurntSushi on 2016-09-26 20:59_

---

_Comment by @BurntSushi on 2016-09-27 00:18_

`grin` is very very slow. This meets my expectation, not only because it's Python, but because Python's regex engine isn't known for being particularly speedy. Here are some very basic benchmarks.

```
(grin) [andrew@Cheetah linux] time rg PM_RESUME | wc -l
16

real    0m0.289s
user    0m1.727s
sys     0m0.417s
(grin) [andrew@Cheetah linux] time rg '[A-Z]+_RESUME' | wc -l
1652

real    0m0.294s
user    0m1.697s
sys     0m0.460s
(grin) [andrew@Cheetah linux] time grin PM_RESUME | rg -v '^\./' | wc -l
16

real    0m3.024s
user    0m2.527s
sys     0m0.503s
(grin) [andrew@Cheetah linux] time grin '[A-Z]+_RESUME' | rg -v '^\./' | wc -l
1653

real    0m17.105s
user    0m16.593s
sys     0m0.527s
```

`grin` hasn't seen any updates in six years. The project is dead. It's not worth including in the README.


---

_Comment by @BurntSushi on 2016-09-27 00:26_

`rak` is even worse on literals, but doesn't degrade as badly on a more complex regexes like `grin` does. My guess is that `grin` does a literal optimization while `rak` doesn't, but I haven't looked at the source code of either.

```
[andrew@Cheetah linux] time rak PM_RESUME | rg '^\S' | wc -l
16

real    0m13.038s
user    0m12.467s
sys     0m0.567s
[andrew@Cheetah linux] time rak '[A-Z]+_RESUME' | rg '^\S' | wc -l
1637

real    0m12.901s
user    0m12.323s
sys     0m0.573s
[andrew@Cheetah linux] time rak 'ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT' | rg '^\S' | wc -l
68

real    0m14.508s
user    0m13.980s
sys     0m0.530s
[andrew@Cheetah linux] time rak -i 'ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT' | rg '^\S' | wc -l
160

real    0m21.410s
user    0m20.877s
sys     0m0.533s
```

And `rg` for the last two:

```
[andrew@Cheetah linux] time rg 'ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT' | wc -l
68

real    0m0.292s
user    0m1.830s
sys     0m0.327s
[andrew@Cheetah linux] time rg -i 'ERR_SYS|PME_TURN_OFF|LINK_REQ_RST|CFG_BME_EVT' | wc -l
160

real    0m0.291s
user    0m1.730s
sys     0m0.413s
```

(Notice how `rg` doesn't even blink at the `-i` flag. :-))


---

_Closed by @BurntSushi on 2016-09-27 00:26_

---

_Comment by @hmeine on 2016-09-27 08:01_

Thanks.  I sort of expected that slowness, too, but I also liked your summary of the background of the different regex engines etc., so I thought you might want to mention them in the documentation / blog post.
Again, thanks for all the effort!


---
