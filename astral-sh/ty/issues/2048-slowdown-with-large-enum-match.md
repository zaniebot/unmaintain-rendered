```yaml
number: 2048
title: "Slowdown with large enum `match`"
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2025-12-18T02:06:57Z
updated_at: 2025-12-18T03:14:22Z
url: https://github.com/astral-sh/ty/issues/2048
synced_at: 2026-01-12T15:54:26Z
```

# Slowdown with large enum `match`

---

_@charliermarsh_

We received a report of a non-linear slowdown with the following:
```python
from enum import StrEnum

class BigEnum(StrEnum):
    VALUE_01 = "VALUE_01"
    VALUE_02 = "VALUE_02"
    VALUE_03 = "VALUE_03"
    VALUE_04 = "VALUE_04"
    VALUE_05 = "VALUE_05"
    VALUE_06 = "VALUE_06"
    VALUE_07 = "VALUE_07"
    VALUE_08 = "VALUE_08"
    VALUE_09 = "VALUE_09"
    VALUE_10 = "VALUE_10"
    VALUE_11 = "VALUE_11"
    VALUE_12 = "VALUE_12"
    VALUE_13 = "VALUE_13"
    VALUE_14 = "VALUE_14"
    VALUE_15 = "VALUE_15"
    VALUE_16 = "VALUE_16"
    VALUE_17 = "VALUE_17"
    VALUE_18 = "VALUE_18"
    VALUE_19 = "VALUE_19"
    VALUE_20 = "VALUE_20"
    VALUE_21 = "VALUE_21"
    VALUE_22 = "VALUE_22"
    VALUE_23 = "VALUE_23"
    VALUE_24 = "VALUE_24"
    VALUE_25 = "VALUE_25"
    VALUE_26 = "VALUE_26"
    VALUE_27 = "VALUE_27"
    VALUE_28 = "VALUE_28"
    VALUE_29 = "VALUE_29"
    VALUE_30 = "VALUE_30"
    VALUE_31 = "VALUE_31"
    VALUE_32 = "VALUE_32"
    VALUE_33 = "VALUE_33"
    VALUE_34 = "VALUE_34"
    VALUE_35 = "VALUE_35"
    VALUE_36 = "VALUE_36"
    VALUE_37 = "VALUE_37"
    VALUE_38 = "VALUE_38"
    VALUE_39 = "VALUE_39"
    VALUE_40 = "VALUE_40"
    VALUE_41 = "VALUE_41"
    VALUE_42 = "VALUE_42"
    VALUE_43 = "VALUE_43"
    VALUE_44 = "VALUE_44"
    VALUE_45 = "VALUE_45"
    VALUE_46 = "VALUE_46"
    VALUE_47 = "VALUE_47"
    VALUE_48 = "VALUE_48"
    VALUE_49 = "VALUE_49"
    VALUE_50 = "VALUE_50"
    VALUE_51 = "VALUE_51"
    VALUE_52 = "VALUE_52"
    VALUE_53 = "VALUE_53"
    VALUE_54 = "VALUE_54"
    VALUE_55 = "VALUE_55"
    VALUE_56 = "VALUE_56"
    VALUE_57 = "VALUE_57"
    VALUE_58 = "VALUE_58"
    VALUE_59 = "VALUE_59"
    VALUE_60 = "VALUE_60"
    VALUE_61 = "VALUE_61"
    VALUE_62 = "VALUE_62"
    VALUE_63 = "VALUE_63"
    VALUE_64 = "VALUE_64"
    VALUE_65 = "VALUE_65"
    VALUE_66 = "VALUE_66"
    VALUE_67 = "VALUE_67"
    VALUE_68 = "VALUE_68"
    VALUE_69 = "VALUE_69"
    VALUE_70 = "VALUE_70"
    VALUE_71 = "VALUE_71"
    VALUE_72 = "VALUE_72"
    VALUE_73 = "VALUE_73"
    VALUE_74 = "VALUE_74"
    VALUE_75 = "VALUE_75"
    VALUE_76 = "VALUE_76"
    VALUE_77 = "VALUE_77"
    VALUE_78 = "VALUE_78"
    VALUE_79 = "VALUE_79"
    VALUE_80 = "VALUE_80"
    VALUE_81 = "VALUE_81"
    VALUE_82 = "VALUE_82"
    VALUE_83 = "VALUE_83"
    VALUE_84 = "VALUE_84"
    VALUE_85 = "VALUE_85"
    VALUE_86 = "VALUE_86"
    VALUE_87 = "VALUE_87"
    VALUE_88 = "VALUE_88"
    VALUE_89 = "VALUE_89"
    VALUE_90 = "VALUE_90"
    VALUE_91 = "VALUE_91"
    VALUE_92 = "VALUE_92"
    VALUE_93 = "VALUE_93"
    VALUE_94 = "VALUE_94"
    VALUE_95 = "VALUE_95"
    VALUE_96 = "VALUE_96"
    VALUE_97 = "VALUE_97"
    VALUE_98 = "VALUE_98"
    VALUE_99 = "VALUE_99"

    def get_info(self) -> tuple[str, int]:
        match self:
            case BigEnum.VALUE_01:
                return self.value, 1
            case BigEnum.VALUE_02:
                return self.value, 2
            case BigEnum.VALUE_03:
                return self.value, 3
            case BigEnum.VALUE_04:
                return self.value, 4
            case BigEnum.VALUE_05:
                return self.value, 5
            case BigEnum.VALUE_06:
                return self.value, 6
            case BigEnum.VALUE_07:
                return self.value, 7
            case BigEnum.VALUE_08:
                return self.value, 8
            case BigEnum.VALUE_09:
                return self.value, 9
            case BigEnum.VALUE_10:
                return self.value, 10
            case BigEnum.VALUE_11:
                return self.value, 11
            case BigEnum.VALUE_12:
                return self.value, 12
            case BigEnum.VALUE_13:
                return self.value, 13
            case BigEnum.VALUE_14:
                return self.value, 14
            case BigEnum.VALUE_15:
                return self.value, 15
            case BigEnum.VALUE_16:
                return self.value, 16
            case BigEnum.VALUE_17:
                return self.value, 17
            case BigEnum.VALUE_18:
                return self.value, 18
            case BigEnum.VALUE_19:
                return self.value, 19
            case BigEnum.VALUE_20:
                return self.value, 20
            case BigEnum.VALUE_21:
                return self.value, 21
            case BigEnum.VALUE_22:
                return self.value, 22
            case BigEnum.VALUE_23:
                return self.value, 23
            case BigEnum.VALUE_24:
                return self.value, 24
            case BigEnum.VALUE_25:
                return self.value, 25
            case BigEnum.VALUE_26:
                return self.value, 26
            case BigEnum.VALUE_27:
                return self.value, 27
            case BigEnum.VALUE_28:
                return self.value, 28
            case BigEnum.VALUE_29:
                return self.value, 29
            case BigEnum.VALUE_30:
                return self.value, 30
            case BigEnum.VALUE_31:
                return self.value, 31
            case BigEnum.VALUE_32:
                return self.value, 32
            case BigEnum.VALUE_33:
                return self.value, 33
            case BigEnum.VALUE_34:
                return self.value, 34
            case BigEnum.VALUE_35:
                return self.value, 35
            case BigEnum.VALUE_36:
                return self.value, 36
            case BigEnum.VALUE_37:
                return self.value, 37
            case BigEnum.VALUE_38:
                return self.value, 38
            case BigEnum.VALUE_39:
                return self.value, 39
            case BigEnum.VALUE_40:
                return self.value, 40
            case BigEnum.VALUE_41:
                return self.value, 41
            case BigEnum.VALUE_42:
                return self.value, 42
            case BigEnum.VALUE_43:
                return self.value, 43
            case BigEnum.VALUE_44:
                return self.value, 44
            case BigEnum.VALUE_45:
                return self.value, 45
            case BigEnum.VALUE_46:
                return self.value, 46
            case BigEnum.VALUE_47:
                return self.value, 47
            case BigEnum.VALUE_48:
                return self.value, 48
            case BigEnum.VALUE_49:
                return self.value, 49
            case BigEnum.VALUE_50:
                return self.value, 50
            case BigEnum.VALUE_51:
                return self.value, 51
            case BigEnum.VALUE_52:
                return self.value, 52
            case BigEnum.VALUE_53:
                return self.value, 53
            case BigEnum.VALUE_54:
                return self.value, 54
            case BigEnum.VALUE_55:
                return self.value, 55
            case BigEnum.VALUE_56:
                return self.value, 56
            case BigEnum.VALUE_57:
                return self.value, 57
            case BigEnum.VALUE_58:
                return self.value, 58
            case BigEnum.VALUE_59:
                return self.value, 59
            case BigEnum.VALUE_60:
                return self.value, 60
            case BigEnum.VALUE_61:
                return self.value, 61
            case BigEnum.VALUE_62:
                return self.value, 62
            case BigEnum.VALUE_63:
                return self.value, 63
            case BigEnum.VALUE_64:
                return self.value, 64
            case BigEnum.VALUE_65:
                return self.value, 65
            case BigEnum.VALUE_66:
                return self.value, 66
            case BigEnum.VALUE_67:
                return self.value, 67
            case BigEnum.VALUE_68:
                return self.value, 68
            case BigEnum.VALUE_69:
                return self.value, 69
            case BigEnum.VALUE_70:
                return self.value, 70
            case BigEnum.VALUE_71:
                return self.value, 71
            case BigEnum.VALUE_72:
                return self.value, 72
            case BigEnum.VALUE_73:
                return self.value, 73
            case BigEnum.VALUE_74:
                return self.value, 74
            case BigEnum.VALUE_75:
                return self.value, 75
            case BigEnum.VALUE_76:
                return self.value, 76
            case BigEnum.VALUE_77:
                return self.value, 77
            case BigEnum.VALUE_78:
                return self.value, 78
            case BigEnum.VALUE_79:
                return self.value, 79
            case BigEnum.VALUE_80:
                return self.value, 80
            case BigEnum.VALUE_81:
                return self.value, 81
            case BigEnum.VALUE_82:
                return self.value, 82
            case BigEnum.VALUE_83:
                return self.value, 83
            case BigEnum.VALUE_84:
                return self.value, 84
            case BigEnum.VALUE_85:
                return self.value, 85
            case BigEnum.VALUE_86:
                return self.value, 86
            case BigEnum.VALUE_87:
                return self.value, 87
            case BigEnum.VALUE_88:
                return self.value, 88
            case BigEnum.VALUE_89:
                return self.value, 89
            case BigEnum.VALUE_90:
                return self.value, 90
            case BigEnum.VALUE_91:
                return self.value, 91
            case BigEnum.VALUE_92:
                return self.value, 92
            case BigEnum.VALUE_93:
                return self.value, 93
            case BigEnum.VALUE_94:
                return self.value, 94
            case BigEnum.VALUE_95:
                return self.value, 95
            case BigEnum.VALUE_96:
                return self.value, 96
            case BigEnum.VALUE_97:
                return self.value, 97
            case BigEnum.VALUE_98:
                return self.value, 98
            case BigEnum.VALUE_99:
                return self.value, 99
```

---

_Label `performance` added by @charliermarsh on 2025-12-18 02:07_

---

_Comment by @charliermarsh on 2025-12-18 03:14_

Closed by https://github.com/astral-sh/ruff/pull/22045.

---

_Closed by @charliermarsh on 2025-12-18 03:14_

---
