```yaml
number: 888
title: "ripgrep searches \"no files\" (No such file or directory (os error 2)) but grep works"
type: issue
state: closed
author: gerdneuman
labels: []
assignees: []
created_at: 2018-04-17T10:33:35Z
updated_at: 2018-04-17T12:36:50Z
url: https://github.com/BurntSushi/ripgrep/issues/888
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep searches "no files" (No such file or directory (os error 2)) but grep works

---

_@gerdneuman_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### What operating system are you using ripgrep on?

Ubuntu 16.04. rg is installed using snap as outlined in the README here.

#### Describe your question, feature request, or bug.

I used to use grep to search through wordpress code under `/var/www/` but thought of giving `rg` a try. But what is working with `grep` results in an error with `rg`.

So with `grep` I did:
```sh
$ grep -R calculate_order_weight plugins/
plugins/dhl-for-woocommerce/includes/abstract-pr-dhl-wc-order.php:			$selected_weight_val = $this->calculate_order_weight( $order_id );
plugins/dhl-for-woocommerce/includes/abstract-pr-dhl-wc-order.php:	protected function calculate_order_weight( $order_id ) {
```

with `rg` I get:
```sh
$ rg calculate_order_weight plugins/
plugins/: No such file or directory (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

Running the same with `--debug`:
```sh
$ rg --debug calculate_order_weight plugins/
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'c',
        'a',
        'l',
        'c',
        'u',
        'l',
        'a',
        't',
        'e',
        '_',
        'o',
        'r',
        'd',
        'e',
        'r',
        '_',
        'w',
        'e',
        'i',
        'g',
        'h',
        't'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(calculate_order_weight)], limit_size: 250, limit_class: 10 }
plugins/: No such file or directory (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

Not sure where `No such file or directory (os error 2)` comes from, because there are files and grep is also able to search them:
```sh
$ ls -lisa plugins/
total 108
4486924 4 drwxr-xr-x 26 www-data www-data 4096 Apr 17 09:19 .
4471937 4 drwxr-xr-x  7 www-data www-data 4096 Apr 17 12:29 ..
4489631 4 drwxr-xr-x  4 www-data www-data 4096 Apr 17 08:33 akismet
4489541 4 drwxr-xr-x  6 www-data www-data 4096 Apr 17 08:33 black-studio-tinymce-widget
4490432 4 drwxr-xr-x  8 www-data www-data 4096 Apr 17 08:33 dhl-for-woocommerce
4491712 4 drwxr-xr-x  4 www-data www-data 4096 Apr 17 08:33 google-sitemap-generator
4489419 4 drwxr-xr-x  8 www-data www-data 4096 Apr 17 08:33 groups
4486925 4 -rw-r--r--  1 www-data www-data   30 Apr 17 08:33 index.php
4490752 4 drwxr-xr-x  7 www-data www-data 4096 Apr 17 08:33 kadence-woo-extras
4492185 4 drwxr-xr-x  8 www-data www-data 4096 Apr 17 08:33 mailchimp-for-wp
4490175 4 drwxr-xr-x  4 www-data www-data 4096 Apr 17 08:33 paymill
4492120 4 drwxr-xr-x  2 www-data www-data 4096 Apr 17 08:33 pll-merge-comments
4488422 4 drwxr-xr-x 12 www-data www-data 4096 Apr 17 08:33 polylang-pro
4487303 4 drwxr-xr-x  9 www-data www-data 4096 Apr 17 08:33 polylang-wc
4492122 4 drwxr-xr-x  8 www-data www-data 4096 Apr 17 08:33 shariff
4489978 4 drwxr-xr-x 11 www-data www-data 4096 Apr 17 08:33 siteorigin-panels
4492477 4 drwxr-xr-x  2 www-data www-data 4096 Apr 17 08:33 varnish-http-purge
4487356 4 drwxr-xr-x  7 www-data www-data 4096 Apr 17 08:33 woocommerce
4490677 4 drwxr-xr-x  5 www-data www-data 4096 Apr 17 08:33 woocommerce-gateway-stripe
4486926 4 drwxr-xr-x  7 www-data www-data 4096 Apr 17 08:33 woocommerce-germanized
4491799 4 drwxr-xr-x  5 www-data www-data 4096 Apr 17 08:33 woocommerce-google-analytics-integration
4488856 4 drwxr-xr-x  7 www-data www-data 4096 Apr 17 08:33 woocommerce-pdf-invoices-packing-slips
4491814 4 drwxr-xr-x  9 www-data www-data 4096 Apr 17 08:33 woocommerce-pdf-ips-pro
4489655 4 drwxr-xr-x  7 www-data www-data 4096 Apr 17 08:33 woocommerce-subscriptions
4490641 4 drwxr-xr-x  6 www-data www-data 4096 Apr 17 08:33 woothemes-updater
4487290 4 drwxr-xr-x  6 www-data www-data 4096 Apr 17 08:33 wpovernight-sidekick
4492487 4 drwxr-xr-x  6 www-data www-data 4096 Apr 17 08:33 wpseo

$ ls -ld plugins/
drwxr-xr-x 26 www-data www-data 4096 Apr 17 09:19 plugins/

```

#### If this is a bug, what is the expected behavior? What do you think ripgrep should have done?

I do not understand why `ripgrep` is not able to search through the files, whereas `grep` is. Maybe also the error message could indicate what I as user should do different, because I do not know what to do instead...

---

_Comment by @BurntSushi on 2018-04-17 10:41_

> Ubuntu 16.04. rg is installed using snap as outlined in the README here.

Please show the exact command you use. In particular, did you install with the `--classic` flag?

---

_Comment by @gerdneuman on 2018-04-17 10:49_

I used `sudo snap install rg`.

---

_Comment by @BurntSushi on 2018-04-17 10:49_

............. Please try with the `--classic` flag.

---

_Comment by @gerdneuman on 2018-04-17 10:53_

Just did, after seeing your commit (sorry, that I did not see the permissions note in the README beforehand). But all the same:
```
$ sudo snap remove rg
[sudo] password for x: 
rg removed
$ sudo snap install --classic rg
rg 0.8.1 from 'tasdomas' installed
$ rg --debug calculate_order_weight plugins/
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'c',
        'a',
        'l',
        'c',
        'u',
        'l',
        'a',
        't',
        'e',
        '_',
        'o',
        'r',
        'd',
        'e',
        'r',
        '_',
        'w',
        'e',
        'i',
        'g',
        'h',
        't'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(calculate_order_weight)], limit_size: 250, limit_class: 10 }
