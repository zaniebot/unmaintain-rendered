```yaml
number: 6554
title: "[`flake8-pytest-style`] Implement `pytest-unittest-raises-assertion` (`PT027`)"
type: pull_request
state: merged
author: harupy
labels:
  - rule
assignees: []
merged: true
base: main
head: PT027
created_at: 2023-08-14T09:09:43Z
updated_at: 2023-08-14T22:29:14Z
url: https://github.com/astral-sh/ruff/pull/6554
synced_at: 2026-01-12T02:52:04Z
```

# [`flake8-pytest-style`] Implement `pytest-unittest-raises-assertion` (`PT027`)

---

_Pull request opened by @harupy on 2023-08-14 09:09_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #6545

## Test Plan

<!-- How was it tested? -->

Unit tests


---

_Comment by @github-actions[bot] on 2023-08-14 09:30_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+436, -0, 0 error(s))

<details><summary>zulip (+436, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/analytics/tests/test_counts.py#L1406'>analytics/tests/test_counts.py:1406:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/analytics/tests/test_counts.py#L1418'>analytics/tests/test_counts.py:1418:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/analytics/tests/test_counts.py#L290'>analytics/tests/test_counts.py:290:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/analytics/tests/test_counts.py#L292'>analytics/tests/test_counts.py:292:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L1901'>corporate/tests/test_stripe.py:1901:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L2459'>corporate/tests/test_stripe.py:2459:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L2618'>corporate/tests/test_stripe.py:2618:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L3254'>corporate/tests/test_stripe.py:3254:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L3783'>corporate/tests/test_stripe.py:3783:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L3794'>corporate/tests/test_stripe.py:3794:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4284'>corporate/tests/test_stripe.py:4284:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4375'>corporate/tests/test_stripe.py:4375:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4378'>corporate/tests/test_stripe.py:4378:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4704'>corporate/tests/test_stripe.py:4704:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4715'>corporate/tests/test_stripe.py:4715:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4745'>corporate/tests/test_stripe.py:4745:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4779'>corporate/tests/test_stripe.py:4779:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L4788'>corporate/tests/test_stripe.py:4788:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L661'>corporate/tests/test_stripe.py:661:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/corporate/tests/test_stripe.py#L677'>corporate/tests/test_stripe.py:677:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/tools/tests/test_template_parser.py#L24'>tools/tests/test_template_parser.py:24:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L351'>zerver/tests/test_auth_backends.py:351:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L397'>zerver/tests/test_auth_backends.py:397:63:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L5776'>zerver/tests/test_auth_backends.py:5776:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L5804'>zerver/tests/test_auth_backends.py:5804:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L5812'>zerver/tests/test_auth_backends.py:5812:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6142'>zerver/tests/test_auth_backends.py:6142:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6156'>zerver/tests/test_auth_backends.py:6156:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6168'>zerver/tests/test_auth_backends.py:6168:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6187'>zerver/tests/test_auth_backends.py:6187:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6194'>zerver/tests/test_auth_backends.py:6194:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6198'>zerver/tests/test_auth_backends.py:6198:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6211'>zerver/tests/test_auth_backends.py:6211:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6359'>zerver/tests/test_auth_backends.py:6359:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6365'>zerver/tests/test_auth_backends.py:6365:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6372'>zerver/tests/test_auth_backends.py:6372:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6415'>zerver/tests/test_auth_backends.py:6415:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6473'>zerver/tests/test_auth_backends.py:6473:62:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6664'>zerver/tests/test_auth_backends.py:6664:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6684'>zerver/tests/test_auth_backends.py:6684:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L684'>zerver/tests/test_auth_backends.py:684:26:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L686'>zerver/tests/test_auth_backends.py:686:26:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L6993'>zerver/tests/test_auth_backends.py:6993:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_auth_backends.py#L702'>zerver/tests/test_auth_backends.py:702:26:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_bots.py#L1746'>zerver/tests/test_bots.py:1746:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_bots.py#L1762'>zerver/tests/test_bots.py:1762:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_bots.py#L1775'>zerver/tests/test_bots.py:1775:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_bots.py#L1791'>zerver/tests/test_bots.py:1791:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L271'>zerver/tests/test_cache.py:271:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L41'>zerver/tests/test_cache.py:41:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L43'>zerver/tests/test_cache.py:43:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L45'>zerver/tests/test_cache.py:45:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L47'>zerver/tests/test_cache.py:47:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L49'>zerver/tests/test_cache.py:49:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L52'>zerver/tests/test_cache.py:52:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L62'>zerver/tests/test_cache.py:62:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L64'>zerver/tests/test_cache.py:64:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L66'>zerver/tests/test_cache.py:66:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L69'>zerver/tests/test_cache.py:69:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L71'>zerver/tests/test_cache.py:71:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_cache.py#L73'>zerver/tests/test_cache.py:73:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_custom_profile_data.py#L205'>zerver/tests/test_custom_profile_data.py:205:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_custom_profile_data.py#L979'>zerver/tests/test_custom_profile_data.py:979:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L1009'>zerver/tests/test_decorators.py:1009:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L1025'>zerver/tests/test_decorators.py:1025:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L1035'>zerver/tests/test_decorators.py:1035:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L1040'>zerver/tests/test_decorators.py:1040:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L1052'>zerver/tests/test_decorators.py:1052:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L1065'>zerver/tests/test_decorators.py:1065:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L157'>zerver/tests/test_decorators.py:157:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L164'>zerver/tests/test_decorators.py:164:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L180'>zerver/tests/test_decorators.py:180:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L202'>zerver/tests/test_decorators.py:202:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L214'>zerver/tests/test_decorators.py:214:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L226'>zerver/tests/test_decorators.py:226:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L244'>zerver/tests/test_decorators.py:244:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L274'>zerver/tests/test_decorators.py:274:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L284'>zerver/tests/test_decorators.py:284:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L388'>zerver/tests/test_decorators.py:388:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L414'>zerver/tests/test_decorators.py:414:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L437'>zerver/tests/test_decorators.py:437:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L896'>zerver/tests/test_decorators.py:896:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L902'>zerver/tests/test_decorators.py:902:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L905'>zerver/tests/test_decorators.py:905:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L914'>zerver/tests/test_decorators.py:914:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L920'>zerver/tests/test_decorators.py:920:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L951'>zerver/tests/test_decorators.py:951:22:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_decorators.py#L969'>zerver/tests/test_decorators.py:969:22:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_change.py#L294'>zerver/tests/test_email_change.py:294:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_change.py#L296'>zerver/tests/test_email_change.py:296:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_mirror.py#L112'>zerver/tests/test_email_mirror.py:112:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_mirror.py#L115'>zerver/tests/test_email_mirror.py:115:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_mirror.py#L129'>zerver/tests/test_email_mirror.py:129:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_mirror.py#L198'>zerver/tests/test_email_mirror.py:198:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_mirror.py#L207'>zerver/tests/test_email_mirror.py:207:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_notifications.py#L161'>zerver/tests/test_email_notifications.py:161:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_notifications.py#L172'>zerver/tests/test_email_notifications.py:172:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_notifications.py#L193'>zerver/tests/test_email_notifications.py:193:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_email_notifications.py#L204'>zerver/tests/test_email_notifications.py:204:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_event_system.py#L248'>zerver/tests/test_event_system.py:248:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_event_system.py#L256'>zerver/tests/test_event_system.py:256:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_event_system.py#L270'>zerver/tests/test_event_system.py:270:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_event_system.py#L503'>zerver/tests/test_event_system.py:503:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_event_system.py#L533'>zerver/tests/test_event_system.py:533:54:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_events.py#L2764'>zerver/tests/test_events.py:2764:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_example.py#L149'>zerver/tests/test_example.py:149:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_example.py#L291'>zerver/tests/test_example.py:291:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_external.py#L67'>zerver/tests/test_external.py:67:13:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_external.py#L71'>zerver/tests/test_external.py:71:13:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_github.py#L87'>zerver/tests/test_github.py:87:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L101'>zerver/tests/test_has_request_variables.py:101:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L106'>zerver/tests/test_has_request_variables.py:106:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L123'>zerver/tests/test_has_request_variables.py:123:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L127'>zerver/tests/test_has_request_variables.py:127:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L144'>zerver/tests/test_has_request_variables.py:144:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L150'>zerver/tests/test_has_request_variables.py:150:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L30'>zerver/tests/test_has_request_variables.py:30:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L43'>zerver/tests/test_has_request_variables.py:43:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L64'>zerver/tests/test_has_request_variables.py:64:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L68'>zerver/tests/test_has_request_variables.py:68:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L73'>zerver/tests/test_has_request_variables.py:73:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L80'>zerver/tests/test_has_request_variables.py:80:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_has_request_variables.py#L97'>zerver/tests/test_has_request_variables.py:97:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_import_export.py#L1334'>zerver/tests/test_import_export.py:1334:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_import_export.py#L1350'>zerver/tests/test_import_export.py:1350:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_internet.py#L10'>zerver/tests/test_internet.py:10:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_invite.py#L1112'>zerver/tests/test_invite.py:1112:13:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_invite.py#L1414'>zerver/tests/test_invite.py:1414:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_invite.py#L1540'>zerver/tests/test_invite.py:1540:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_invite.py#L1879'>zerver/tests/test_invite.py:1879:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_invite.py#L1917'>zerver/tests/test_invite.py:1917:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_invite.py#L1944'>zerver/tests/test_invite.py:1944:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_invite.py#L2403'>zerver/tests/test_invite.py:2403:12:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L128'>zerver/tests/test_management_commands.py:128:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L151'>zerver/tests/test_management_commands.py:151:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L185'>zerver/tests/test_management_commands.py:185:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L189'>zerver/tests/test_management_commands.py:189:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L217'>zerver/tests/test_management_commands.py:217:26:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L236'>zerver/tests/test_management_commands.py:236:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L245'>zerver/tests/test_management_commands.py:245:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L256'>zerver/tests/test_management_commands.py:256:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L278'>zerver/tests/test_management_commands.py:278:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L301'>zerver/tests/test_management_commands.py:301:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L38'>zerver/tests/test_management_commands.py:38:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L409'>zerver/tests/test_management_commands.py:409:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L45'>zerver/tests/test_management_commands.py:45:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L562'>zerver/tests/test_management_commands.py:562:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L575'>zerver/tests/test_management_commands.py:575:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L591'>zerver/tests/test_management_commands.py:591:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L64'>zerver/tests/test_management_commands.py:64:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L66'>zerver/tests/test_management_commands.py:66:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L78'>zerver/tests/test_management_commands.py:78:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L81'>zerver/tests/test_management_commands.py:81:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_management_commands.py#L86'>zerver/tests/test_management_commands.py:86:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_markdown.py#L2915'>zerver/tests/test_markdown.py:2915:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_markdown.py#L2923'>zerver/tests/test_markdown.py:2923:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_markdown.py#L2935'>zerver/tests/test_markdown.py:2935:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_markdown.py#L2950'>zerver/tests/test_markdown.py:2950:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_markdown.py#L978'>zerver/tests/test_markdown.py:978:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_edit.py#L3809'>zerver/tests/test_message_edit.py:3809:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L115'>zerver/tests/test_message_fetch.py:115:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L129'>zerver/tests/test_message_fetch.py:129:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L135'>zerver/tests/test_message_fetch.py:135:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L267'>zerver/tests/test_message_fetch.py:267:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L297'>zerver/tests/test_message_fetch.py:297:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L365'>zerver/tests/test_message_fetch.py:365:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L371'>zerver/tests/test_message_fetch.py:371:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L415'>zerver/tests/test_message_fetch.py:415:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L418'>zerver/tests/test_message_fetch.py:418:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L478'>zerver/tests/test_message_fetch.py:478:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L506'>zerver/tests/test_message_fetch.py:506:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L521'>zerver/tests/test_message_fetch.py:521:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L549'>zerver/tests/test_message_fetch.py:549:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L563'>zerver/tests/test_message_fetch.py:563:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_fetch.py#L831'>zerver/tests/test_message_fetch.py:831:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_flags.py#L1494'>zerver/tests/test_message_flags.py:1494:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L1622'>zerver/tests/test_message_send.py:1622:22:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L1721'>zerver/tests/test_message_send.py:1721:22:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L1816'>zerver/tests/test_message_send.py:1816:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L1843'>zerver/tests/test_message_send.py:1843:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L1870'>zerver/tests/test_message_send.py:1870:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L1885'>zerver/tests/test_message_send.py:1885:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L1891'>zerver/tests/test_message_send.py:1891:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2063'>zerver/tests/test_message_send.py:2063:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2117'>zerver/tests/test_message_send.py:2117:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2120'>zerver/tests/test_message_send.py:2120:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2123'>zerver/tests/test_message_send.py:2123:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2154'>zerver/tests/test_message_send.py:2154:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2158'>zerver/tests/test_message_send.py:2158:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2166'>zerver/tests/test_message_send.py:2166:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2179'>zerver/tests/test_message_send.py:2179:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2184'>zerver/tests/test_message_send.py:2184:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2342'>zerver/tests/test_message_send.py:2342:20:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2459'>zerver/tests/test_message_send.py:2459:20:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2521'>zerver/tests/test_message_send.py:2521:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2548'>zerver/tests/test_message_send.py:2548:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2557'>zerver/tests/test_message_send.py:2557:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2588'>zerver/tests/test_message_send.py:2588:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2597'>zerver/tests/test_message_send.py:2597:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2612'>zerver/tests/test_message_send.py:2612:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2666'>zerver/tests/test_message_send.py:2666:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L2727'>zerver/tests/test_message_send.py:2727:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_message_send.py#L77'>zerver/tests/test_message_send.py:77:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_mirror_users.py#L25'>zerver/tests/test_mirror_users.py:25:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_mirror_users.py#L43'>zerver/tests/test_mirror_users.py:43:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L1004'>zerver/tests/test_openapi.py:1004:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L110'>zerver/tests/test_openapi.py:110:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L119'>zerver/tests/test_openapi.py:119:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L160'>zerver/tests/test_openapi.py:160:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L175'>zerver/tests/test_openapi.py:175:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L780'>zerver/tests/test_openapi.py:780:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L782'>zerver/tests/test_openapi.py:782:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L812'>zerver/tests/test_openapi.py:812:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L851'>zerver/tests/test_openapi.py:851:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L859'>zerver/tests/test_openapi.py:859:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_openapi.py#L98'>zerver/tests/test_openapi.py:98:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_outgoing_webhook_interfaces.py#L58'>zerver/tests/test_outgoing_webhook_interfaces.py:58:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_outgoing_webhook_system.py#L150'>zerver/tests/test_outgoing_webhook_system.py:150:72:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L1240'>zerver/tests/test_push_notifications.py:1240:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L2544'>zerver/tests/test_push_notifications.py:2544:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L2551'>zerver/tests/test_push_notifications.py:2551:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L2562'>zerver/tests/test_push_notifications.py:2562:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L2573'>zerver/tests/test_push_notifications.py:2573:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L2579'>zerver/tests/test_push_notifications.py:2579:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L2713'>zerver/tests/test_push_notifications.py:2713:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_push_notifications.py#L2717'>zerver/tests/test_push_notifications.py:2717:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_queue_worker.py#L791'>zerver/tests/test_queue_worker.py:791:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_reactions.py#L207'>zerver/tests/test_reactions.py:207:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_reactions.py#L233'>zerver/tests/test_reactions.py:233:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm.py#L121'>zerver/tests/test_realm.py:121:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm.py#L124'>zerver/tests/test_realm.py:124:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm.py#L294'>zerver/tests/test_realm.py:294:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm.py#L531'>zerver/tests/test_realm.py:531:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm.py#L78'>zerver/tests/test_realm.py:78:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_domains.py#L133'>zerver/tests/test_realm_domains.py:133:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_domains.py#L137'>zerver/tests/test_realm_domains.py:137:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_domains.py#L143'>zerver/tests/test_realm_domains.py:143:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_domains.py#L148'>zerver/tests/test_realm_domains.py:148:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_domains.py#L166'>zerver/tests/test_realm_domains.py:166:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_emoji.py#L394'>zerver/tests/test_realm_emoji.py:394:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_export.py#L100'>zerver/tests/test_realm_export.py:100:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_export.py#L291'>zerver/tests/test_realm_export.py:291:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_export.py#L38'>zerver/tests/test_realm_export.py:38:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_realm_linkifiers.py#L261'>zerver/tests/test_realm_linkifiers.py:261:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_redis_utils.py#L57'>zerver/tests/test_redis_utils.py:57:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_redis_utils.py#L68'>zerver/tests/test_redis_utils.py:68:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_redis_utils.py#L74'>zerver/tests/test_redis_utils.py:74:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_retention.py#L339'>zerver/tests/test_retention.py:339:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_retention.py#L648'>zerver/tests/test_retention.py:648:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_scheduled_messages.py#L726'>zerver/tests/test_scheduled_messages.py:726:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_scheduled_messages.py#L729'>zerver/tests/test_scheduled_messages.py:729:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_scheduled_messages.py#L732'>zerver/tests/test_scheduled_messages.py:732:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_scheduled_messages.py#L748'>zerver/tests/test_scheduled_messages.py:748:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_scheduled_messages.py#L752'>zerver/tests/test_scheduled_messages.py:752:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_scheduled_messages.py#L757'>zerver/tests/test_scheduled_messages.py:757:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_send_email.py#L102'>zerver/tests/test_send_email.py:102:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_send_email.py#L136'>zerver/tests/test_send_email.py:136:26:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_server_settings.py#L14'>zerver/tests/test_server_settings.py:14:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_service_bot_system.py#L210'>zerver/tests/test_service_bot_system.py:210:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_service_bot_system.py#L253'>zerver/tests/test_service_bot_system.py:253:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_service_bot_system.py#L364'>zerver/tests/test_service_bot_system.py:364:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_service_bot_system.py#L411'>zerver/tests/test_service_bot_system.py:411:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_settings.py#L526'>zerver/tests/test_settings.py:526:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1243'>zerver/tests/test_signup.py:1243:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1381'>zerver/tests/test_signup.py:1381:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1427'>zerver/tests/test_signup.py:1427:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1476'>zerver/tests/test_signup.py:1476:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1536'>zerver/tests/test_signup.py:1536:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1680'>zerver/tests/test_signup.py:1680:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1734'>zerver/tests/test_signup.py:1734:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1736'>zerver/tests/test_signup.py:1736:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1967'>zerver/tests/test_signup.py:1967:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1969'>zerver/tests/test_signup.py:1969:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1973'>zerver/tests/test_signup.py:1973:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1975'>zerver/tests/test_signup.py:1975:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1980'>zerver/tests/test_signup.py:1980:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1982'>zerver/tests/test_signup.py:1982:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1986'>zerver/tests/test_signup.py:1986:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L1994'>zerver/tests/test_signup.py:1994:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L2408'>zerver/tests/test_signup.py:2408:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L3056'>zerver/tests/test_signup.py:3056:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L3136'>zerver/tests/test_signup.py:3136:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L3834'>zerver/tests/test_signup.py:3834:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L3888'>zerver/tests/test_signup.py:3888:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L4252'>zerver/tests/test_signup.py:4252:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L957'>zerver/tests/test_signup.py:957:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_signup.py#L989'>zerver/tests/test_signup.py:989:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L136'>zerver/tests/test_slack_importer.py:136:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L140'>zerver/tests/test_slack_importer.py:140:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L147'>zerver/tests/test_slack_importer.py:147:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L151'>zerver/tests/test_slack_importer.py:151:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L156'>zerver/tests/test_slack_importer.py:156:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L160'>zerver/tests/test_slack_importer.py:160:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L167'>zerver/tests/test_slack_importer.py:167:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_slack_importer.py#L228'>zerver/tests/test_slack_importer.py:228:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L1469'>zerver/tests/test_subs.py:1469:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L1476'>zerver/tests/test_subs.py:1476:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L1647'>zerver/tests/test_subs.py:1647:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L1678'>zerver/tests/test_subs.py:1678:9:</a> PT027 Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L2366'>zerver/tests/test_subs.py:2366:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L2376'>zerver/tests/test_subs.py:2376:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L2412'>zerver/tests/test_subs.py:2412:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L2473'>zerver/tests/test_subs.py:2473:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L3067'>zerver/tests/test_subs.py:3067:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L3316'>zerver/tests/test_subs.py:3316:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L3960'>zerver/tests/test_subs.py:3960:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L5164'>zerver/tests/test_subs.py:5164:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L5181'>zerver/tests/test_subs.py:5181:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L5532'>zerver/tests/test_subs.py:5532:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L5536'>zerver/tests/test_subs.py:5536:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L5807'>zerver/tests/test_subs.py:5807:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6411'>zerver/tests/test_subs.py:6411:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6413'>zerver/tests/test_subs.py:6413:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6426'>zerver/tests/test_subs.py:6426:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6428'>zerver/tests/test_subs.py:6428:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6445'>zerver/tests/test_subs.py:6445:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6447'>zerver/tests/test_subs.py:6447:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6449'>zerver/tests/test_subs.py:6449:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6451'>zerver/tests/test_subs.py:6451:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6455'>zerver/tests/test_subs.py:6455:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6457'>zerver/tests/test_subs.py:6457:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6472'>zerver/tests/test_subs.py:6472:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L6485'>zerver/tests/test_subs.py:6485:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L783'>zerver/tests/test_subs.py:783:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_subs.py#L793'>zerver/tests/test_subs.py:793:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_templates.py#L125'>zerver/tests/test_templates.py:125:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_templates.py#L99'>zerver/tests/test_templates.py:99:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_timestamp.py#L31'>zerver/tests/test_timestamp.py:31:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_timestamp.py#L46'>zerver/tests/test_timestamp.py:46:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_upload.py#L1350'>zerver/tests/test_upload.py:1350:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_upload_s3.py#L121'>zerver/tests/test_upload_s3.py:121:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_upload_s3.py#L142'>zerver/tests/test_upload_s3.py:142:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_upload_s3.py#L148'>zerver/tests/test_upload_s3.py:148:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_upload_s3.py#L397'>zerver/tests/test_upload_s3.py:397:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_upload_s3.py#L399'>zerver/tests/test_upload_s3.py:399:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_upload_s3.py#L401'>zerver/tests/test_upload_s3.py:401:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1045'>zerver/tests/test_users.py:1045:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1047'>zerver/tests/test_users.py:1047:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1051'>zerver/tests/test_users.py:1051:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1055'>zerver/tests/test_users.py:1055:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1059'>zerver/tests/test_users.py:1059:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1064'>zerver/tests/test_users.py:1064:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1069'>zerver/tests/test_users.py:1069:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1103'>zerver/tests/test_users.py:1103:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1105'>zerver/tests/test_users.py:1105:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1149'>zerver/tests/test_users.py:1149:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L1298'>zerver/tests/test_users.py:1298:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L2185'>zerver/tests/test_users.py:2185:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L2613'>zerver/tests/test_users.py:2613:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L2618'>zerver/tests/test_users.py:2618:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L329'>zerver/tests/test_users.py:329:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L433'>zerver/tests/test_users.py:433:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L435'>zerver/tests/test_users.py:435:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L441'>zerver/tests/test_users.py:441:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L447'>zerver/tests/test_users.py:447:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L449'>zerver/tests/test_users.py:449:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_users.py#L454'>zerver/tests/test_users.py:454:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L101'>zerver/tests/test_validators.py:101:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L105'>zerver/tests/test_validators.py:105:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L113'>zerver/tests/test_validators.py:113:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L121'>zerver/tests/test_validators.py:121:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L126'>zerver/tests/test_validators.py:126:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L128'>zerver/tests/test_validators.py:128:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L130'>zerver/tests/test_validators.py:130:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L138'>zerver/tests/test_validators.py:138:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L142'>zerver/tests/test_validators.py:142:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L154'>zerver/tests/test_validators.py:154:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L157'>zerver/tests/test_validators.py:157:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L162'>zerver/tests/test_validators.py:162:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L166'>zerver/tests/test_validators.py:166:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L170'>zerver/tests/test_validators.py:170:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L174'>zerver/tests/test_validators.py:174:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L190'>zerver/tests/test_validators.py:190:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L194'>zerver/tests/test_validators.py:194:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L200'>zerver/tests/test_validators.py:200:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L207'>zerver/tests/test_validators.py:207:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L214'>zerver/tests/test_validators.py:214:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L234'>zerver/tests/test_validators.py:234:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L251'>zerver/tests/test_validators.py:251:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L261'>zerver/tests/test_validators.py:261:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L283'>zerver/tests/test_validators.py:283:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L294'>zerver/tests/test_validators.py:294:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L300'>zerver/tests/test_validators.py:300:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L309'>zerver/tests/test_validators.py:309:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L320'>zerver/tests/test_validators.py:320:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L324'>zerver/tests/test_validators.py:324:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L335'>zerver/tests/test_validators.py:335:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L339'>zerver/tests/test_validators.py:339:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L350'>zerver/tests/test_validators.py:350:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L376'>zerver/tests/test_validators.py:376:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L378'>zerver/tests/test_validators.py:378:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L380'>zerver/tests/test_validators.py:380:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L382'>zerver/tests/test_validators.py:382:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L384'>zerver/tests/test_validators.py:384:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L386'>zerver/tests/test_validators.py:386:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L388'>zerver/tests/test_validators.py:388:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L390'>zerver/tests/test_validators.py:390:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L392'>zerver/tests/test_validators.py:392:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L394'>zerver/tests/test_validators.py:394:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L396'>zerver/tests/test_validators.py:396:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L398'>zerver/tests/test_validators.py:398:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L400'>zerver/tests/test_validators.py:400:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L402'>zerver/tests/test_validators.py:402:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L404'>zerver/tests/test_validators.py:404:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L407'>zerver/tests/test_validators.py:407:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L47'>zerver/tests/test_validators.py:47:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L55'>zerver/tests/test_validators.py:55:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L59'>zerver/tests/test_validators.py:59:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L63'>zerver/tests/test_validators.py:63:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L71'>zerver/tests/test_validators.py:71:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L75'>zerver/tests/test_validators.py:75:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L83'>zerver/tests/test_validators.py:83:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L86'>zerver/tests/test_validators.py:86:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L91'>zerver/tests/test_validators.py:91:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_validators.py#L93'>zerver/tests/test_validators.py:93:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_webhooks_common.py#L49'>zerver/tests/test_webhooks_common.py:49:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_webhooks_common.py#L82'>zerver/tests/test_webhooks_common.py:82:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_webhooks_common.py#L95'>zerver/tests/test_webhooks_common.py:95:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/tests/test_widgets.py#L19'>zerver/tests/test_widgets.py:19:18:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/jira/tests.py#L239'>zerver/webhooks/jira/tests.py:239:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/librato/tests.py#L73'>zerver/webhooks/librato/tests.py:73:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/librato/tests.py#L83'>zerver/webhooks/librato/tests.py:83:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L110'>zerver/webhooks/newrelic/tests.py:110:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L120'>zerver/webhooks/newrelic/tests.py:120:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L189'>zerver/webhooks/newrelic/tests.py:189:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L218'>zerver/webhooks/newrelic/tests.py:218:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L231'>zerver/webhooks/newrelic/tests.py:231:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L243'>zerver/webhooks/newrelic/tests.py:243:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L253'>zerver/webhooks/newrelic/tests.py:253:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L56'>zerver/webhooks/newrelic/tests.py:56:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L85'>zerver/webhooks/newrelic/tests.py:85:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/newrelic/tests.py#L98'>zerver/webhooks/newrelic/tests.py:98:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/papertrail/tests.py#L65'>zerver/webhooks/papertrail/tests.py:65:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/pivotal/tests.py#L205'>zerver/webhooks/pivotal/tests.py:205:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/pivotal/tests.py#L221'>zerver/webhooks/pivotal/tests.py:221:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaisesRegex`
+ <a href='https://github.com/zulip/zulip/blob/d205850d54f73d648488bc1dbd37cca2dfdef272/zerver/webhooks/travis/tests.py#L124'>zerver/webhooks/travis/tests.py:124:14:</a> PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PT027 | 436 | 436 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.02ms    10.4 MB/sec    1.00      3.9±0.03ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.00    774.8±8.92µs    21.5 MB/sec    1.00    776.8±4.50µs    21.4 MB/sec
formatter/numpy/globals.py                 1.00     78.5±0.36µs    37.6 MB/sec    1.00     78.8±0.29µs    37.5 MB/sec
formatter/pydantic/types.py                1.00  1561.1±31.90µs    16.3 MB/sec    1.00  1562.9±16.78µs    16.3 MB/sec
linter/all-rules/large/dataset.py          1.02     10.4±0.07ms     3.9 MB/sec    1.00     10.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    393.0±2.12µs     7.5 MB/sec    1.01    397.4±4.56µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      5.6±0.13ms     4.6 MB/sec    1.00      5.4±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.5±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1194.7±10.86µs    13.9 MB/sec    1.01   1205.2±8.32µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.9±2.48µs    20.9 MB/sec    1.04    146.3±6.13µs    20.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.4 MB/sec    1.00      2.5±0.02ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.2±0.06ms     9.7 MB/sec    1.00      4.2±0.06ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   824.0±13.20µs    20.2 MB/sec    1.00   822.4±11.10µs    20.2 MB/sec
formatter/numpy/globals.py                 1.00     84.0±0.95µs    35.1 MB/sec    1.01     85.0±2.26µs    34.7 MB/sec
formatter/pydantic/types.py                1.00  1679.9±32.97µs    15.2 MB/sec    1.00  1687.0±25.68µs    15.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.10ms     3.2 MB/sec    1.00     12.6±0.10ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.9±5.25µs     6.8 MB/sec    1.00    436.7±6.32µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.08ms     3.9 MB/sec    1.00      6.5±0.12ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.08ms     5.8 MB/sec    1.01      7.0±0.08ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1470.5±16.23µs    11.3 MB/sec    1.01  1480.4±17.30µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.5±5.41µs    17.2 MB/sec    1.00    170.7±2.56µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.03ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @harupy on 2023-08-14 11:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-14 20:10_

---

_Label `rule` added by @charliermarsh on 2023-08-14 20:16_

---

_Renamed from "Implement PT027" to "[`flake8-pytest-style`] Implement `pytest-unittest-raises-assertion` (`PT027`)" by @charliermarsh on 2023-08-14 20:16_

---

_Comment by @charliermarsh on 2023-08-14 20:21_

(The Zulip changes are expected. They don't use pytest, but we run with `ALL` on the ecosystem CI.)

---

_Merged by @charliermarsh on 2023-08-14 20:25_

---

_Closed by @charliermarsh on 2023-08-14 20:25_

---

_Branch deleted on 2023-08-14 22:29_

---
