  - **Y**et **A**nother **K**ey**B**oard *layout* **D**esigner

YAKBD is a common format and toolchain for defining software keyboard layouts.

Its source format is designed as a superset of the features offered by individual keyboard layout systems. From one definition, YAKBD aims to generate layouts for different operating systems and software keyboard solutions. It also aims to import existing layouts, making it easier to understand and migrate them.

When a target cannot represent part of a YAKBD definition, the toolchain should explain what will change or be lost. The goal is to make each conversion predictable and reviewable.

YAKBD's long-term aim is broad platform support: define a layout once, then produce the formats needed wherever that layout will be used.
