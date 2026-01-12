```yaml
number: 655
title: add notebook file extensions to types
type: pull_request
state: merged
author: mpacer
labels: []
assignees: []
merged: true
base: master
head: add_nb_type
created_at: 2017-10-25T20:48:55Z
updated_at: 2017-11-22T11:56:32Z
url: https://github.com/BurntSushi/ripgrep/pull/655
synced_at: 2026-01-12T18:23:13Z
```

# add notebook file extensions to types

---

_@mpacer_

This PR adds Jupyter notebook file extensions (`.ipynb`/`.jpynb`) to the type list. 

Some context: Notebooks are widely used in the scientific computing world. Under-the-hood are json files that are a bit unwieldy (understatement) to look at when printed to the command line. 

I used the `nb` key for notebook, but am happy to change it if that is preferred.

---

_Comment by @mpacer on 2017-10-27 18:30_

@BurntSushi it looks like the test failure may be related to resource availability and not the code I added. Is that an accurate assessment or do I need to do something else to make this pass the tests?

---

_Comment by @BurntSushi on 2017-10-29 14:10_

I believe Mathematica uses `nb` as an extension for their notebook files. How should we handle that?

(And yes, I believe the CI failures are unrelated to this PR.)

---

_Comment by @BurntSushi on 2017-11-01 11:11_

ping @mpacer 

---

_Comment by @carno on 2017-11-08 16:58_

How about `s/nb/jupyter/`?

---

_Comment by @mpacer on 2017-11-10 22:36_

oh! sorry totally missed this while I've been at a conference, I'm fine with using ipynb or jupyter, whichever is preferred.

---

_@BurntSushi approved on 2017-11-10 22:45_

Seems fine to me!

---

_Merged by @BurntSushi on 2017-11-22 11:56_

---

_Closed by @BurntSushi on 2017-11-22 11:56_

---