plugins/: No such file or directory (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

---

_Comment by @BurntSushi on 2018-04-17 11:03_

Then I don't know what's wrong. The error you're getting is very clear: ripgrep believes that there is no `plugins/` path. There's nothing else it can do and there is no possible better error message. Given that I've been consistently surprised by the behavior of `snap` installed packages, I would be surprised if the issue was still because of `snap`, but I cannot begin to speculate as to why.

You might consider trying a binary Linux x86_64 release instead and see if that fixes things: https://github.com/BurntSushi/ripgrep/releases --- If it does, then there is something wrong either with `snap` or some other aspect of your environment.

If a binary release yields the same error, then I'm pretty well stumped. At that point, I'd probably request that you run ripgrep under strace and report back. e.g.,

```
$ strace rg calculate_order_weight plugins/ 2> strace.log
```

and then gist the contents of `strace.log`.

---

_Comment by @gerdneuman on 2018-04-17 12:17_

It works with ripgrep-0.8.1-x86_64-unknown-linux-musl.tar.gz and it also works with ripgrep_0.8.1_amd64.deb (which I now use). Does not work `sudo snap install --classic rg`

So closing because it seems to be a snap issue.

PS Since I already have your full attention, I'd like to let you know that I discovered `xsv` some weeks ago and have since used it for joining different CSV tables. Best tool I've discovered for years, same league like `sed`, `find` and the like, absolutely filling a gap! `xsv` is so awesome and so powerful for piping results in and out. And now I see, that you're the same maintainer. Wow! Let's see what `ripgrep` can bring to my life now that it is working :)

---

_Closed by @gerdneuman on 2018-04-17 12:17_

---

_Comment by @BurntSushi on 2018-04-17 12:36_

@gerdneuman Thanks for the kind words, and glad you got it working. :-)

I am very close to removing the `snap` install instructions from the README. It has caused a large amount of frustration, and I don't have time to wade through its weeds.

---
