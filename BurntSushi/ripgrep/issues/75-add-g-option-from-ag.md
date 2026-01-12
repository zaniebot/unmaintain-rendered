```yaml
number: 75
title: "Add `-g` option from ag"
type: issue
state: closed
author: rubik
labels: []
assignees: []
created_at: 2016-09-25T08:03:54Z
updated_at: 2016-12-02T15:03:42Z
url: https://github.com/BurntSushi/ripgrep/issues/75
synced_at: 2026-01-12T16:13:21Z
```

# Add `-g` option from ag

---

_@rubik_

The `-g` option searches only in the file names. Example:

``` sh
$ ag -g init | head
scipy/sparse/linalg/eigen/arpack/__init__.py
scipy/sparse/linalg/isolve/__pycache__/__init__.cpython-35.pyc
scipy/sparse/linalg/isolve/__init__.py
scipy/sparse/linalg/__pycache__/__init__.cpython-35.pyc
scipy/sparse/linalg/__init__.py
scipy/sparse/linalg/dsolve/__pycache__/__init__.cpython-35.pyc
scipy/sparse/linalg/dsolve/__init__.py
scipy/sparse/__pycache__/__init__.cpython-35.pyc
scipy/sparse/__init__.py
scipy/sparse/csgraph/__pycache__/__init__.cpython-35.pyc
```

It's quite useful in combination with [`ctrlp.vim`](https://github.com/ctrlpvim/ctrlp.vim), to replace the Vim file finder with a faster alternative. Does it seem like a sensible addition to rg?


---

_Comment by @BurntSushi on 2016-09-25 13:09_

It's already there. Use `--files`. (Note that there's a bug with it accepting file path arguments in the latest release that is fixed on master. I'm getting a new release out today.)


---

_Closed by @BurntSushi on 2016-09-25 13:09_

---

_Comment by @rubik on 2016-09-25 20:11_

I had seen that option, but it's not equivalent:

``` sh
$ ag -g django
notebook/static/components/codemirror/mode/django/django.js
$ rg --files -u | wc -l
18281
$ rg --files -u | rg -N django
notebook/static/components/codemirror/mode/django/django.js
```

`ag -g` actually searches the file names, while `rg --files` just lists all the files.


---

_Comment by @BurntSushi on 2016-09-25 20:16_

@rubik Check out `rg --help`, please. `rg --files -g 'django'`. Note that `-g` is short for `--glob`, so you'll need to do `rg --files -g 'django*'` to get the same results. And use `rg --files -g '**/django/**'` to match anywhere in the path.


---

_Comment by @rubik on 2016-09-25 20:31_

Thanks, I actually read `man rg` but got confused. My bad.


---

_Comment by @pdbradley on 2016-12-02 12:00_

hey @rubik I also use Ctrlp, and I set up ripgrep as a replacment for silversearcher (ag), but one problem I'm having is that using ripgrep, the list of search results includes the full path to the file.  I don't like showing the full path, it clutters things.

Somehow using ag, the results only included the relative path of the project:

app/models/user.rb (for example)

and using rg, the result would show:

/User/myusername/projects/someproject/app/models/user.rb

I spent about an hour or two trying to find some option in either ctrlp or ripgrep related to this, and could not. 

I know this sort of might be the wrong place to solve this, but it might not be.  Here is my relevant vimrc below.  If you got ripgrep working with ctrlp in vim, could you tell me what I'm missing?  Thanks.

```
"WHY WONT ripgrep and ctrlp stop showing the full path and matching on it???!!!
"trying out ripgrep...use ag instead of ack with ack.vim; -i means case insensitive
 if executable('rg')
     let g:ctrlp_user_command = 'rg --files "" %s'
endif

"use ag instead of ack with ack.vim; -i means case insensitive
if executable('ag')
    let g:ctrlp_user_command = 'ag %s -l --nocolor -g ""'
endif
```


---

_Comment by @BurntSushi on 2016-12-02 12:14_

@pdbradley Running with `--files` outputs relative paths. I'd suggest simplifying your problem so that you can debug better. For example:

```
[andrew@Serval 75] rg --files
hi
foo/hi
foo/bar/hi
[andrew@Serval 75] pwd
/tmp/75
[andrew@Serval 75] rg --files $(pwd)
/tmp/75/hi
/tmp/75/foo/hi
/tmp/75/foo/bar/hi
```

Note the last example above. ripgrep outputs paths based on the paths you've specified.

---

_Comment by @BurntSushi on 2016-12-02 12:15_

Note that `ag` has the same behavior:

```
[andrew@Serval 75] ag -g . ./
hi
foo/hi
foo/bar/hi
[andrew@Serval 75] ag -g . $(pwd)
/tmp/75/hi
/tmp/75/foo/hi
/tmp/75/foo/bar/hi
```

---

_Comment by @pdbradley on 2016-12-02 15:03_

I figured it out.  In my vimrc I needed to change this:

```
     let g:ctrlp_user_command = 'rg --files "" %s'
```
to this:
```
     let g:ctrlp_user_command = 'rg --files  %s'

```

i.e, remove the "".  I don't know why I had that in there.  now the paths look fine in ctrlp.  thanks for your attention @BurntSushi I heard about ripgrep on The Bike Shed podcast and thought I would try it out.  great work.

---
