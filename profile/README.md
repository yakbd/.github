<p align="center">
  <img src="../icon.png" alt="YAKBD logo" width="240">
</p>

<h1 align="center">YAKBD</h1>

<p align="center">
  <strong>Y</strong>et <strong>A</strong>nother <strong>K</strong>ey<strong>B</strong>oard <em>layout</em> <strong>D</strong>esigner
</p>

<p align="center">
  Define a keyboard layout once. Understand what happens everywhere else.
</p>

---

YAKBD is a common source format and toolchain for software keyboard layouts. It aims to import existing layouts and generate the formats used by different operating systems and keyboard software.

### One definition, many targets

The YAKBD format is designed to express features found across keyboard layout systems, even when no single target supports them all. This gives a layout one place to be defined, edited, and maintained.

Converting between layout formats can be lossy. When a target cannot represent a feature, YAKBD aims to show what will change or be omitted, so the result can be reviewed before use.

The long-term goal is broad platform support: **write a layout once, then build it for wherever you need it.**
