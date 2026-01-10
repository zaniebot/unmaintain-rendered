---
number: 10795
title: Ruff 0.3.5 not respecting directive for all lines of a multiline expression
type: issue
state: closed
author: BenZickel
labels: []
assignees: []
created_at: 2024-04-05T22:40:34Z
updated_at: 2024-04-06T14:51:19Z
url: https://github.com/astral-sh/ruff/issues/10795
synced_at: 2026-01-10T01:22:50Z
---

# Ruff 0.3.5 not respecting directive for all lines of a multiline expression

---

_Issue opened by @BenZickel on 2024-04-05 22:40_

The bug occurs when switching from ruff 0.3.4 to 0.3.5 with the directive `# noga: F822` not respected for all lines of a multiline expression.

The problem is described in [this](https://github.com/pyro-ppl/pyro/pull/3354) issue in the Pyro repo, mainly:
- Linting with ruff version 0.3.5 fails as can be seen [here](https://github.com/pyro-ppl/pyro/actions/runs/8572360312/job/23494720392?pr=3352#step:5:16).
- The failure is due to ruff version 0.3.5 not respecting [this](https://github.com/pyro-ppl/pyro/blob/58080f81b662bd9575cdf4b466ab3d87236c95df/pyro/distributions/torch.py#L350) directive.
- The problem does not occur with ruff version 0.3.4.

The command that is executed is `ruff check .` and is part of the CI workflow of the Pyro repo.


---

_Comment by @charliermarsh on 2024-04-05 22:43_

Hmm... I don't think we respected `# noqa` in that position in 0.3.4. Is it possible that we weren't raising diagnostics there _at all_ in 0.3.4? I'll test it out.

---

_Comment by @charliermarsh on 2024-04-05 22:45_

Nevermind, I stand corrected! I'll take a look at what changed...

---

_Comment by @charliermarsh on 2024-04-05 22:46_

Ah ok, the change is that we fixed the ranges on those diagnostics to be on the actual strings. So, previously, they all said line 350:

```
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Bernoulli` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Cauchy` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Chi2` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `ContinuousBernoulli` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `ExponentialFamily` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Exponential` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `FisherSnedecor` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Gumbel` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `HalfCauchy` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `HalfNormal` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Kumaraswamy` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Laplace` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `LKJCholesky` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `LogisticNormal` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `MixtureSameFamily` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `NegativeBinomial` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `OneHotCategoricalStraightThrough` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Pareto` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `RelaxedBernoulli` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `RelaxedOneHotCategorical` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `StudentT` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `TransformedDistribution` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `VonMises` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Weibull` in `__all__`
pyro/pyro/distributions/torch.py:350:1: F822 Undefined name `Wishart` in `__all__`
```

Now, we have the correct lines and columns:

```
pyro/pyro/distributions/torch.py:351:5: F822 Undefined name `Bernoulli` in `__all__`
pyro/pyro/distributions/torch.py:355:5: F822 Undefined name `Cauchy` in `__all__`
pyro/pyro/distributions/torch.py:356:5: F822 Undefined name `Chi2` in `__all__`
pyro/pyro/distributions/torch.py:357:5: F822 Undefined name `ContinuousBernoulli` in `__all__`
pyro/pyro/distributions/torch.py:359:5: F822 Undefined name `ExponentialFamily` in `__all__`
pyro/pyro/distributions/torch.py:360:5: F822 Undefined name `Exponential` in `__all__`
pyro/pyro/distributions/torch.py:361:5: F822 Undefined name `FisherSnedecor` in `__all__`
pyro/pyro/distributions/torch.py:364:5: F822 Undefined name `Gumbel` in `__all__`
pyro/pyro/distributions/torch.py:365:5: F822 Undefined name `HalfCauchy` in `__all__`
pyro/pyro/distributions/torch.py:366:5: F822 Undefined name `HalfNormal` in `__all__`
pyro/pyro/distributions/torch.py:368:5: F822 Undefined name `Kumaraswamy` in `__all__`
pyro/pyro/distributions/torch.py:369:5: F822 Undefined name `Laplace` in `__all__`
pyro/pyro/distributions/torch.py:370:5: F822 Undefined name `LKJCholesky` in `__all__`
pyro/pyro/distributions/torch.py:372:5: F822 Undefined name `LogisticNormal` in `__all__`
pyro/pyro/distributions/torch.py:374:5: F822 Undefined name `MixtureSameFamily` in `__all__`
pyro/pyro/distributions/torch.py:377:5: F822 Undefined name `NegativeBinomial` in `__all__`
pyro/pyro/distributions/torch.py:380:5: F822 Undefined name `OneHotCategoricalStraightThrough` in `__all__`
pyro/pyro/distributions/torch.py:381:5: F822 Undefined name `Pareto` in `__all__`
pyro/pyro/distributions/torch.py:383:5: F822 Undefined name `RelaxedBernoulli` in `__all__`
pyro/pyro/distributions/torch.py:384:5: F822 Undefined name `RelaxedOneHotCategorical` in `__all__`
pyro/pyro/distributions/torch.py:385:5: F822 Undefined name `StudentT` in `__all__`
pyro/pyro/distributions/torch.py:386:5: F822 Undefined name `TransformedDistribution` in `__all__`
pyro/pyro/distributions/torch.py:388:5: F822 Undefined name `VonMises` in `__all__`
pyro/pyro/distributions/torch.py:389:5: F822 Undefined name `Weibull` in `__all__`
pyro/pyro/distributions/torch.py:390:5: F822 Undefined name `Wishart` in `__all__`
```

But now your `# noqa` no longer works, since it's on the "wrong" line.

---

_Comment by @BenZickel on 2024-04-05 22:58_

Thanks for the quick reply @charliermarsh!
So it was never the intention to support applying a single directive to all lines of a multiline expression? 

---

_Comment by @charliermarsh on 2024-04-05 23:25_

It wasn't the intention, but we can support it I think.

---

_Referenced in [astral-sh/ruff#10798](../../astral-sh/ruff/pulls/10798.md) on 2024-04-05 23:41_

---

_Comment by @charliermarsh on 2024-04-05 23:41_

Will be fixed in the next release: https://github.com/astral-sh/ruff/pull/10798. Thanks, and sorry for the disruption.

---

_Closed by @charliermarsh on 2024-04-06 14:51_

---

_Referenced in [pyro-ppl/pyro#3357](../../pyro-ppl/pyro/pulls/3357.md) on 2024-04-12 13:28_

---
