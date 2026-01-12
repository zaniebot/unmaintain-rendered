```yaml
number: 1425
title: Duplicate results
type: issue
state: closed
author: kkoomen
labels:
  - invalid
assignees: []
created_at: 2019-11-12T04:03:37Z
updated_at: 2019-11-12T11:18:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1425
synced_at: 2026-01-12T16:13:23Z
```

# Duplicate results

---

_@kkoomen_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

MacOS Mojave 10.14.6

#### Describe your question, feature request, or bug.

I'm getting duplicates results in some weird way. The command below is what I use:

```
rg --column --line-number --no-heading --fixed-strings --ignore-case --color "always" voteBtn
```

and this is the result:

```
model:9829:148:templates/general/jiathis_socialsharing.html:19:13:			<?= $this->tool('vote')->voteBtn(
model:22186:143:modules/listings/templates/review.html:18:14:				<?= $this->tool('vote')->voteBtn(
model:23866:145:modules/blog/templates/comment/item.html:14:14:				<?= $this->tool('vote')->voteBtn(
framework/tools/vote.class.php:8:18:	public function voteBtn($model, string $url, string $icon = null) {
templates/general/jiathis_socialsharing.html:19:29:			<?= $this->tool('vote')->voteBtn(
modules/listings/templates/review.html:18:30:				<?= $this->tool('vote')->voteBtn(
modules/blog/templates/comment/item.html:14:30:				<?= $this->tool('vote')->voteBtn(
```

#### If this is a bug, what is the expected behavior?

The result should be like this:

```
framework/tools/vote.class.php:8:18:	public function voteBtn($model, string $url, string $icon = null) {
templates/general/jiathis_socialsharing.html:19:29:			<?= $this->tool('vote')->voteBtn(
modules/listings/templates/review.html:18:30:				<?= $this->tool('vote')->voteBtn(
modules/blog/templates/comment/item.html:14:30:				<?= $this->tool('vote')->voteBtn(
```

<hr />

Is this expected behavior? Or do I have to enable some kind of flag?

---

_Comment by @kkoomen on 2019-11-12 05:59_

Sorry, this is not something related to the plugin. There was literally a file locally that was named `model` that had weird output.

---

_Closed by @kkoomen on 2019-11-12 05:59_

---

_Label `invalid` added by @BurntSushi on 2019-11-12 11:18_

---
