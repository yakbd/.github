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

YAKBD is an open-source keyboard layout designer. People design a keyboard layout once, in an open file format, and YAKBD turns it into native, installable layouts for each operating system and keyboard tool, and converts existing layouts from their formats. Its aim is that a custom or minority-language layout can be made, shared and kept up to date by anyone, on every platform, without depending on a single vendor's tool.

The open format is [`.kbl.json`](https://github.com/yakbd/kbl-spec), a superset of Unicode CLDR Keyboard 3.0.

### Where it stands

- **Today: Windows.** The [`yakbd` tool](https://github.com/yakbd/yakbd-cli) reads and writes MSKLC `.klc` files and CLDR keyboard XML, reads installed Windows layout DLLs back, and builds native layout DLLs with one installer for every Windows architecture (x64, x86 and ARM64).
- **Next: other operating systems.** Their official formats: macOS `.keylayout`, Linux XKB, and others such as ChromeOS and Android where they have one.
- **Later: keyboard tools** such as Keyman.

Coming from Microsoft Keyboard Layout Creator? yakbd builds your `.klc` files as they are: see [migrating from MSKLC](https://github.com/yakbd/yakbd-cli/blob/main/docs/migrating-from-msklc.md).

### One definition, many targets

The `.kbl.json` format is designed to express features found across keyboard layout systems, even when no single target supports them all. A layout has one place to be defined, edited and maintained, and a section per target for the choices only that target needs.

Converting between layout formats can be lossy. When a target cannot represent a feature, YAKBD reports exactly what will change or be dropped, so the result can be reviewed before use.

The goal is broad platform support: **write a layout once, then build it for wherever you need it.**

### Repositories

- [`kbl-spec`](https://github.com/yakbd/kbl-spec): the `.kbl.json` format, its JSON Schema and a checker
- [`yakbd-cli`](https://github.com/yakbd/yakbd-cli): the `yakbd` command-line tool, which converts and builds layouts
- [`yakbd-gui`](https://github.com/yakbd/yakbd-gui): the YAKBD Editor, a desktop editor for Windows, macOS and Linux

All of it is a preview, licensed MIT-0. [Contributing](https://github.com/yakbd/.github/blob/main/CONTRIBUTING.md) explains how to help.
