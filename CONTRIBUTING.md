# Contributing to YAKBD

Thank you for helping. YAKBD is an open-source keyboard layout designer. People design a keyboard layout once, in an
open file format, and YAKBD turns it into native, installable layouts for each operating system and keyboard tool,
and converts existing layouts from their formats. Its aim is that a custom or minority-language layout can be made,
shared and kept up to date by anyone, on every platform, without depending on a single vendor's tool. Which platforms
are supported today, and the documentation of every part, is on the website: https://yakbd.github.io/.

Questions, bug reports and pull requests are all welcome in every YAKBD repository. All projects are previews: the
format, the tool and the editor may still change between versions.

| repository | what it holds |
|---|---|
| [`yakbd/kbl-spec`](https://github.com/yakbd/kbl-spec) | the `.kbl.json` format: specification, JSON Schema, checker |
| [`yakbd/yakbd-cli`](https://github.com/yakbd/yakbd-cli) | the `yakbd` tool: converts layouts and builds them into installable layouts for each platform |
| [`yakbd/yakbd-gui`](https://github.com/yakbd/yakbd-gui) | the YAKBD Editor: a desktop editor for `.kbl.json` files (Qt 6, Windows, macOS, Linux) |

Questions about what a field means or what the schema accepts go to `yakbd/kbl-spec`; questions about converting,
building or installing a layout go to `yakbd/yakbd-cli`; questions about the editor's window, views and forms go
to `yakbd/yakbd-gui`. Each repository's issue templates ask for what is needed.

## How to propose a change

1. For anything larger than a typo, open an issue first and say what you want to change and why. A change to the
   format is a design decision and is discussed before it is written.
2. Fork the repository, make the change on a branch, and run the checks of that repository (below).
3. Open a pull request that says what changed, why, and how you checked it. Link the issue.
4. Each repository has an `AGENTS.md` with its rules for changing it, for people and coding agents alike. Read it
   before a larger change.

Contributions are licensed under MIT-0, the licence of every YAKBD repository (`LICENSE` in each). Third-party
material keeps its own licence and needs an entry in that repository's `NOTICE`. Describe CLDR in your own words
and link to it rather than copying its text.

## Clean room

YAKBD is written clean-room, and every contribution must keep it so. By opening a pull request you state that, for
what you contribute, you did **not** use:

- Microsoft source code, including leaked source, or the headers and samples of the Windows SDK or WDK;
- decompiled or disassembled code of any program, including Windows and MSKLC;
- leaked or confidential material of any kind;
- source code of Wine or ReactOS.

Every fact about Windows (layout DLLs, Windows behaviour, installation, `.klc` files) needs a **public source** you
cite (a public Microsoft documentation page, for example) or a **measurement**: a test on a Windows machine that you
describe well enough for someone else to repeat it. A fact you only remember is not enough; open an issue instead.
How each repository was written, and from which sources, is recorded in its `CLEAN-ROOM.md`.

For **macOS, Linux and every later platform other than Windows** the rule is different; the Windows rule above is
unchanged:

- **any public source may be read**: documentation, specifications, public repositories and source code of any
  licence (GPL included), public mirrors of headers, and reference tools for their behaviour;
- **nothing is copied**: no code and no long prose; write code and text from your own understanding. Short names
  and values a format requires (element names, keysym names, key codes) are facts and may be used;
- **every source you read is recorded** in the repository's `CLEAN-ROOM.md`: its URL (with a version, tag or commit
  where there is one), what you read it for, and its licence.

## `yakbd/kbl-spec`: the format

- **Spec question**: something in `SPEC.md` or a target document is unclear, missing or contradicts itself.
- **Schema bug**: `schema/0.1/kbl.schema.json` accepts a file the specification forbids, or rejects one it allows.
  `SPEC.md` wins over the schema, so such a difference is fixed in the schema.

Checks, on Node.js 22, from the repository root:

    npm ci                           # exact pins from package-lock.json
    npm test                         # tsc + schema freshness + checker; this is what CI runs

- Edit the schema source `src/schema.ts`, never the generated schema, and run `npm run gen`.
- A design decision becomes the next numbered entry of `DECISIONS.md`, with its reason; entries are never
  renumbered. A question for the maintainers goes to `OPEN-QUESTIONS.md`, with a recommendation.
- A change to the format's shape gets a line in the changelog of `STATUS.md`. What may change in 0.x, and how, is
  in [`COMPATIBILITY.md`](https://github.com/yakbd/kbl-spec/blob/main/COMPATIBILITY.md).

## `yakbd/yakbd-cli`: the tool

- **Layout bug**: a layout converts, builds, installs or types differently from what you expected (what to attach
  is below).
- **Tool bug**: a command crashes, hangs, rejects a valid file or writes a wrong one.
- **Question or idea**: anything else about the tool.

Checks, with Go 1.26 or later and nothing else (the tool uses the standard library only), from the repository root:

    gofmt -l .                       # must print nothing
    go vet ./...
    go test ./...                    # about 10 s; this is what CI runs
    go build -o /dev/null ./cmd/yakbd

Fuzz targets exist for every reader of outside input. CI runs only their seed inputs as part of `go test`; the
fuzzer runs locally, one target at a time, for example:

    go test -run='^$' -fuzz='^FuzzParse$' -fuzztime=10m -timeout 0 ./format/klc

The targets, their checks and how to keep a finding as a test are in
[`docs/fuzzing.md`](https://github.com/yakbd/yakbd-cli/blob/main/docs/fuzzing.md).

- Standard library only; no Python anywhere (checks are Go tests or Go commands under `tools/`).
- When you change a package the installer program uses, rebuild `installer/stub-386.exe` with
  `go run ./tools/buildstub` and commit it with the change.
- An intended change of test output is regenerated with `-update`, and the diff reviewed.
- Windows behaviour is tested in disposable Windows virtual machines with the bundles under `tools/`. A change that
  needs such a test says so in the pull request; a maintainer runs it.

### Reporting a layout bug

Please attach:

1. **The layout file**: the `.klc`, `.kbl.json` or CLDR `.xml` file, or the smallest file that still shows the
   problem. For a problem with a DLL you converted, the DLL, if you may share it.
2. **The report as JSON**: run the command again with `-report report.json`, for example
   `yakbd installer -report report.json my-layout.klc`, and attach `report.json`.
3. **The installer log**, for a problem when installing or uninstalling: `%TEMP%\yakbd-<dll name>-setup.log` on
   the Windows PC (paste `%TEMP%` into File Explorer's address bar), or the file you named with `/l*v log.txt`.
   An installer run as administrator from another account writes it into that account's `%TEMP%`.
4. **Windows version, build and architecture**: in Settings → System → About, the "Edition", "Version", "OS build"
   and "System type" lines (for example Windows 11 Pro, 25H2, 26200.8037, 64-bit x64 or ARM64), and the yakbd
   version (`yakbd version`).
5. **What you typed and what you got**: the keys (with Shift, AltGr, Caps Lock, dead keys) and the characters
   you expected and saw, and in which app.

The report contains file names and the layout's own text. The log also names Windows accounts by their security
id (SID) and the user folders it worked in. Look through both before you attach them, and remove what you do not
want to share.

## `yakbd/yakbd-gui`: the editor

The editor edits `.kbl.json` files and runs the `yakbd` tool for checking, converting and building, so a wrong
report, conversion or build is usually a `yakbd/yakbd-cli` bug; a problem with the window, the keyboard view, the
forms, saving or undo belongs here.

Build and test as the repository's [README](https://github.com/yakbd/yakbd-gui#build) says for your platform: Qt is
downloaded at the pinned version by `tools/getqt.cmake`, then `cmake`, `cmake --build` and
`ctest --test-dir build --output-on-failure` (the tests need the released `yakbd` CLI). What has been checked
where is in its `VERIFIED.md`.

### Reporting an editor bug

Please attach:

1. **Your system**: the operating system and its version (for example Windows 11 24H2, macOS 15.2, Ubuntu 24.04),
   and on Linux the desktop and session (for example GNOME on Wayland, KDE Plasma on X11).
2. **The editor and its style**: the editor's version (from its About box, or the release you downloaded) and the style it runs with (FluentWinUI3,
   macOS or Fusion, or what `QT_QUICK_CONTROLS_STYLE` names), and whether the app was in light or dark appearance.
3. **A screenshot** of the problem.
4. **What you did and what happened**: the steps, and the layout file if it matters (or the smallest one that still
   shows the problem).
