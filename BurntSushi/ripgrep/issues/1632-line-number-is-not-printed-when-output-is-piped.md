```yaml
number: 1632
title: Line number is not printed when output is piped to GNU sort
type: issue
state: closed
author: lamyergeier
labels:
  - invalid
assignees: []
created_at: 2020-06-28T23:47:38Z
updated_at: 2020-06-29T00:03:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1632
synced_at: 2026-01-12T16:13:23Z
```

# Line number is not printed when output is piped to GNU sort

---

_@lamyergeier_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 12.0.1
```

```
$ sort --version
sort (GNU coreutils) 8.30
```

#### How did you install ripgrep?
linuxbrew

#### What operating system are you using ripgrep on?

Ubuntu 20.04


#### Describe your bug.

Line number is not printed when output is piped to GNU sort.  Thus GNU Sort is not able to sort as per line number!

> Note: I am not using `--sort-file` inbuilt option of ripgrep.

#### What are the steps to reproduce the behavior?

```
$ grep --with-filename --line-number 'ripgrep' *.md | sort
ImprovedAlternateUtilities.md:21:| grep             | [BurntSushi/ripgrep][], [beyondgrep/ack3][]                   
ImprovedAlternateUtilities.md:55:  [BurntSushi/ripgrep]: https://github.com/BurntSushi/ripgrep
```

```
$ rg -t md 'ripgrep' | sort
ImprovedAlternateUtilities.md:  [BurntSushi/ripgrep]: https://github.com/BurntSushi/ripgrep
ImprovedAlternateUtilities.md:| grep             | [BurntSushi/ripgrep][], [beyondgrep/ack3][]        
```

```
$ rg -t md 'ripgrep' 
ImprovedAlternateUtilities.md
21:| grep             | [BurntSushi/ripgrep][], [beyondgrep/ack3][]          
55:  [BurntSushi/ripgrep]: https://github.com/BurntSushi/ripgrep
```

#### What is the expected behavior?

```
$ rg -t md 'ripgrep' | sort
ImprovedAlternateUtilities.md:21:| grep             | [BurntSushi/ripgrep][], [beyondgrep/ack3][]        
ImprovedAlternateUtilities.md:55:  [BurntSushi/ripgrep]: https://github.com/BurntSushi/ripgrep
```

Note that the sorting order is correct as it is sorted by line number!

---

_Comment by @BurntSushi on 2020-06-29 00:03_

Yes, this is correct. Pass `-n/--line-number` just like you would with grep. Aa you can see in your examples, ripgrep changes its output format depending on whether it's printing to a tty or not.

---

_Closed by @BurntSushi on 2020-06-29 00:03_

---

_Label `invalid` added by @BurntSushi on 2020-06-29 00:03_

---
