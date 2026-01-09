---
number: 15798
title: "Request: More unsafe fixes for `custom-type-var-return-type`/`PYI019`"
type: issue
state: closed
author: Avasam
labels:
  - fixes
assignees: []
created_at: 2025-01-28T23:55:20Z
updated_at: 2025-02-02T18:38:51Z
url: https://github.com/astral-sh/ruff/issues/15798
synced_at: 2026-01-07T13:12:16-06:00
---

# Request: More unsafe fixes for `custom-type-var-return-type`/`PYI019`

---

_Issue opened by @Avasam on 2025-01-28 23:55_

### Description

This is a direct follow-up to https://github.com/astral-sh/ruff/issues/14183, which was closed by #14238 but there are still cases that I think can be autofixed. Namely, when other method params than `self`/`cls` are typed.

For example, as of Ruff 0.9.3:

This is currently autofixed
```py
class _BaseHeterogeneousEnsemble(MetaEstimatorMixin, _BaseComposition, metaclass=ABCMeta):
    def set_params(self: _BaseHeterogeneousEnsemble_Self, **params) -> _BaseHeterogeneousEnsemble_Self: ...
```

But these aren't:
```py
class _SigmoidCalibration(RegressorMixin, BaseEstimator):
    def fit(
        self: _SigmoidCalibration_Self,
        X: ArrayLike,
        y: ArrayLike,
        sample_weight: None | ArrayLike = None,
    ) -> _SigmoidCalibration_Self: ...


class _PLS(
    ClassNamePrefixFeaturesOutMixin,
    TransformerMixin,
    RegressorMixin,
    MultiOutputMixin,
    BaseEstimator,
    metaclass=ABCMeta,
):
    def fit(self: _PLS_Self, X: MatrixLike, Y: MatrixLike | ArrayLike) -> _PLS_Self: ...


class _BaseNMF(ClassNamePrefixFeaturesOutMixin, TransformerMixin, BaseEstimator, ABC):
    def fit(self: _BaseNMF_Self, X: MatrixLike | ArrayLike, y: Any = None, **params) -> _BaseNMF_Self: ...


class _BinMapper(TransformerMixin, BaseEstimator):
    def fit(self: _BinMapper_Self, X: MatrixLike, y=None) -> _BinMapper_Self: ...


class _BaseFilter(SelectorMixin, BaseEstimator):
    def fit(self: _BaseFilter_Self, X: MatrixLike, y: ArrayLike) -> _BaseFilter_Self: ...


class _BinaryGaussianProcessClassifierLaplace(BaseEstimator):
    def fit(
        self: _BinaryGaussianProcessClassifierLaplace_Self,
        X: MatrixLike | ArrayLike,
        y: ArrayLike,
    ) -> _BinaryGaussianProcessClassifierLaplace_Self: ...


class _GeneralizedLinearRegressor(RegressorMixin, BaseEstimator):
    def fit(
        self: _GeneralizedLinearRegressor_Self,
        X: MatrixLike | ArrayLike,
        y: ArrayLike,
        sample_weight: None | ArrayLike = None,
    ) -> _GeneralizedLinearRegressor_Self: ...


class _RidgeGCV(LinearModel):
    def fit(
        self: _RidgeGCV_Self,
        X: MatrixLike,
        y: MatrixLike | ArrayLike,
        sample_weight: float | None | ArrayLike = None,
    ) -> _RidgeGCV_Self: ...


class _MultiOutputEstimator(MetaEstimatorMixin, BaseEstimator, metaclass=ABCMeta):
    def partial_fit(
        self: _MultiOutputEstimator_Self,
        X: MatrixLike | ArrayLike,
        y: MatrixLike,
        classes: Sequence[ArrayLike] | None = None,
        sample_weight: None | ArrayLike = None,
    ) -> _MultiOutputEstimator_Self: ...
```
(examples taken from https://github.com/astral-sh/ruff/issues/14183#issuecomment-2481832123 and https://github.com/microsoft/python-type-stubs/pull/340)

---

_Referenced in [microsoft/python-type-stubs#340](../../microsoft/python-type-stubs/pulls/340.md) on 2025-01-29 00:15_

---

_Label `fixes` added by @AlexWaygood on 2025-01-29 09:51_

---

_Referenced in [astral-sh/ruff#15821](../../astral-sh/ruff/pulls/15821.md) on 2025-01-30 00:09_

---

_Closed by @AlexWaygood on 2025-02-02 18:38_

---
