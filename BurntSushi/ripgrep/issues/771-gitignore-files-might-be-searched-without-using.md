```yaml
number: 771
title: ".gitignore files might be searched without using '--hidden'"
type: issue
state: closed
author: sharkdp
labels:
  - question
assignees: []
created_at: 2018-02-01T21:57:27Z
updated_at: 2018-02-01T22:24:38Z
url: https://github.com/BurntSushi/ripgrep/issues/771
synced_at: 2026-01-12T16:13:22Z
```

# .gitignore files might be searched without using '--hidden'

---

_@sharkdp_

I have to admit that I found this quite confusing when I discovered this by accident:
``` bash
> ls -a
.  ..  .gitignore

> # .gitignore is searched, even though we did not specify `--hidden` or `-uu`:
> rg --files
.gitignore

> # .gitignore is not searched when we specify `--no-ignore`
> rg --files --no-ignore
```

I found out that this was caused by a `!.gitignore` entry inside the `.gitignore` file. It looks like this ripgrep behavior is actually by design (see #90), but I just wanted to make sure - given that this seems to be a [common pattern](https://github.com/search?q=%22%21.gitignore%22&type=Code&utf8=%E2%9C%93).

---

_Comment by @BurntSushi on 2018-02-01 22:08_

This seems like it's working as intended? ripgrep respects whitelists in your `.gitignore` file. This is why `.ignore` files exist---so you can override things that make sense for git tracking but not for searching.

I understand you might have been confused by this, but what other behavior would you expect here? Note that running ripgrep with `--debug` will usually tell you if a file has been whitelisted. Example:

```
[andrew@Cheetah ripgrep-771] cat .gitignore 
!.gitignore
[andrew@Cheetah ripgrep-771] rg --files --debug
DEBUG:grep::search: regex ast:
Repeat {                                                                                                                      
    e: Literal {                                                                                                                                                                                                     
        chars: [                                                                                                                                                                                                     
            'z'                                                                                                                                                                                                      
        ],                                                                                                                                                                                                           
        casei: false                                                                                                                                                                                                 
    },                                                                                                                                                                                                               
    r: Range {                                                                                                                                                                                                       
        min: 0,                                                                                                                                                                                                      
        max: Some(                                                                                                                                                                                                   
            0                                                                                                                                                                                                        
        )                                                                                                                                                                                                            
    },                                                                                                                                                                                                               
    greedy: true                                                                                                                                                                                                     
}                                                                                                                                                                                                                    
DEBUG:globset: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes                                                                                       
DEBUG:ignore::walk: whitelisting ./.gitignore: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!.gitignore", actual: "**/.gitignore", is_whitelist: true, is_only_dir: false })))      
.gitignore
```

Notice the bit at the end about whitelisting `./.gitignore`.

---

_Label `question` added by @BurntSushi on 2018-02-01 22:08_

---

_Comment by @sharkdp on 2018-02-01 22:24_

@BurntSushi Thank you for the clarification!

I was confused/surprised because I somehow expected the hidden/non-hidden distinction to take precedence over ignored/non-ignored. I would have expected hidden files to never be searched unless `--hidden` was specified.

But I guess it makes sense when thinking in terms of whitelisting. This example might have been particularly surprising because the `.gitignore` file whitelisted itself (for reasons that are obviously not related to this file having valuable content to search through :smile:).

> Note that running ripgrep with --debug will usually tell you if a file has been whitelisted

Thanks, I didn't know that!

---

_Closed by @sharkdp on 2018-02-01 22:24_

---
