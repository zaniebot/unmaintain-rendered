```yaml
number: 1980
title: Large match/case on enum causes slowdown (exhaustiveness checks?)
type: issue
state: closed
author: indigoviolet
labels:
  - performance
  - enums
assignees: []
created_at: 2025-12-17T06:52:45Z
updated_at: 2025-12-17T21:40:42Z
url: https://github.com/astral-sh/ty/issues/1980
synced_at: 2026-01-12T15:54:26Z
```

# Large match/case on enum causes slowdown (exhaustiveness checks?)

---

_@indigoviolet_

### Summary

```py
from enum import StrEnum


class ExampleEnum(StrEnum):
    item_0 = "item_0"
    item_1 = "item_1"
    item_2 = "item_2"
    item_3 = "item_3"
    item_4 = "item_4"
    item_5 = "item_5"
    item_6 = "item_6"
    item_7 = "item_7"
    item_8 = "item_8"
    item_9 = "item_9"
    item_10 = "item_10"
    item_11 = "item_11"
    item_12 = "item_12"
    item_13 = "item_13"
    item_14 = "item_14"
    item_15 = "item_15"
    item_16 = "item_16"
    item_17 = "item_17"
    item_18 = "item_18"
    item_19 = "item_19"
    item_20 = "item_20"
    item_21 = "item_21"
    item_22 = "item_22"
    item_23 = "item_23"
    item_24 = "item_24"
    item_25 = "item_25"
    item_26 = "item_26"
    item_27 = "item_27"
    item_28 = "item_28"
    item_29 = "item_29"
    item_30 = "item_30"
    item_31 = "item_31"
    item_32 = "item_32"
    item_33 = "item_33"
    item_34 = "item_34"
    item_35 = "item_35"
    item_36 = "item_36"
    item_37 = "item_37"
    item_38 = "item_38"
    item_39 = "item_39"
    item_40 = "item_40"
    item_41 = "item_41"
    item_42 = "item_42"
    item_43 = "item_43"
    item_44 = "item_44"
    item_45 = "item_45"
    item_46 = "item_46"
    item_47 = "item_47"
    item_48 = "item_48"
    item_49 = "item_49"
    item_50 = "item_50"
    item_51 = "item_51"
    item_52 = "item_52"
    item_53 = "item_53"
    item_54 = "item_54"
    item_55 = "item_55"
    item_56 = "item_56"
    item_57 = "item_57"
    item_58 = "item_58"
    item_59 = "item_59"
    item_60 = "item_60"
    item_61 = "item_61"
    item_62 = "item_62"
    item_63 = "item_63"
    item_64 = "item_64"
    item_65 = "item_65"
    item_66 = "item_66"
    item_67 = "item_67"
    item_68 = "item_68"
    item_69 = "item_69"
    item_70 = "item_70"
    item_71 = "item_71"
    item_72 = "item_72"
    item_73 = "item_73"
    item_74 = "item_74"
    item_75 = "item_75"
    item_76 = "item_76"
    item_77 = "item_77"
    item_78 = "item_78"
    item_79 = "item_79"
    item_80 = "item_80"
    item_81 = "item_81"
    item_82 = "item_82"
    item_83 = "item_83"
    item_84 = "item_84"
    item_85 = "item_85"
    item_86 = "item_86"
    item_87 = "item_87"
    item_88 = "item_88"
    item_89 = "item_89"
    item_90 = "item_90"
    item_91 = "item_91"
    item_92 = "item_92"
    item_93 = "item_93"
    item_94 = "item_94"
    item_95 = "item_95"
    item_96 = "item_96"
    item_97 = "item_97"
    item_98 = "item_98"
    item_99 = "item_99"


def describe(doc: ExampleEnum) -> str:
    match doc:
        case ExampleEnum.item_0:
            return "Handled item_0"
        case ExampleEnum.item_1:
            return "Handled item_1"
        case ExampleEnum.item_2:
            return "Handled item_2"
        case ExampleEnum.item_3:
            return "Handled item_3"
        case ExampleEnum.item_4:
            return "Handled item_4"
        case ExampleEnum.item_5:
            return "Handled item_5"
        case ExampleEnum.item_6:
            return "Handled item_6"
        case ExampleEnum.item_7:
            return "Handled item_7"
        case ExampleEnum.item_8:
            return "Handled item_8"
        case ExampleEnum.item_9:
            return "Handled item_9"
        case ExampleEnum.item_10:
            return "Handled item_10"
        case ExampleEnum.item_11:
            return "Handled item_11"
        case ExampleEnum.item_12:
            return "Handled item_12"
        case ExampleEnum.item_13:
            return "Handled item_13"
        case ExampleEnum.item_14:
            return "Handled item_14"
        case ExampleEnum.item_15:
            return "Handled item_15"
        case ExampleEnum.item_16:
            return "Handled item_16"
        case ExampleEnum.item_17:
            return "Handled item_17"
        case ExampleEnum.item_18:
            return "Handled item_18"
        case ExampleEnum.item_19:
            return "Handled item_19"
        case ExampleEnum.item_20:
            return "Handled item_20"
        case ExampleEnum.item_21:
            return "Handled item_21"
        case ExampleEnum.item_22:
            return "Handled item_22"
        case ExampleEnum.item_23:
            return "Handled item_23"
        case ExampleEnum.item_24:
            return "Handled item_24"
        case ExampleEnum.item_25:
            return "Handled item_25"
        case ExampleEnum.item_26:
            return "Handled item_26"
        case ExampleEnum.item_27:
            return "Handled item_27"
        case ExampleEnum.item_28:
            return "Handled item_28"
        case ExampleEnum.item_29:
            return "Handled item_29"
        case ExampleEnum.item_30:
            return "Handled item_30"
        case ExampleEnum.item_31:
            return "Handled item_31"
        case ExampleEnum.item_32:
            return "Handled item_32"
        case ExampleEnum.item_33:
            return "Handled item_33"
        case ExampleEnum.item_34:
            return "Handled item_34"
        case ExampleEnum.item_35:
            return "Handled item_35"
        case ExampleEnum.item_36:
            return "Handled item_36"
        case ExampleEnum.item_37:
            return "Handled item_37"
        case ExampleEnum.item_38:
            return "Handled item_38"
        case ExampleEnum.item_39:
            return "Handled item_39"
        case ExampleEnum.item_40:
            return "Handled item_40"
        case ExampleEnum.item_41:
            return "Handled item_41"
        case ExampleEnum.item_42:
            return "Handled item_42"
        case ExampleEnum.item_43:
            return "Handled item_43"
        case ExampleEnum.item_44:
            return "Handled item_44"
        case ExampleEnum.item_45:
            return "Handled item_45"
        case ExampleEnum.item_46:
            return "Handled item_46"
        case ExampleEnum.item_47:
            return "Handled item_47"
        case ExampleEnum.item_48:
            return "Handled item_48"
        case ExampleEnum.item_49:
            return "Handled item_49"

    return 2

```

https://play.ty.dev/f3be0b59-0593-4ecc-8c23-0cea276ac514

The slowness gets worse as the number of `case` arms increases. 

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Label `performance` added by @AlexWaygood on 2025-12-17 07:31_

---

_Label `enums` added by @AlexWaygood on 2025-12-17 07:31_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-17 08:56_

---

_Comment by @indigoviolet on 2025-12-17 18:54_

Same issue as #1588 

---

_Comment by @carljm on 2025-12-17 21:40_

Thanks for the report!

---

_Closed by @carljm on 2025-12-17 21:40_

---
